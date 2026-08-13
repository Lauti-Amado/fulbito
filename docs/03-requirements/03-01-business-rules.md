# ⚽ Reglas de Negocio (Business Rules)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento define las restricciones, invariantes y reglas lógicas que gobiernan el funcionamiento de Fulbito. Estas reglas son independientes de la interfaz de usuario o de la tecnología utilizada para programar la aplicación; son verdades absolutas que el sistema debe garantizar en todo momento.

## Reglas de Grupo y Privacidad
- **BR-001:** Un grupo es estrictamente privado. Solo los miembros aceptados pueden visualizar el historial, los partidos y las estadísticas.
- **BR-002:** Todos los miembros activos de un grupo poseen los mismos permisos funcionales dentro del grupo, salvo aquellas acciones que requieran aprobación colectiva.
- **BR-003:** Solo los integrantes activos de un grupo pueden crear partidos, registrar eventos y realizar acciones permitidas dentro de ese grupo.
- **BR-004:** El historial de partidos y estadísticas de un grupo nunca se elimina automáticamente; perdura mientras el grupo exista.

## Reglas de Partido y Equipos
- **BR-005:** Un partido válido debe estar conformado obligatoriamente por dos equipos rivales.
- **BR-006:** Un jugador no puede pertenecer a más de un equipo simultáneamente en el mismo partido.
- **BR-007:** Un partido solo puede tener tres estados: Programado, En Curso o Finalizado.
- **BR-008:** Un partido solo puede ser finalizado una única vez.
- **BR-009:** Una vez que un partido pasa al estado "Finalizado", sus resultados y eventos no pueden modificarse directamente. Cualquier corrección deberá realizarse mediante el mecanismo definido por el grupo para resolver inconsistencias.

## Reglas de Eventos y Estadísticas
- **BR-010:** Un gol puede registrarse con o sin un jugador asociado, ya que no siempre se recuerda con certeza quién lo convirtió. Una asistencia, en cambio, siempre debe estar asociada a un único jugador válido que haya participado en el partido, dado que registrar una asistencia sin autor no aporta información útil.
- **BR-010-a:** Una asistencia solo puede registrarse si el gol al que hace referencia tiene un jugador anotador conocido, y el asistente debe ser un jugador distinto de dicho anotador.
- **BR-011:** Un jugador no puede registrarse una asistencia a sí mismo.
- **BR-012:** Las estadísticas individuales (goles, victorias, rachas) y grupales solo impactan y se actualizan en la base de datos cuando el partido cambia a estado "Finalizado", nunca mientras está "En Curso".
- **BR-013:** Si un partido es cancelado o eliminado antes de finalizar, ninguno de los eventos registrados durante el mismo afectará las estadísticas históricas de los jugadores ni del grupo.
- **BR-014:** Solo los jugadores incluidos en un partido pueden registrar eventos asociados a ese partido.
- **BR-015:** El resultado final del partido debe ser consistente con la suma de goles registrados para cada equipo.
- **BR-016:** Un jugador puede pertenecer simultáneamente a múltiples grupos independientes.
- **BR-017:** La generación automática de equipos nunca modifica manualmente la composición definida por los jugadores.
- **BR-018:** La eliminación de un jugador de un grupo no elimina su participación histórica en partidos anteriores.
- **BR-019:** Las acciones que puedan afectar permanentemente el historial del grupo (por ejemplo, correcciones sobre partidos finalizados) deberán requerir aprobación mediante votación, según la fórmula y los criterios definidos en el documento de Reglas de Votación (`08-voting-rules.md`).
- **BR-020:** Ningún miembro puede alterar unilateralmente el historial del grupo.
- **BR-021:** Todo partido registrado pertenece a un único grupo y forma parte permanente de su historial.
- **BR-022:** Las estadísticas mostradas por el sistema deben calcularse exclusivamente a partir de los partidos válidos registrados en el historial del grupo.
- **BR-023:** Fulbito no posee roles jerárquicos permanentes (como un rol "administrador"). Todos los integrantes activos de un grupo poseen el mismo nivel de permisos, salvo por las acciones explícitamente definidas como críticas.
- **BR-024:** La expulsión de un integrante del grupo y la eliminación completa de un grupo son consideradas acciones críticas. Ningún integrante puede ejecutarlas unilateralmente: requieren aprobación mediante votación colectiva, según lo definido en las Reglas de Votación.