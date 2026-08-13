# ⚽ Requisitos Funcionales

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento detalla las funciones específicas que el sistema (MVP) debe ser capaz de realizar. Define qué puede hacer el usuario dentro de Fulbito.

---

### FR-001 — Iniciar sesión anónima automáticamente

- **Descripción:** El sistema deberá crear automáticamente una sesión anónima para todo usuario que abra la aplicación por primera vez, sin requerir ninguna acción manual ni mostrar una pantalla de login previa.
- **Prioridad:** Alta
- **Actor:** Usuario nuevo (sin sesión previa)
- **Criterios de aceptación:**
  - El sistema crea una sesión anónima válida mediante Firebase Authentication.
  - El sistema crea un perfil básico del usuario asociado a esa sesión.
  - El usuario accede directamente a la pantalla principal de la aplicación, sin pasar por ningún formulario de registro.

---

### FR-002 — Crear o vincular una cuenta completa

- **Descripción:** El sistema deberá permitir que un usuario con una sesión anónima vincule su perfil a una cuenta completa (correo electrónico y contraseña, Google o Apple), de forma opcional, para sincronizar y conservar sus datos entre dispositivos.
- **Prioridad:** Media
- **Actor:** Usuario con sesión anónima
- **Criterios de aceptación:**
  - El usuario puede iniciar el proceso de vinculación desde su perfil, en cualquier momento.
  - Al completar la vinculación, el `uid` original se conserva, junto con todos los grupos, partidos y estadísticas asociados.
  - Un usuario que ya posee una cuenta completa en otro dispositivo puede iniciar sesión con ella para recuperar sus datos.

---

### FR-003 — Crear grupo

- **Descripción:** El sistema deberá permitir a un usuario crear un nuevo grupo privado de fútbol.
- **Prioridad:** Alta
- **Actor:** Usuario con sesión activa (anónima o vinculada a una cuenta completa)
- **Criterios de aceptación:**
  - El grupo se crea correctamente.
  - El creador pasa a formar parte del grupo.
  - El sistema genera un enlace o código de invitación para incorporar nuevos integrantes.

---

### FR-004 — Unirse a un grupo

- **Descripción:** El sistema deberá permitir a un usuario incorporarse a un grupo existente mediante un enlace o código de invitación válido.
- **Prioridad:** Alta
- **Actor:** Usuario con sesión activa (anónima o vinculada a una cuenta completa)
- **Criterios de aceptación:**
  - El usuario pasa a formar parte del grupo.
  - Obtiene acceso al historial, partidos y estadísticas del grupo.

---

### FR-005 — Crear partido

- **Descripción:** El sistema deberá permitir a cualquier integrante del grupo crear un nuevo partido, especificando la fecha y la modalidad de juego.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Se crea un nuevo partido asociado al grupo.
  - El creador puede definir la fecha y seleccionar la modalidad de juego entre los valores admitidos: F5, F6, F7, F9 o F11.
  - Si el grupo tiene una modalidad predeterminada configurada (ver FR-020), el sistema la propone por defecto, permitiendo modificarla para ese partido puntual.
  - El partido queda creado en estado **Programado**.
  - La asignación de jugadores a los equipos no forma parte de este proceso: se realiza en un paso posterior, una vez confirmada la participación (ver FR-006 y FR-016).

---

### FR-006 — Confirmar participación en un partido

- **Descripción:** El sistema deberá permitir indicar qué integrantes participarán de un partido antes de su inicio.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Se pueden seleccionar los jugadores que participarán del encuentro.
  - Solo los jugadores confirmados pueden asignarse a un equipo.
  - Solo los jugadores confirmados pueden asociarse a eventos del partido.

---

### FR-007 — Registrar goles

- **Descripción:** El sistema deberá permitir registrar los goles convertidos durante un partido.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Cada gol queda asociado al equipo correspondiente.
  - Opcionalmente puede asociarse al jugador que convirtió el gol.
  - El marcador del partido se actualiza inmediatamente.

---

### FR-008 — Registrar asistencias

- **Descripción:** El sistema deberá permitir registrar la asistencia asociada a un gol.
- **Prioridad:** Media
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - La asistencia queda vinculada al gol correspondiente.
  - El asistente debe ser un jugador distinto del autor del gol.

---

### FR-009 — Finalizar partido

- **Descripción:** El sistema deberá permitir finalizar un partido en curso.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - El partido cambia al estado **Finalizado**.
  - El sistema actualiza automáticamente todas las estadísticas correspondientes.
  - El partido pasa a formar parte del historial permanente del grupo.

---

### FR-010 — Consultar historial de partidos

- **Descripción:** El sistema deberá permitir consultar el historial de partidos de un grupo.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Se muestra un listado cronológico de los partidos finalizados.
  - Cada partido muestra al menos la fecha, el resultado y los equipos participantes.
  - Es posible acceder al detalle completo de un partido.

---

### FR-011 — Consultar estadísticas

- **Descripción:** El sistema deberá permitir visualizar las estadísticas históricas de los integrantes del grupo.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Se muestran estadísticas como partidos jugados, victorias, empates, derrotas, goles, asistencias y racha.
  - El listado puede ordenarse según diferentes métricas.

---

### FR-012 — Gestionar invitaciones

- **Descripción:** El sistema deberá permitir generar y compartir enlaces o códigos de invitación al grupo.
- **Prioridad:** Media
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - El sistema permite copiar el código o enlace de invitación.
  - Los usuarios invitados pueden unirse al grupo utilizando dicho enlace o código.

---

### FR-013 — Abandonar un grupo

- **Descripción:** El sistema deberá permitir a un usuario salir voluntariamente de un grupo.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - El usuario pierde inmediatamente el acceso al grupo.
  - El historial de partidos y estadísticas en los que participó permanece inalterado.

---

### FR-014 — Buscar jugadores dentro del grupo

- **Descripción:** El sistema deberá permitir localizar rápidamente a un integrante del grupo.
- **Prioridad:** Baja
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Se puede buscar por nombre o apodo.
  - Los resultados se actualizan a medida que se ingresa el texto.

---

### FR-015 — Editar perfil

- **Descripción:** El sistema deberá permitir al usuario modificar su información personal.
- **Prioridad:** Media
- **Actor:** Usuario con sesión activa (anónima o vinculada a una cuenta completa)
- **Criterios de aceptación:**
  - El usuario puede actualizar su nombre, apodo y foto de perfil.
  - Los cambios se reflejan automáticamente en todos los grupos a los que pertenece.

---

### FR-016 — Generar equipos automáticamente

- **Descripción:** El sistema deberá permitir distribuir automáticamente a los jugadores confirmados en dos equipos.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - La distribución puede realizarse de forma aleatoria o considerando estadísticas históricas para equilibrar el nivel.
  - La distribución generada puede modificarse manualmente antes de iniciar el partido.

---

### FR-017 — Registrar autogoles

- **Descripción:** El sistema deberá permitir registrar un gol en contra.
- **Prioridad:** Media
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - El gol se suma al marcador del equipo rival.
  - El evento queda registrado como autogol asociado al jugador correspondiente.

---

### FR-018 — Solicitar corrección de un partido

- **Descripción:** El sistema deberá permitir proponer modificaciones sobre un partido ya finalizado.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - El partido original permanece sin modificaciones hasta resolver la solicitud.
  - La propuesta queda visible para todos los integrantes del grupo.

---

### FR-019 — Aprobar correcciones mediante votación

- **Descripción:** El sistema deberá permitir que los integrantes del grupo aprueben o rechacen una solicitud de corrección.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Los integrantes pueden votar a favor o en contra.
  - Al alcanzarse el criterio de aprobación definido por el sistema, se aplican los cambios y se recalculan automáticamente las estadísticas.

---

### FR-020 — Configurar preferencias del grupo

- **Descripción:** El sistema deberá permitir modificar la información general del grupo.
- **Prioridad:** Media
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Se puede modificar el nombre, la imagen y la modalidad de juego predeterminada.
  - Los cambios son visibles inmediatamente para todos los integrantes.

---

### FR-021 — Consultar perfil de un jugador

- **Descripción:** El sistema deberá permitir visualizar el perfil deportivo de cualquier integrante del grupo.
- **Prioridad:** Alta
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Se muestran las estadísticas acumuladas del jugador.
  - Se visualiza su racha reciente de partidos.

---

### FR-022 — Buscar partidos

- **Descripción:** El sistema deberá permitir realizar búsquedas dentro del historial de partidos del grupo.
- **Prioridad:** Media
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Es posible buscar por fecha, modalidad de juego o participación de un jugador.
  - Se muestran los partidos que cumplen con los criterios de búsqueda.

---

### FR-023 — Filtrar historial

- **Descripción:** El sistema deberá permitir aplicar filtros sobre el historial de partidos.
- **Prioridad:** Baja
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - Es posible filtrar por año, modalidad de juego o resultado.
  - La lista se actualiza inmediatamente al aplicar los filtros.

---

### FR-024 — Compartir resultados

- **Descripción:** El sistema deberá permitir compartir el resumen de un partido o las estadísticas de un jugador fuera de la aplicación.
- **Prioridad:** Media
- **Actor:** Integrante del grupo
- **Criterios de aceptación:**
  - El sistema genera una imagen con la información principal.
  - La imagen puede compartirse mediante las opciones nativas del dispositivo.