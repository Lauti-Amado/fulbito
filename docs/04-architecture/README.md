# ⚽ Arquitectura del Sistema

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Esta carpeta reúne toda la documentación técnica que define la arquitectura del MVP de **Fulbito**.

El objetivo de esta etapa es transformar los requisitos funcionales y no funcionales en una solución técnica concreta. Aquí se documentan la arquitectura general del sistema, el modelo de dominio, el diseño de la base de datos, las interacciones entre los distintos componentes, las políticas de seguridad, la estrategia de despliegue y las principales decisiones arquitectónicas adoptadas durante el proyecto.

---

# Contenido

| Documento | Descripción |
| --------- | ----------- |
| `README.md` | Documento principal con la introducción y organización de la arquitectura. |
| `04-01-system-overview.md` | Visión general de la arquitectura del sistema, describiendo los principales componentes y su interacción. |
| `04-02-domain-model.md` | Modelo conceptual del dominio, definiendo las entidades principales y sus relaciones. |
| `04-03-database-design.md` | Diseño físico de la base de datos en Cloud Firestore, incluyendo colecciones, documentos y decisiones de modelado. |
| `04-04-api-design.md` | Diseño conceptual de las operaciones entre la aplicación móvil y los servicios de Firebase. |
| `04-05-security.md` | Estrategias de autenticación, autorización, privacidad y protección de datos. |
| `04-06-deployment.md` | Estrategia de despliegue, administración de entornos y publicación de la aplicación. |
| `04-07-decisions.md` | Registro de Decisiones Arquitectónicas (ADR), documentando el contexto y la justificación de las principales elecciones tecnológicas. |

---

# Objetivo de esta etapa

La documentación de arquitectura constituye el plano técnico del proyecto. Su propósito es establecer una visión compartida sobre la estructura del sistema, el flujo de información y las decisiones que sustentan su implementación.

La arquitectura propuesta se basa en un enfoque **Serverless (Backend as a Service)** utilizando Firebase, priorizando la simplicidad operativa, la sincronización de datos en tiempo real, la escalabilidad y la mantenibilidad.

Estas decisiones permiten que el equipo concentre sus esfuerzos en el desarrollo de las funcionalidades del producto sin asumir la complejidad de administrar infraestructura propia.

Los documentos de esta carpeta servirán como referencia técnica durante las etapas de implementación, pruebas y evolución del sistema.

---

> **Una buena arquitectura es invisible para el usuario, pero es la base que permite que cada partido se registre de forma rápida, segura y confiable.**

---

## Próximos pasos

Finalizada la definición de la arquitectura del sistema, la siguiente etapa consiste en diseñar la interfaz y la experiencia de usuario (UI/UX), definiendo la estructura de las pantallas, los flujos de navegación y los principios visuales que guiarán la interacción con la aplicación.

---

## Navegación

⬅ **Anterior:** `../03-requirements/`

➡ **Siguiente:** `../05-ui-ux-design/`