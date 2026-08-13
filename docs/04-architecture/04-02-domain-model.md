# ⚽ Modelo de Dominio (Domain Model)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento describe el modelo conceptual de **Fulbito**. Define las entidades principales del dominio, sus responsabilidades y las relaciones existentes entre ellas.

El objetivo del modelo de dominio es representar el funcionamiento del negocio de forma independiente de la tecnología utilizada para implementarlo. Por este motivo, no se describen detalles propios de Firebase, Firestore o la estructura física de la base de datos.

---

# 1. Diagrama Conceptual

```text
                 Usuario
                    │
              pertenece a
                    │
                    ▼
                 Jugador
                    │
                    │ pertenece a
                    ▼
                  Grupo
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
   Invitación               Partido
                                 │
                     ┌───────────┴───────────┐
                     ▼                       ▼
                 Equipo A                Equipo B
                     │                       │
                     └───────────┬───────────┘
                                 ▼
                       JugadorEnPartido
                                 │
                                 ▼
                           Evento de Partido
                          /        |        \
                        Gol   Asistencia  Autogol
                                 │
                                 ▼
                     Solicitud de Corrección
                                 │
                                 ▼
                              Votación
```

---

# 2. Núcleo Social

## Usuario

Representa a una persona registrada en la plataforma.

- **Atributos clave:** ID de usuario, correo electrónico, nombre, apodo y foto de perfil.
- **Responsabilidad:** Es la identidad global del sistema. Un usuario puede participar simultáneamente en múltiples grupos.

---

## Grupo

Representa un grupo privado de jugadores.

Todo el historial deportivo ocurre dentro del contexto de un grupo.

- **Atributos clave:** ID de grupo, nombre, imagen, modalidad de juego predeterminada.
- **Responsabilidad:** Contener los integrantes, partidos, historial y estadísticas del grupo.

---

## Invitación

Representa el mecanismo mediante el cual nuevos usuarios pueden incorporarse a un grupo.

- **Atributos clave:** Código o enlace único, estado (activo o revocado).
- **Responsabilidad:** Permitir el acceso controlado de nuevos integrantes.

---

## Jugador (Miembro del Grupo)

Representa la participación de un usuario dentro de un grupo determinado.

- **Atributos clave:** ID de usuario e ID de grupo.
- **Responsabilidad:** Vincular al usuario con un grupo específico y servir como referencia para la participación en partidos y el cálculo de estadísticas.

> **Nota:** Esta entidad no posee un atributo de rol jerárquico (como "administrador"). Todos los Jugadores de un mismo Grupo tienen el mismo nivel de permisos. Las pocas acciones críticas del sistema (expulsión de un integrante, eliminación del grupo) se resuelven mediante votación colectiva, no mediante un rol individual. Ver `03-requirements/07-roles-permissions.md`.

> **Nota:** Las estadísticas del jugador no constituyen información propia de esta entidad. Son datos derivados del historial de partidos del grupo.

---

# 3. Núcleo Deportivo

## Partido

Representa un encuentro deportivo perteneciente a un grupo.

- **Atributos clave:** ID de partido, fecha, modalidad, estado y marcador.
- **Estados posibles:** Programado, En Curso y Finalizado.
- **Responsabilidad:** Organizar el encuentro y contener todos los eventos ocurridos durante su desarrollo.

---

## JugadorEnPartido

Representa la participación de un jugador en un partido específico.

- **Atributos clave:** Estado de confirmación (Voy, No Voy o Pendiente) y equipo asignado.
- **Responsabilidad:** Determinar quién participa del partido y a qué equipo pertenece.

---

## Equipo

Representa uno de los dos equipos que disputan un partido.

- **Atributos clave:** Nombre, color (opcional) y marcador.
- **Responsabilidad:** Agrupar a los jugadores participantes y acumular el resultado del encuentro.

---

# 4. Eventos del Partido

## Evento de Partido

Representa cualquier acción relevante ocurrida durante un partido en estado **En Curso**.

- **Atributos clave:** ID del evento, instante de ocurrencia e ID del jugador involucrado (opcional en el caso de un gol sin autor identificado).

De esta entidad derivan distintos tipos de eventos.

### Gol

Representa un gol convertido por un jugador.

- Incrementa el marcador de su equipo.
- Puede tener asociada una asistencia.
- El jugador anotador es opcional: puede registrarse un gol sin autor identificado cuando no se recuerda con certeza quién lo convirtió.

---

### Asistencia

Representa el pase previo a un gol.

- Se vincula exactamente a un gol.
- Debe pertenecer a un jugador distinto del goleador.

---

### Autogol

Representa un gol convertido en la propia portería.

- Incrementa el marcador del equipo rival.
- Queda registrado como evento independiente.

---

# 5. Correcciones

## Solicitud de Corrección

Representa una propuesta para modificar un partido ya finalizado.

- **Atributos clave:** ID, partido asociado, solicitante, descripción y estado (Pendiente, Aprobada o Rechazada).
- **Responsabilidad:** Evitar modificaciones directas sobre el historial y registrar formalmente la propuesta de cambio.

---

## Votación

Representa el mecanismo mediante el cual los integrantes del grupo aprueban o rechazan una solicitud de corrección.

- **Atributos clave:** Votos a favor, votos en contra y criterio de aprobación.
- **Responsabilidad:** Determinar si la corrección debe aplicarse y disparar el recálculo de las estadísticas correspondientes.

---

# 6. Estadísticas

## Estadística

Representa los indicadores deportivos obtenidos a partir del historial de partidos de un grupo.

Las estadísticas no constituyen una entidad independiente del negocio, sino una representación derivada de los partidos finalizados.

Ejemplos de estadísticas:

- Partidos jugados
- Victorias
- Empates
- Derrotas
- Goles
- Asistencias
- Racha
- Promedio de gol

---

# 7. Relaciones Principales

- Un **Usuario** puede pertenecer a múltiples **Grupos**.
- Un **Grupo** contiene múltiples **Jugadores**.
- Un **Grupo** posee múltiples **Partidos**.
- Un **Grupo** puede generar múltiples **Invitaciones**.
- Un **Partido** pertenece a un único **Grupo**.
- Un **Partido** posee exactamente dos **Equipos**.
- Un **Partido** posee múltiples **JugadoresEnPartido**.
- Un **JugadorEnPartido** pertenece a un único **Equipo** dentro de ese partido.
- Un **Partido** contiene múltiples **Eventos de Partido**.
- Un **Evento de Partido** está asociado a un único jugador participante del partido.
- Un **Gol** puede tener asociada cero o una **Asistencia**.
- Un **Partido Finalizado** puede tener múltiples **Solicitudes de Corrección**.
- Cada **Solicitud de Corrección** posee una única **Votación**.

---

> **Nota de Diseño**
>
> La separación entre **Usuario** y **Jugador (Miembro del Grupo)** permite que una misma persona participe simultáneamente en distintos grupos sin compartir estadísticas entre ellos. Cada grupo mantiene su propio historial y sus propios indicadores deportivos de manera completamente independiente.