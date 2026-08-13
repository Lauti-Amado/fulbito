# ⚽ Registro de Decisiones Arquitectónicas (Architecture Decision Records - ADR)

Este documento registra las principales decisiones arquitectónicas tomadas durante el diseño de **Fulbito**.

Cada decisión incluye el problema que se buscó resolver, la alternativa elegida y los principales beneficios y compromisos asumidos. Su objetivo es preservar el contexto de diseño para facilitar el mantenimiento y la evolución futura del sistema.

---

# ADR-001 — Uso de Cloud Firestore en lugar de una Base de Datos Relacional

**Estado:** Aceptada

## Contexto

Fulbito requiere que los cambios realizados durante un partido (goles, asistencias, marcador, etc.) se reflejen prácticamente en tiempo real en los dispositivos del resto de los integrantes del grupo.

Implementar este comportamiento utilizando una base de datos relacional tradicional implicaría incorporar componentes adicionales para sincronización en tiempo real, aumentando la complejidad del sistema.

## Decisión

Utilizar **Cloud Firestore** como base de datos principal del sistema.

## Ventajas

- Sincronización de datos en tiempo real mediante listeners.
- Escalabilidad administrada.
- Integración nativa con Firebase Authentication y Cloud Functions.
- Flexibilidad para evolucionar el modelo de datos durante el desarrollo del MVP.

## Consecuencias

- Necesidad de desnormalizar cierta información.
- Consultas complejas más limitadas que en una base de datos relacional.
- Mayor atención al costo de las lecturas realizadas por la aplicación.

---

# ADR-002 — React Native + Expo en lugar de Desarrollo Nativo

**Estado:** Aceptada

## Contexto

El público objetivo utiliza dispositivos Android e iOS indistintamente.

Mantener dos aplicaciones nativas independientes aumentaría considerablemente el tiempo de desarrollo y mantenimiento.

## Decisión

Desarrollar la aplicación utilizando **React Native** junto con **Expo**.

## Ventajas

- Una única base de código.
- Desarrollo simultáneo para Android e iOS.
- Iteraciones rápidas durante el desarrollo del MVP.
- Simplificación del proceso de compilación y distribución.

## Consecuencias

- Ligera pérdida de rendimiento respecto a aplicaciones completamente nativas.
- Dependencia del ecosistema React Native y Expo.

---

# ADR-003 — Arquitectura Serverless sobre Firebase

**Estado:** Aceptada

## Contexto

El objetivo principal del proyecto es concentrar el esfuerzo de desarrollo en la lógica del negocio y la experiencia del usuario, evitando la administración de infraestructura propia.

## Decisión

Adoptar una arquitectura **Backend as a Service (BaaS)** utilizando Firebase.

## Ventajas

- Eliminación de la administración de servidores.
- Escalabilidad automática.
- Alta disponibilidad.
- Integración entre Authentication, Firestore, Storage y Cloud Functions.

## Consecuencias

- Dependencia tecnológica del ecosistema Firebase.
- Una futura migración requerirá adaptar parte importante de la infraestructura.

---

# ADR-004 — Estadísticas Cacheadas mediante Desnormalización

**Estado:** Aceptada

## Contexto

Las consultas más frecuentes del sistema corresponden al ranking de jugadores y al perfil deportivo.

Calcular estas estadísticas recorriendo todos los partidos cada vez que un usuario abre la aplicación produciría tiempos de respuesta elevados y un mayor consumo de lecturas en Firestore.

## Decisión

Mantener una copia actualizada de las estadísticas dentro del documento del miembro del grupo.

Las estadísticas serán actualizadas automáticamente mediante Cloud Functions.

## Ventajas

- Consultas extremadamente rápidas.
- Menor cantidad de lecturas.
- Mejor experiencia de usuario.

## Consecuencias

- Mayor complejidad durante la escritura.
- Necesidad de garantizar la consistencia entre partidos y estadísticas.

---

# ADR-005 — Separación entre Usuario y Miembro del Grupo

**Estado:** Aceptada

## Contexto

Un mismo usuario puede participar simultáneamente en varios grupos de fútbol.

Cada grupo debe conservar un historial y estadísticas completamente independientes.

## Decisión

Separar conceptualmente la entidad **Usuario** de la entidad **Miembro del Grupo**.

## Ventajas

- Aislamiento completo de estadísticas entre grupos.
- Independencia de roles por grupo.
- Conservación del historial deportivo aunque un usuario abandone un grupo.

## Consecuencias

- Modelo de datos ligeramente más complejo.
- Mayor cantidad de relaciones entre entidades.

---

# ADR-006 — Uso de Cloud Functions para la Lógica Crítica

**Estado:** Aceptada

## Contexto

Operaciones como recalcular estadísticas, procesar correcciones o validar invitaciones no deberían ejecutarse desde el dispositivo del usuario, ya que podrían comprometer la integridad de los datos.

## Decisión

Centralizar la lógica crítica del negocio en **Cloud Functions**.

## Ventajas

- Mayor seguridad.
- Consistencia de los datos.
- Automatización de procesos desencadenados por eventos.
- Menor posibilidad de manipulación desde el cliente.

## Consecuencias

- Incremento de la complejidad del backend.
- Dependencia de funciones desplegadas en Firebase.

---

> ## Resumen
>
> Las decisiones arquitectónicas de Fulbito priorizan la simplicidad operativa, la sincronización en tiempo real y la escalabilidad. Aunque algunas elecciones implican aceptar compromisos —como la desnormalización de datos o la dependencia del ecosistema Firebase—, todas responden al objetivo principal del proyecto: ofrecer una aplicación móvil rápida, mantenible y centrada en la experiencia del usuario.