# ⚽ Diseño de Interfaces y Experiencia de Usuario (UI/UX)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Esta carpeta reúne toda la documentación relacionada con el diseño visual, la arquitectura de la información, la navegación y la experiencia de usuario del MVP de **Fulbito**.

El objetivo de esta etapa es traducir los requisitos funcionales y la arquitectura del sistema en una interfaz tangible, consistente, atractiva y, sobre todo, rápida de utilizar.

Aquí se define **cómo se ve Fulbito, cómo se organiza la información, cómo navega el usuario y cómo interactúa con las funciones principales de la aplicación**, manteniendo como principio central la idea de **"fricción casi nula"**.

---

# Contenido

| Documento | Descripción |
| --- | --- |
| `README.md` | Documento principal con la introducción, organización y objetivos de la etapa de diseño UI/UX. |
| `01-design-system.md` | Sistema de diseño: definición de colores, tipografías, iconografía, botones, tarjetas, campos de entrada, espaciados y estados visuales. |
| `02-information-architecture.md` | Arquitectura de la información: mapa de navegación, jerarquía de contenidos, navegación global, navegación dentro de grupos y uso de modales y Bottom Sheets. |
| `03-user-flows.md` | Flujos de usuario detallados paso a paso para las principales tareas de Fulbito, incluyendo creación de grupos, organización de partidos, ingreso de jugadores y solicitud de correcciones. |
| `04-wireframes.md` | Estructura conceptual de las interfaces mediante esquemas de baja fidelidad, definiendo la distribución funcional de los elementos antes de aplicar el diseño visual definitivo. |
| `05-screen-design.md` | Diseño visual de alta fidelidad de las pantallas principales, aplicando el sistema de diseño, la arquitectura de información y los flujos previamente definidos. |
| `06-usability.md` | Criterios y directrices de usabilidad, accesibilidad, ergonomía, feedback, prevención de errores, navegación y adaptación al contexto de uso deportivo. |

---

# Objetivo de esta etapa

Esta documentación constituye el punto de encuentro entre la lógica del sistema y la experiencia del jugador.

Su finalidad es establecer una guía clara para que el equipo de desarrollo pueda implementar interfaces consistentes y coherentes con los requisitos funcionales definidos anteriormente.

El diseño de Fulbito debe transmitir una estética **deportiva, moderna y profesional**, tomando como referencia las experiencias de las grandes plataformas deportivas, pero adaptándolas al contexto específico del fútbol amateur y a la dinámica social de los grupos.

La interfaz debe priorizar:

- **Fricción casi nula:** las acciones frecuentes deben requerir la menor cantidad posible de pasos.
- **Rapidez:** consultar información o registrar eventos debe ser un proceso ágil.
- **Claridad:** la información importante debe identificarse inmediatamente.
- **Consistencia:** los mismos componentes y patrones deben mantener los mismos significados.
- **Accesibilidad:** la información debe ser legible y comprensible en diferentes condiciones de uso.
- **Contexto:** la interfaz debe adaptarse a las necesidades del usuario antes, durante y después del partido.
- **Prevención de errores:** las acciones importantes deben contar con mecanismos adecuados de confirmación y recuperación.

El diseño debe garantizar que **cargar un partido, confirmar asistencia, armar equipos, registrar un gol o consultar una estadística** sea un proceso intuitivo y veloz que no interrumpa la dinámica del grupo ni compita con la experiencia real de jugar.

---

# Principios de Diseño

Toda decisión de UI/UX de Fulbito debe respetar los siguientes principios:

### ⚡ Fricción casi nula

El usuario debe poder realizar las acciones frecuentes con la menor cantidad de pasos e interacciones posibles.

### 👆 Diseño orientado al pulgar

Las acciones principales deben encontrarse en zonas accesibles para utilizar la aplicación cómodamente con una sola mano.

### ⚽ Diseño orientado al contexto

La interfaz debe considerar que Fulbito puede utilizarse en diferentes momentos:

- Antes del partido.
- Durante el partido.
- Después del partido.

Cada contexto debe priorizar la información y las acciones más relevantes para ese momento.

### 🎯 Información primero

Los marcadores, resultados, estadísticas y estados de los partidos deben tener una jerarquía visual clara y ser fáciles de identificar.

### 🛡️ Prevención de errores

Las acciones importantes deben contar con mecanismos que permitan confirmar o deshacer operaciones antes de afectar permanentemente la información.

### 🔄 Feedback inmediato

Toda interacción relevante debe producir una respuesta visual, háptica o informativa que permita al usuario saber que su acción fue registrada.

### 🧭 Contexto permanente

El usuario debe saber siempre dónde se encuentra dentro de la aplicación y, especialmente, en qué grupo está trabajando.

### 🎨 Consistencia visual

Los colores, componentes, espaciados, tipografías e interacciones definidos en el Sistema de Diseño deben mantenerse consistentes en todas las pantallas.

---

# Relación entre los documentos

Los documentos de esta etapa siguen una progresión desde las decisiones más generales hasta la definición concreta de las interfaces:

```text
01 - Design System
        │
        │ Define la base visual
        ▼
02 - Information Architecture
        │
        │ Define cómo se organiza la información
        ▼
03 - User Flows
        │
        │ Define cómo se mueve el usuario
        ▼
04 - Wireframes
        │
        │ Define la estructura de las pantallas
        ▼
05 - Screen Design
        │
        │ Aplica el diseño visual definitivo
        ▼
06 - Usability
        │
        │ Valida la experiencia de interacción
        ▼
     Implementación
```

De esta manera, cada documento utiliza como base las decisiones tomadas en los anteriores y reduce la posibilidad de inconsistencias durante el desarrollo.

---

# Resultado esperado

Al finalizar esta etapa, el proyecto contará con una definición completa de la experiencia de usuario de Fulbito, incluyendo:

- Sistema visual.
- Arquitectura de navegación.
- Flujos principales.
- Estructura de las pantallas.
- Diseño visual de alta fidelidad.
- Criterios de usabilidad.
- Criterios de accesibilidad.
- Principios de interacción.
- Prevención y recuperación de errores.

Estos elementos servirán como referencia directa durante la implementación de la aplicación en **React Native + Expo**, permitiendo que el desarrollo respete las decisiones de diseño establecidas.

---

> **La mejor interfaz es la que no se nota.**
>
> **En Fulbito, el diseño no compite con el partido; lo inmortaliza.**

---

## Próximos pasos

Finalizada la etapa de diseño de UI/UX, y con los flujos, estructuras y pantallas definidos, el proyecto contará con los planos necesarios para comenzar la implementación.

La siguiente etapa se enfocará en transformar estas definiciones en código, integrando las interfaces con la arquitectura, los servicios y la lógica de negocio de Fulbito.

---

## Navegación

⬅ **Anterior:** `../04-architecture/`

➡ **Siguiente:** `../06-development/`