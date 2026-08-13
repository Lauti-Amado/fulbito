# ⚽ Requisitos del Sistema

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Esta carpeta reúne toda la documentación funcional que especifica el comportamiento esperado del MVP de **Fulbito**.

El objetivo de esta fase es traducir la visión del producto y los hallazgos de la investigación de mercado en especificaciones claras, medibles y accionables para el equipo de desarrollo. Aquí se especifica qué debe hacer la aplicación, quiénes interactuarán con ella y bajo qué reglas deberá operar.

---

# Contenido

| Documento | Descripción |
|-----------|-------------|
| `README.md` | Documento principal con la introducción y organización de los requisitos. |
| `01-business-rules.md` | Reglas de negocio que dictan la lógica central de Fulbito (ej. cómo se determina la racha de un jugador o qué constituye un partido válido). |
| `02-functional-requirements.md` | Detalle exhaustivo de las acciones específicas que el sistema debe permitir realizar para cumplir con los objetivos del MVP. |
| `03-user-stories.md` | Requisitos expresados desde la perspectiva del jugador, enfocados en el valor y la motivación detrás de cada funcionalidad. |
| `04-use-cases.md` | Casos de uso que describen las interacciones paso a paso entre los usuarios y la aplicación. |
| `05-non-functional-requirements.md` | Atributos de calidad del sistema, incluyendo rendimiento, disponibilidad, sincronización en tiempo real, seguridad, escalabilidad y demás restricciones técnicas. |
| `06-glossary.md` | Definición de términos clave (ej. Partido Rápido, Historial, MVP) para mantener un lenguaje unificado y evitar ambigüedades durante el desarrollo. |
| `07-roles-permissions.md` | Modelo horizontal de permisos: qué puede hacer cada integrante y qué acciones son consideradas críticas. |
| `08-voting-rules.md` | Fórmula y mecánica de votación para las acciones críticas del sistema (correcciones, expulsiones, eliminación de grupo). |

---

# Objetivo de esta etapa

Esta documentación sirve como el puente definitivo entre la idea abstracta y el código. Permite asegurar que todo el esfuerzo de desarrollo esté perfectamente alineado con la filosofía de Fulbito: mantener una simplicidad extrema, garantizar una carga de datos sin fricciones y construir una experiencia que no interfiera con el partido. Cada funcionalidad implementada deberá poder trazarse hasta uno o más requisitos definidos en esta carpeta, garantizando la coherencia entre la visión del producto y su implementación.

Los documentos de esta carpeta serán la referencia principal para las etapas posteriores de arquitectura, diseño, implementación y pruebas.

---

> **La visión define el destino. Los requisitos trazan el mapa exacto para llegar a él sin perder el foco.**

---

## Próximos pasos

Finalizada la definición de los requisitos del sistema, el siguiente paso consistirá en estructurar la arquitectura técnica, el modelo de datos y el diseño de la experiencia de usuario (UI/UX), asegurando que la interfaz móvil cumpla con la promesa de inmediatez.

---

## Navegación

⬅ **Anterior:** `../02-market-research/`

➡ **Siguiente:** `../04-architecture/`