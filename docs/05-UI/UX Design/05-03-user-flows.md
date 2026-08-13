# ⚽ Flujos de Usuario (User Flows)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento describe los principales recorridos que realiza un jugador para completar las tareas fundamentales dentro de **Fulbito**.

El objetivo es validar que la navegación y las interacciones definidas en la arquitectura de información sean claras, coherentes y requieran la menor cantidad de pasos posibles, respetando el principio de **"fricción casi nula"**.

Los flujos se encuentran alineados con el modelo de navegación, el sistema de diseño y las reglas de negocio definidas para el MVP.

---

# 1. Flujo de Onboarding y Creación de Grupo

Este flujo representa el recorrido de un usuario que ingresa por primera vez a la aplicación y crea su propio grupo.

### [1. Pantalla de Inicio]

Al abrir la aplicación por primera vez, el sistema crea automáticamente una sesión anónima en segundo plano (ver CU-001), sin mostrar ninguna pantalla de login. El usuario llega directamente a la pantalla principal, donde visualiza los grupos a los que pertenece (inicialmente, ninguno).

Toca el botón primario:

**"Crear Nuevo Grupo"**

↓

### [2. Modal: Configurar Grupo]

Se presenta un formulario donde el usuario puede:

- Ingresar el nombre del grupo.
- Seleccionar un escudo predeterminado o cargar una imagen.
- Seleccionar la modalidad de juego predeterminada (F5, F6, F7, F9 o F11).

Al confirmar, el sistema crea el grupo y agrega automáticamente al usuario como integrante. Al no existir un rol de administrador en Fulbito, el creador del grupo tiene exactamente los mismos permisos que cualquier otro integrante que se una posteriormente.

↓

### [3. Cartelera del Grupo]

El usuario es redirigido automáticamente al entorno del nuevo grupo.

Como todavía no existen partidos ni otros integrantes, se muestra un **Empty State** con una invitación a incorporar jugadores.

↓

### [4. Modal: Invitar Amigos]

El usuario toca:

**"Invitar Amigos"**

El sistema genera una invitación válida y muestra las opciones disponibles para compartirla.

↓

### [5. Acción Nativa: Compartir]

Se abre la hoja de compartir nativa de Android o iOS.

El usuario puede enviar el enlace de invitación mediante WhatsApp u otra aplicación compatible.

---

# 2. Flujo del Ciclo de Vida del Partido

Este es el flujo principal de Fulbito. Se repite periódicamente y debe estar diseñado para minimizar las interacciones necesarias durante la actividad deportiva.

## 2.1. Creación del Partido

### [1. Crear Partido]

Un integrante ingresa a la **Cartelera** y toca:

**"Nuevo Partido"**

Define:

- Fecha.
- Hora.
- Modalidad de juego.

El partido se crea inicialmente con estado:

**Programado**

↓

## 2.2. Confirmación de Asistencia

### [2. Confirmar Asistencia (RSVP)]

Los integrantes reciben la información del partido y pueden indicar su disponibilidad mediante acciones rápidas:

- **Voy**
- **No Voy**
- **Pendiente**

La lista de participantes se actualiza en tiempo real para todos los integrantes del grupo.

↓

## 2.3. Armado de Equipos

### [3. Armar Equipos]

Una vez definida la lista de jugadores confirmados, un integrante toca:

**"Armar Equipos"**

El sistema genera automáticamente dos equipos.

El armado puede realizarse:

- Aleatoriamente.
- Según el nivel de los jugadores, cuando exista dicha información.

El usuario puede modificar manualmente la distribución antes de comenzar el partido.

↓

## 2.4. Inicio del Partido

### [4. Comenzar Partido]

Una vez confirmados los equipos, se toca:

**"Iniciar Partido"**

El estado del partido cambia de:

`Programado → En Curso`

La interfaz se transforma en una pantalla de partido con:

- Marcador grande.
- Identificación de ambos equipos.
- Jugadores participantes.
- Acciones para registrar eventos.

↓

## 2.5. Registro de Eventos

### [5. Registrar Gol]

Durante el partido, el usuario toca el equipo correspondiente al gol.

Se abre un **Bottom Sheet** con los jugadores de ese equipo, junto con una opción adicional:

**"Gol sin autor identificado"**

El usuario puede seleccionar al jugador que convirtió el gol, o bien utilizar la opción anterior cuando no se recuerde con certeza quién lo hizo (ver BR-010).

Si se seleccionó un autor, el sistema permite opcionalmente indicar también al jugador que realizó la asistencia. Esta opción no está disponible si el gol se registró sin autor, ya que una asistencia no puede vincularse a un anotador desconocido.

El sistema registra los eventos y actualiza inmediatamente el marcador.

Por ejemplo:

**Equipo A 1 - 0 Equipo B**

Los cambios se sincronizan en tiempo real con los dispositivos de los integrantes conectados.

↓

## 2.6. Finalización del Partido

### [6. Finalizar Partido]

Al terminar el encuentro, cualquier integrante del grupo toca:

**"Finalizar Partido"**

Se presenta un **Modal de pantalla completa** con un resumen que incluye:

- Resultado final.
- Equipos.
- Goleadores.
- Asistencias.
- Eventos registrados.

El usuario puede revisar visualmente la información antes de confirmar.

↓

## 2.7. Confirmación y Actualización de Estadísticas

### [7. Confirmar Finalización]

El usuario confirma el resultado.

El estado cambia de:

`En Curso → Finalizado`

El partido pasa a formar parte del **Historial**.

Una **Cloud Function** procesa el partido finalizado y actualiza las estadísticas derivadas de los jugadores, incluyendo:

- Partidos jugados.
- Victorias.
- Empates.
- Derrotas.
- Goles.
- Asistencias.
- Autogoles.
- Rachas.

El **Ranking** queda actualizado a partir de la información procesada.

---

# 3. Flujo de Ingreso de un Nuevo Jugador

Este flujo representa el recorrido de un usuario que recibe una invitación para incorporarse a un grupo existente.

### [1. Recibir Invitación]

El usuario recibe un enlace o código de invitación mediante WhatsApp u otro medio.

↓

### [2. Autenticación]

El usuario abre el enlace.

Si no posee una sesión activa, debe autenticarse mediante **Firebase Authentication**.

Puede utilizar alguno de los métodos habilitados por la aplicación, como:

- Correo electrónico y contraseña.
- Google.
- Apple.

Si ya posee una sesión activa, este paso se omite.

↓

### [3. Validación de la Invitación]

El sistema detecta el código asociado a la invitación y muestra información básica del grupo.

Por ejemplo:

> **Te invitaron a "Fútbol de los Jueves"**

Se presenta el botón:

**"Unirme al Grupo"**

↓

### [4. Incorporación al Grupo]

El usuario confirma su ingreso.

El sistema valida que la invitación sea válida y crea su registro como miembro del grupo.

↓

### [5. Ingreso al Entorno del Grupo]

El usuario es redirigido automáticamente a la **Cartelera**.

Puede visualizar el estado actual del grupo y participar en los próximos partidos.

---

# 4. Flujo de Resolución de Inconsistencias

Este flujo permite corregir errores en partidos finalizados sin permitir que un único usuario modifique unilateralmente el historial.

## 4.1. Solicitud de Corrección

### [1. Historial]

El jugador ingresa a la pestaña **Historial**.

Selecciona el partido que contiene el error.

↓

### [2. Detalle del Partido]

Se muestra el detalle completo del encuentro:

- Resultado.
- Equipos.
- Jugadores.
- Goles.
- Asistencias.
- Eventos registrados.

El usuario toca:

**"Solicitar Corrección"**

↓

### [3. Crear Solicitud]

Se presenta un formulario donde el usuario describe el problema.

Por ejemplo:

> "Me anotaron un gol de menos."

El usuario confirma la solicitud.

La solicitud queda en estado:

**Pendiente**

↓

## 4.2. Votación

### [4. Notificación al Grupo]

Los jugadores que participaron del partido afectado reciben una notificación indicando que existe una solicitud de corrección pendiente, ya que son quienes están habilitados para votar (ver `03-requirements/03-08-voting-rules.md`).

↓

### [5. Revisar Solicitud]

Los integrantes ingresan al detalle de la solicitud y pueden consultar:

- Partido afectado.
- Usuario solicitante.
- Descripción del problema.
- Modificación propuesta.
- Estado de la votación.

↓

### [6. Emitir Voto]

Mediante un **Bottom Sheet**, el jugador selecciona:

- **Aprobar**
- **Rechazar**

Solo pueden votar los jugadores que participaron del partido afectado. Cada uno de ellos puede emitir un único voto.

↓

## 4.3. Aplicación de la Corrección

### [7. Evaluación Automática]

Cuando se alcanza el criterio de aprobación establecido, una **Cloud Function** procesa automáticamente la solicitud.

Si la solicitud es aprobada:

1. Se modifica la información correspondiente del partido.
2. Se recalculan las estadísticas afectadas.
3. Se actualiza el ranking.
4. Se actualiza el historial.
5. La solicitud pasa a estado **Aprobada**.

Si los votos en contra hacen matemáticamente imposible alcanzar el criterio de aprobación (`requiredToPass`), la solicitud pasa automáticamente a estado **Rechazada**, conforme a la fórmula definida en `03-requirements/03-08-voting-rules.md`.

↓

### [8. Notificación del Resultado]

Los integrantes reciben una notificación indicando el resultado de la solicitud.

Por ejemplo:

**"Corrección aprobada. Las estadísticas fueron actualizadas."**

---

# 5. Flujo de Consulta del Ranking

Este flujo describe cómo un jugador consulta las estadísticas acumuladas de su grupo.

### [1. Ingresar al Ranking]

Desde la navegación inferior del grupo, el usuario selecciona:

**Ranking**

↓

### [2. Visualizar Tabla General]

El sistema muestra la clasificación de los integrantes del grupo.

La información principal incluye:

- Posición.
- Avatar.
- Nombre.
- Partidos jugados.
- Victorias.
- Empates.
- Derrotas.
- Goles.
- Asistencias.
- Racha.

↓

### [3. Consultar Perfil Deportivo]

El usuario toca un jugador de la tabla.

↓

### [4. Ficha del Jugador]

Se muestra el perfil deportivo individual con sus estadísticas acumuladas y evolución dentro del grupo.

Las estadísticas pertenecen exclusivamente al grupo desde el cual se accedió al perfil.

---

# 6. Flujo de Consulta del Historial

Este flujo representa la consulta de los partidos finalizados del grupo.

### [1. Ingresar al Historial]

Desde la navegación inferior, el usuario selecciona:

**Historial**

↓

### [2. Lista de Partidos]

Se muestra una lista cronológica de los partidos finalizados.

Cada **Match Card** presenta información resumida:

- Fecha.
- Modalidad.
- Equipos.
- Marcador.
- Principales eventos.

↓

### [3. Consultar Partido]

El usuario toca un partido.

↓

### [4. Detalle del Partido]

Se muestra el encuentro completo, incluyendo:

- Resultado.
- Equipos.
- Participantes.
- Goles.
- Asistencias.
- Autogoles.
- Eventos registrados.

Desde esta pantalla también puede iniciarse una **Solicitud de Corrección** cuando corresponda.

---

# 7. Principios Aplicados a los Flujos

Todos los recorridos definidos para Fulbito siguen los siguientes principios de diseño:

### Fricción casi nula

Las acciones frecuentes deben requerir la menor cantidad posible de interacciones.

### Contexto persistente

El usuario debe comprender siempre en qué grupo y sección se encuentra.

### Acciones rápidas

Las operaciones frecuentes, especialmente durante un partido, deben poder realizarse mediante uno o pocos toques.

### Feedback inmediato

Las acciones deben producir una respuesta visual clara y, cuando corresponda, actualizar la información en tiempo real.

### Prevención de errores

Las operaciones críticas deben solicitar confirmación antes de ejecutarse.

### Integridad del historial

Los partidos finalizados no pueden modificarse directamente. Las correcciones deben seguir el flujo de solicitud y votación definido.

### Consistencia

Los flujos deben respetar los componentes, estados visuales y patrones de navegación definidos en el **Design System** y la **Arquitectura de la Información**.

---

> **Resumen**
>
> Los flujos de usuario de Fulbito están diseñados alrededor de su actividad principal: organizar partidos, jugar, registrar resultados y conservar la historia deportiva del grupo.
>
> La experiencia prioriza acciones rápidas durante el partido, navegación clara fuera de él y mecanismos de control para preservar la integridad de las estadísticas y del historial.