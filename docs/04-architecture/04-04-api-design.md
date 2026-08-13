# ⚽ Diseño de la API (API / Firebase Actions Design)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Aunque **Fulbito** utiliza **Cloud Firestore** y **Firebase Authentication** mediante sus SDKs nativos, documentar las operaciones siguiendo un paradigma REST constituye una buena práctica de diseño.

Este documento modela las principales interacciones entre el cliente móvil y el backend mediante **endpoints conceptuales**, independientemente de la implementación real basada en Firebase. Su objetivo es servir como contrato funcional para el desarrollo y facilitar la comprensión del flujo de datos.

> **Nota:** En la implementación real, las operaciones de lectura se realizan mediante consultas (`getDoc`, `getDocs`) o listeners en tiempo real (`onSnapshot`) proporcionados por el SDK de Firebase.

---

# 1. Autenticación e Identidad (Firebase Authentication)

Estas operaciones son gestionadas por Firebase Authentication.

| Método | Endpoint (Conceptual) | Descripción | Procesado por | Payload |
| :----- | :-------------------- | :---------- | :------------ | :------ |
| **POST** | `/auth/anonymous` | Crea una sesión anónima automáticamente al abrir la aplicación por primera vez. | Firebase Auth | — |
| **POST** | `/auth/link` | Vincula la sesión anónima actual a una credencial permanente, conservando el `uid`. | Firebase Auth | `email`/`password`, o credencial de Google/Apple |
| **POST** | `/auth/login` | Inicia sesión con una cuenta completa ya existente (por ejemplo, desde otro dispositivo). | Firebase Auth | `email`, `password` u OAuth |
| **POST** | `/auth/logout` | Cierra la sesión actual. | Firebase Auth | — |
| **PATCH** | `/users/{userId}` | Actualiza el perfil del usuario. | Cliente → Firestore | `nickname`, `avatarUrl`, `fcmTokens` |

> **Nota:** `/auth/anonymous` se invoca automáticamente al abrir la aplicación por primera vez, sin ninguna acción del usuario (ver FR-001). `/auth/link` corresponde al flujo opcional de vinculación de cuenta (ver FR-002 y CU-001-a): el `uid` original se conserva, por lo que no es necesario migrar datos entre usuarios.

---

# 2. Grupos e Invitaciones

Operaciones sobre la colección `/groups`.

| Método | Endpoint | Descripción | Procesado por | Payload |
| :----- | :------- | :---------- | :------------ | :------ |
| **POST** | `/groups` | Crea un nuevo grupo. | Cliente → Firestore | `name`, `defaultGameMode`, `imageUrl` |
| **GET** | `/users/{userId}/groups` | Obtiene los grupos del usuario. | Firestore | — |
| **GET** | `/groups/{groupId}` | Obtiene la información del grupo. | Firestore | — |
| **PATCH** | `/groups/{groupId}` | Modifica los datos del grupo. | Cliente → Firestore | `name`, `defaultGameMode`, `imageUrl` |
| **POST** | `/groups/{groupId}/invitations` | Genera un nuevo código de invitación. | Cliente → Firestore | `createdBy` |
| **POST** | `/groups/join` | Une un usuario a un grupo mediante un código válido. | Cloud Function | `code` |

---

# 3. Partidos y Confirmación de Asistencia (RSVP)

Operaciones sobre `/groups/{groupId}/matches`.

| Método | Endpoint | Descripción | Procesado por | Payload |
| :----- | :------- | :---------- | :------------ | :------ |
| **POST** | `.../matches` | Crea un nuevo partido. | Cliente → Firestore | `date`, `mode` |
| **GET** | `.../matches` | Obtiene el historial de partidos. | Firestore | Filtros opcionales |
| **GET** | `.../matches/{matchId}` | Obtiene el detalle del partido. | Firestore | — |
| **PATCH** | `.../matches/{matchId}` | Actualiza datos del partido. | Cliente → Firestore | `status`, marcador |
| **PATCH** | `.../matches/{matchId}/rsvp` | Actualiza la asistencia de un jugador. | Cliente → Firestore | `status` |
| **PATCH** | `.../matches/{matchId}/teams` | Guarda la distribución de equipos. | Cliente → Firestore | Asignaciones |

---

# 4. Eventos del Partido

Operaciones sobre `/groups/{groupId}/matches/{matchId}/events`.

| Método | Endpoint | Descripción | Procesado por | Payload |
| :----- | :------- | :---------- | :------------ | :------ |
| **POST** | `.../events` | Registra un evento del partido. | Cliente → Firestore | `type`, `playerId`, `team`, `timestamp` |
| **DELETE** | `.../events/{eventId}` | Elimina un evento registrado por error. | Cliente → Firestore | — |

> **Flujo para registrar un gol con asistencia**
>
> 1. Registrar un evento de tipo **Gol**.
> 2. Registrar un evento de tipo **Asistencia**, referenciando el gol mediante `linkedEventId`.

---

# 5. Estadísticas y Ranking

Operaciones sobre `/groups/{groupId}/members`.

| Método | Endpoint | Descripción | Procesado por | Payload |
| :----- | :------- | :---------- | :------------ | :------ |
| **GET** | `.../members` | Obtiene el ranking del grupo. | Firestore | `orderBy`, `limit` |
| **GET** | `.../members/{userId}` | Obtiene el perfil deportivo de un jugador. | Firestore | — |
| **DELETE** | `.../members/{userId}` | Elimina al jugador del grupo. | Cliente → Firestore | — |

> **Importante:** Las estadísticas de los jugadores nunca son modificadas directamente por el cliente. Su actualización es realizada automáticamente mediante **Cloud Functions** cuando un partido pasa al estado **Finalizado**.

---

# 6. Correcciones y Votaciones

Operaciones sobre `/groups/{groupId}/correction_requests`.

| Método | Endpoint | Descripción | Procesado por | Payload |
| :----- | :------- | :---------- | :------------ | :------ |
| **POST** | `.../correction_requests` | Solicita corregir un partido. | Cliente → Firestore | `matchId`, `description` |
| **GET** | `.../correction_requests` | Lista solicitudes pendientes. | Firestore | `status` |
| **POST** | `.../{requestId}/vote` | Emite un voto sobre una solicitud. | Cliente → Firestore | `vote` |

---

# 7. Operaciones Automáticas del Backend

Las siguientes acciones no son invocadas directamente por el cliente, sino que son ejecutadas automáticamente mediante **Cloud Functions**.

| Evento disparador | Acción automática |
| :---------------- | :---------------- |
| Partido cambia a **Finalizado** | Recalcular estadísticas individuales y grupales |
| Corrección aprobada | Actualizar el partido y recalcular estadísticas |
| Creación de un nuevo partido | Enviar notificaciones push al grupo |
| Creación de una invitación | Generar un código único de acceso |

---

# 8. Respuestas Conceptuales

Aunque Firestore no utiliza respuestas HTTP tradicionales, conceptualmente cada operación devuelve uno de los siguientes recursos.

| Operación | Recurso devuelto |
| :-------- | :--------------- |
| Crear grupo | `Group` |
| Obtener grupos | `Group[]` |
| Crear partido | `Match` |
| Obtener historial | `Match[]` |
| Registrar evento | `Event` |
| Obtener ranking | `Member[]` |
| Obtener jugador | `Member` |
| Crear solicitud | `CorrectionRequest` |

---

# 9. Errores Conceptuales

Las siguientes respuestas representan los posibles resultados de una operación.

| Código | Significado |
| :----- | :---------- |
| **400** | Solicitud inválida |
| **401** | Usuario no autenticado |
| **403** | Usuario sin permisos suficientes |
| **404** | Recurso inexistente |
| **409** | Conflicto de datos |
| **500** | Error interno del servidor |

---

# 10. Convenciones

- Los identificadores (`userId`, `groupId`, `matchId`, etc.) corresponden a documentos de Cloud Firestore.
- Las fechas se almacenan utilizando `Timestamp` de Firebase.
- Las consultas en tiempo real utilizan `onSnapshot()`.
- Las consultas puntuales utilizan `getDoc()` o `getDocs()`.
- Las modificaciones parciales utilizan `updateDoc()`.
- La creación de documentos utiliza `addDoc()` o `setDoc()`.
- La eliminación utiliza `deleteDoc()`.
- Las operaciones críticas son protegidas mediante **Firestore Security Rules** y, cuando corresponde, procesadas por **Cloud Functions**.