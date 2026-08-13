# ⚽ Glosario (Glossary)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento define los términos clave del dominio del proyecto Fulbito. Establecer un vocabulario ubicuo (Ubiquitous Language) permite que todos los integrantes del proyecto utilicen los mismos conceptos durante el análisis, el diseño y el desarrollo del sistema.

---

## A

- **Asistencia:** Pase inmediatamente previo a un gol, atribuido a un jugador distinto del autor del tanto.
- **Autogol (Gol en contra):** Gol convertido por un jugador en su propia portería. Incrementa el marcador del equipo rival y queda registrado como un evento del partido.

---

## E

- **Equipo:** Conjunto temporal de jugadores que disputa un Partido. Cada encuentro está conformado por dos equipos rivales.
- **Estadística:** Información calculada a partir del historial de partidos, como goles, asistencias, victorias, derrotas o rachas. Solo considera partidos finalizados.
- **Evento:** Acción registrada durante un partido, como un gol, asistencia o autogol.

---

## G

- **Grupo:** Espacio privado donde un conjunto de jugadores organiza sus partidos y conserva su historial y estadísticas.

---

## H

- **Historial:** Registro cronológico de todos los partidos finalizados pertenecientes a un grupo. Constituye la fuente de información para el cálculo de las estadísticas.

---

## I

- **Integrante:** Usuario que pertenece a un grupo. Todos los integrantes poseen los mismos permisos funcionales, salvo aquellas acciones que requieren aprobación colectiva.

- **Invitación:** Código o enlace utilizado para incorporarse a un grupo existente.

---

## J

- **Jugador:** Usuario (con sesión anónima o vinculada a una cuenta completa) que participa en uno o más grupos y puede formar parte de los partidos organizados dentro de ellos.

---

## M

- **MVP (Minimum Viable Product):** Primera versión funcional de Fulbito, orientada a validar la propuesta de valor principal mediante el menor conjunto posible de funcionalidades.

---

## P

- **Partido:** Encuentro deportivo perteneciente a un grupo. Puede encontrarse en uno de los siguientes estados:

  1. **Programado:** El partido fue creado y espera la confirmación de los participantes y la definición de los equipos.
  2. **En Curso:** El partido se está disputando y admite el registro de eventos.
  3. **Finalizado:** El partido concluyó y sus resultados pasan a formar parte permanente del historial y las estadísticas del grupo.

---

## R

- **Racha:** Secuencia de resultados más recientes de un jugador, expresada mediante Victorias (V), Empates (E) y Derrotas (D).

- **RSVP (Confirmación de participación):** Acción mediante la cual un integrante indica si participará o no de un partido programado. Solo los jugadores confirmados pueden integrar un equipo y registrar eventos durante el encuentro.

---

## S

- **Sesión anónima:** Sesión de Firebase Authentication creada automáticamente al abrir la aplicación por primera vez, sin requerir un registro tradicional. Posee un identificador (`uid`) único y válido, con los mismos permisos funcionales que una cuenta completa, pero sin persistencia garantizada entre dispositivos hasta que se vincule a un método de autenticación permanente.