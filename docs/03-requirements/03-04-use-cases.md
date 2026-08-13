# ⚽ Casos de Uso (Use Cases)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento describe las interacciones completas paso a paso entre los actores (usuarios) y el sistema para llevar a cabo los procesos principales de Fulbito.

---

## CU-001 — Primer acceso a la aplicación

**Actor:**
Usuario nuevo

**Precondición:**
El usuario abre la aplicación por primera vez en un dispositivo, sin sesión previa.

**Flujo principal:**

1. El usuario abre la aplicación.
2. El sistema crea automáticamente una sesión anónima mediante Firebase Authentication, sin mostrar ninguna pantalla de login.
3. El sistema crea un perfil básico asociado a esa sesión.
4. El sistema muestra la pantalla principal de la aplicación.

**Postcondición:**
El usuario accede a la aplicación y puede utilizar todas las funcionalidades del MVP sin haber completado un registro tradicional.

---

## CU-001-a — Vincular cuenta completa

**Actor:**
Usuario con sesión anónima

**Precondición:**
El usuario posee una sesión anónima activa y decide conservar sus datos a futuro.

**Flujo principal:**

1. El usuario accede a su perfil y selecciona "Vincular cuenta".
2. El sistema solicita el método de vinculación (correo electrónico y contraseña, Google o Apple).
3. El usuario completa la información requerida.
4. El sistema vincula la credencial a la sesión anónima existente, conservando el mismo `uid`.
5. El sistema confirma que los datos quedaron asociados a la nueva cuenta.

**Postcondición:**
El usuario conserva todos sus grupos, partidos y estadísticas, y puede recuperarlos iniciando sesión desde otro dispositivo.

---

## CU-002 — Crear grupo

**Actor:**
Usuario con sesión activa (anónima o vinculada a una cuenta completa)

**Precondición:**
El usuario posee una sesión activa (anónima o vinculada a una cuenta completa).

**Flujo principal:**

1. El usuario selecciona "Crear nuevo grupo".
2. El sistema solicita el nombre del grupo y la modalidad de juego predeterminada.
3. El usuario completa la información y confirma.
4. El sistema crea el grupo.
5. El sistema incorpora automáticamente al creador como integrante.
6. El sistema genera un enlace o código de invitación para compartir.

**Postcondición:**
El grupo queda creado y listo para incorporar nuevos integrantes.

---

## CU-003 — Unirse a un grupo

**Actor:**
Usuario con sesión activa (anónima o vinculada a una cuenta completa)

**Precondición:**
El usuario posee una sesión activa (anónima o vinculada a una cuenta completa) y dispone de un enlace o código de invitación válido.

**Flujo principal:**

1. El usuario ingresa el código o abre el enlace de invitación.
2. El sistema valida la invitación.
3. El sistema muestra la información básica del grupo.
4. El usuario confirma que desea unirse.
5. El sistema incorpora al usuario al grupo.
6. El sistema muestra la pantalla principal del grupo.

**Postcondición:**
El usuario pasa a formar parte del grupo y puede acceder a su información.

---

## CU-004 — Crear y programar un partido

**Actor:**
Integrante del grupo

**Precondición:**
Existe un grupo con al menos dos integrantes.

**Flujo principal:**

1. El usuario selecciona "Nuevo partido".
2. El sistema solicita la fecha y la modalidad de juego.
3. El usuario completa la información.
4. El sistema crea el partido en estado **Programado**.
5. El sistema permite que los integrantes confirmen su participación.

**Postcondición:**
El partido queda disponible para organizarse.

---

## CU-005 — Armar equipos

**Actor:**
Integrante del grupo

**Precondición:**
Existe un partido programado con suficientes jugadores confirmados.

**Flujo principal:**

1. El usuario accede al partido.
2. El sistema muestra la lista de jugadores confirmados.
3. El usuario decide distribuir los equipos manualmente o automáticamente.
4. El sistema genera la distribución (si corresponde).
5. El usuario realiza modificaciones manuales si lo desea.
6. El usuario confirma la formación.
7. El sistema cambia el estado del partido a **En Curso**.

**Postcondición:**
Los equipos quedan definidos y el partido puede comenzar.

---

## CU-006 — Registrar eventos del partido

**Actor:**
Integrante del grupo

**Precondición:**
El partido se encuentra en estado **En Curso**.

**Flujo principal:**

1. El usuario selecciona registrar un nuevo evento.
2. El sistema permite elegir el tipo de evento (gol, asistencia o autogol).
3. El usuario completa la información correspondiente.
4. El sistema registra el evento.
5. El sistema actualiza el marcador cuando corresponde.

**Postcondición:**
El partido refleja correctamente los eventos ocurridos durante el encuentro.

---

## CU-007 — Finalizar partido

**Actor:**
Integrante del grupo

**Precondición:**
El partido se encuentra en estado **En Curso**.

**Flujo principal:**

1. El usuario selecciona "Finalizar partido".
2. El sistema muestra un resumen del encuentro.
3. El usuario confirma la finalización.
4. El sistema cambia el estado del partido a **Finalizado**.
5. El sistema recalcula las estadísticas de todos los participantes.
6. El sistema incorpora el partido al historial del grupo.

**Postcondición:**
El partido queda cerrado y sus estadísticas pasan a formar parte del historial permanente.

---

## CU-008 — Consultar historial y estadísticas

**Actor:**
Integrante del grupo

**Precondición:**
Existe al menos un partido finalizado.

**Flujo principal:**

1. El usuario accede a la sección "Historial" o "Estadísticas".
2. El sistema muestra los partidos y el ranking del grupo.
3. El usuario puede ordenar, buscar o filtrar la información.
4. El usuario selecciona un partido o un jugador.
5. El sistema muestra el detalle correspondiente.

**Postcondición:**
El usuario consulta correctamente la información histórica y estadística del grupo.

---

## CU-009 — Solicitar y votar una corrección

**Actor:**
Integrante del grupo

**Precondición:**
Existe un partido finalizado.

**Flujo principal:**

1. El usuario accede al detalle del partido.
2. Selecciona "Solicitar corrección".
3. El sistema solicita la modificación propuesta.
4. El usuario envía la solicitud.
5. El sistema notifica al resto del grupo.
6. Los jugadores que participaron del partido emiten su voto.
7. Si se alcanza el criterio de aprobación definido por el sistema, el cambio se aplica automáticamente y las estadísticas se recalculan.

**Postcondición:**
La corrección queda aprobada o rechazada según el resultado de la votación.

---

## CU-010 — Abandonar un grupo

**Actor:**
Integrante del grupo

**Precondición:**
El usuario pertenece al grupo.

**Flujo principal:**

1. El usuario accede a la configuración del grupo.
2. Selecciona "Abandonar grupo".
3. El sistema solicita confirmación.
4. El usuario confirma la acción.
5. El sistema elimina al usuario del grupo.

**Postcondición:**
El usuario deja de pertenecer al grupo y pierde acceso a su información.

---

## CU-011 — Compartir resultados

**Actor:**
Integrante del grupo

**Precondición:**
Existe un partido finalizado o un perfil de jugador.

**Flujo principal:**

1. El usuario selecciona la opción "Compartir".
2. El sistema genera una imagen con la información correspondiente.
3. El sistema abre el menú de compartir del dispositivo.
4. El usuario selecciona la aplicación destino.

**Postcondición:**
El contenido queda listo para compartirse fuera de Fulbito.