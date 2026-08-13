# ⚽ Roles y Permisos

> **Versión:** 1.0
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

# 1. Modelo horizontal

Fulbito no implementa una jerarquía de roles permanente. No existe un rol "administrador", "dueño del grupo" ni ningún otro rol individual con permisos especiales sobre el resto de los integrantes.

Todos los integrantes activos de un grupo poseen el mismo nivel de permisos funcionales, con una única excepción: un conjunto reducido de **acciones críticas**, que en lugar de depender de un rol individual, requieren aprobación mediante **votación colectiva** (ver `08-voting-rules.md`).

Este enfoque es coherente con la filosofía del producto definida en la Visión y en la Investigación de Mercado: Fulbito es horizontal por diseño, sin delegados ni administradores de liga.

---

# 2. Tabla de permisos

| Acción | Quién puede realizarla |
|--------|------------------------|
| Crear un partido | Cualquier integrante |
| Confirmar participación (RSVP) | Cualquier integrante, sobre sí mismo |
| Registrar goles, asistencias o autogoles | Cualquier integrante confirmado en ese partido |
| Finalizar un partido | Cualquier integrante |
| Generar equipos automáticamente / modificarlos manualmente | Cualquier integrante |
| Modificar nombre, imagen o modalidad predeterminada del grupo | Cualquier integrante |
| Generar invitaciones al grupo | Cualquier integrante |
| Abandonar el grupo | Cualquier integrante, sobre sí mismo, sin aprobación previa |
| Solicitar una corrección sobre un partido finalizado | Cualquier integrante |
| Votar una solicitud de corrección | Los jugadores que participaron del partido en cuestión |
| **Expulsar a un integrante del grupo** | Requiere votación colectiva (acción crítica) |
| **Eliminar el grupo completo** | Requiere votación colectiva (acción crítica) |

---

# 3. Acciones críticas

Se consideran acciones críticas aquellas que pueden afectar de forma permanente al grupo o a sus integrantes, y por lo tanto no pueden ser ejecutadas unilateralmente por una sola persona:

- Expulsar a un integrante del grupo.
- Eliminar un grupo completo.
- Aprobar una corrección sobre un partido finalizado.

El mecanismo, la fórmula de aprobación y los votantes elegibles para cada tipo de acción crítica están definidos en `08-voting-rules.md`.

---

> ## Nota de diseño
>
> La ausencia de roles jerárquicos es una decisión de producto, no una limitación técnica. Fulbito prioriza que el grupo sea horizontal: todos los amigos tienen el mismo peso.