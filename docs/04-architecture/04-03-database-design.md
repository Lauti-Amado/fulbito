# ⚽ Diseño de la Base de Datos (Database Design)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento detalla la estructura física de los datos para el MVP de **Fulbito**, implementada sobre **Cloud Firestore** (base de datos NoSQL orientada a documentos). 

El diseño traduce el Modelo de Dominio a colecciones y subcolecciones, priorizando la velocidad de lectura, la sincronización en tiempo real y la escalabilidad. Se aplica desnormalización controlada (duplicar ciertos datos) para evitar consultas complejas y costosas, apoyando la actualización de los mismos mediante Cloud Functions.

---

# 1. Estructura General del Árbol de Datos

En Firestore, la información se organiza jerárquicamente. La raíz de la base de datos contendrá las colecciones globales, de las cuales se desprenden subcolecciones específicas para aislar la información de cada grupo.

```text
/
├── users/ (Colección raíz)
│
└── groups/ (Colección raíz)
    │
    ├── members/ (Subcolección)
    │
    ├── invitations/ (Subcolección)
    │
    ├── matches/ (Subcolección)
    │   │
    │   └── events/ (Subcolección anidada)
    │
    └── correction_requests/ (Subcolección)
```

---

# 2. Esquema de Colecciones y Documentos

A continuación se detalla la estructura JSON representativa de cada documento dentro de las colecciones.

## 2.1. Colección `users`
Almacena la información global de las personas registradas.

**Ruta:** `/users/{userId}`

```json
{
  "id": "u123456",
  "isAnonymous": true,
  "authProvider": null,
  "email": null,
  "displayName": "Jugador",
  "nickname": "Lauti",
  "avatarUrl": "https://firebasestorage...",
  "fcmTokens": ["token_dispositivo_1", "token_dispositivo_2"],
  "createdAt": "2026-08-05T14:00:00Z"
}
```
> **Nota:** `fcmTokens` es un arreglo para poder enviar notificaciones push a múltiples dispositivos del mismo usuario.

> **Nota:** `isAnonymous` indica si el usuario todavía no vinculó su sesión a un método de autenticación permanente. `authProvider` permanece `null` mientras la sesión sea anónima, y toma valores como `"password"`, `"google.com"` o `"apple.com"` una vez vinculada. Al vincularse una cuenta, el documento se actualiza in-place (mismo `id`/`uid`), preservando todas las referencias existentes en `groups/{groupId}/members`.

---

## 2.2. Colección `groups`
Almacena los metadatos de los grupos privados.

**Ruta:** `/groups/{groupId}`

```json
{
  "id": "g987654",
  "name": "Los del Barrio",
  "imageUrl": "https://firebasestorage...",
  "defaultGameMode": "F5",
  "createdAt": "2026-08-06T10:00:00Z"
}
```

---

## 2.3. Subcolección `members` (Jugador)
Representa a un usuario dentro del contexto de un grupo. Aquí se almacenan (cacheadas) las estadísticas derivadas para que la lectura del ranking sea instantánea, actualizadas vía Cloud Functions.

**Ruta:** `/groups/{groupId}/members/{userId}`

```json
{
  "userId": "u123456",
  "joinedAt": "2026-08-06T10:05:00Z",
  "stats": {
    "matchesPlayed": 15,
    "wins": 10,
    "draws": 2,
    "losses": 3,
    "goals": 24,
    "assists": 8,
    "ownGoals": 0,
    "streak": ["V", "V", "E", "D", "V"] 
  }
}
```

---

## 2.4. Subcolección `matches`
Contiene la información de los encuentros. Los equipos y sus marcadores se desnormalizan dentro de este documento para facilitar la carga de la vista del historial.

**Ruta:** `/groups/{groupId}/matches/{matchId}`

```json
{
  "id": "m111222",
  "date": "2026-08-07T21:00:00Z",
  "mode": "F5",
  "status": "finished", 
  "teamA": {
    "name": "Equipo A",
    "color": "#FF0000",
    "score": 5
  },
  "teamB": {
    "name": "Equipo B",
    "color": "#0000FF",
    "score": 3
  },
  "players": {
    "u123456": { "rsvp": "voy", "team": "teamA" },
    "u999888": { "rsvp": "voy", "team": "teamB" },
    "u777666": { "rsvp": "no_voy", "team": null }
  }
}
```
> **Nota:** El mapa `players` gestiona simultáneamente el RSVP (quién va) y la asignación de equipos (`teamA`, `teamB`), facilitando las reglas de negocio de asistencia y armado automático.

---

## 2.5. Subcolección `events`
Almacena los eventos específicos de un partido (goles, asistencias, etc.). Se utiliza una subcolección para evitar que el documento del partido crezca indefinidamente y para facilitar el ordenamiento cronológico.

**Ruta:** `/groups/{groupId}/matches/{matchId}/events/{eventId}`

```json
{
  "id": "e555444",
  "type": "goal", 
  "playerId": "u123456",
  "team": "teamA",
  "timestamp": "2026-08-07T21:15:30Z",
  "linkedEventId": null 
}
```
*Ejemplo de una asistencia vinculada al gol anterior:*
```json
{
  "id": "e555445",
  "type": "assist",
  "playerId": "u999888",
  "team": "teamA",
  "timestamp": "2026-08-07T21:15:31Z",
  "linkedEventId": "e555444" 
}
```

> **Nota:** El campo `playerId` es **nullable** en eventos de tipo `goal`. Un gol puede registrarse sin autor identificado (`"playerId": null`). En cambio, para eventos de tipo `assist`, `playerId` es obligatorio, y solo puede registrarse una asistencia cuando el gol vinculado tiene un `playerId` no nulo.

---

## 2.6. Subcolección `correction_requests`
Almacena las solicitudes de modificación sobre partidos finalizados y el estado de la votación colectiva.

**Ruta:** `/groups/{groupId}/correction_requests/{requestId}`

```json
{
  "id": "cr777888",
  "matchId": "m111222",
  "requestedBy": "u123456",
  "description": "El segundo gol del Equipo A fue mío, no de Juan.",
  "status": "pending",
  "createdAt": "2026-08-08T10:00:00Z",
  "voting": {
    "eligibleVoters": ["u123456", "u999888", "u777666", "u222333"],
    "votesFor": ["u123456", "u999888"],
    "votesAgainst": [],
    "requiredToPass": 3
  }
}
```

> **Nota:** `eligibleVoters` corresponde a los jugadores que participaron del partido en cuestión (no a todo el grupo). `requiredToPass` se calcula dinámicamente al crear la solicitud mediante una Cloud Function, aplicando la fórmula `floor(eligibleVoters.length / 2) + 1` (mayoría simple). El detalle completo está en `03-requirements/03-08-voting-rules.md`.

---

## 2.7. Subcolección `invitations`
Contiene los códigos hash temporales o permanentes para que los usuarios se unan al grupo.

**Ruta:** `/groups/{groupId}/invitations/{invitationId}`

```json
{
  "code": "F5-X7K9-PQ2M-8R4T",
  "createdBy": "u123456",
  "status": "active",
  "createdAt": "2026-08-05T14:30:00Z"
}
```

---

# 3. Decisiones de Diseño (Trade-offs)

1. **Estadísticas Cacheadas vs. Calculadas:** 
   * *Decisión:* Las estadísticas (goles totales, racha) se guardan directamente en el documento del `member`.
   * *Razón:* Si calculáramos las estadísticas leyendo todos los partidos cada vez que un usuario abre el ranking, el costo de lectura en Firebase se dispararía y la app sería lenta. Una Cloud Function actualiza este documento cada vez que un partido pasa a estado `finished`.
2. **Eventos en Subcolección:**
   * *Decisión:* Los goles no son un simple array dentro del documento de `matches`, sino una subcolección `events`.
   * *Razón:* Permite escuchar en tiempo real (realtime listeners) únicamente los eventos nuevos agregados durante el partido, reduciendo el consumo de datos en la red del celular, optimizando el rendimiento.
3. **Ausencia de Roles Jerárquicos:**
   * *Decisión:* El documento de `members` no posee un campo `role`. Todos los integrantes tienen el mismo nivel de permisos.
   * *Razón:* Fulbito es un producto horizontal por diseño (ver Visión y Reglas de Negocio). Las acciones consideradas críticas (expulsar integrantes, eliminar el grupo) no dependen de un rol individual: se resuelven mediante votación colectiva y son ejecutadas por Cloud Functions únicamente cuando se alcanza el criterio de aprobación definido en las Reglas de Votación.