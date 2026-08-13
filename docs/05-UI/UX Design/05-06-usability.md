# ⚽ Usabilidad y Experiencia (Usability)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento define los principios de usabilidad y experiencia de usuario que guiarán la interacción con **Fulbito**.

El objetivo es que la aplicación pueda utilizarse de forma rápida, intuitiva y con una carga cognitiva mínima, incluso en contextos adversos como durante un partido, bajo luz solar directa o utilizando el dispositivo con una sola mano.

Estos criterios complementan el Sistema de Diseño, la Arquitectura de la Información y los Flujos de Usuario definidos para la aplicación.

---

## 1. Facilidad de Uso (Ease of Use)

La aplicación debe requerir una carga cognitiva mínima. El usuario no debería necesitar aprender procedimientos complejos para utilizar Fulbito, sino percibirlo como una extensión natural de la organización de sus partidos.

### Ley del Pulgar (Thumb-Zone)

Las interacciones críticas deben poder realizarse cómodamente con una sola mano.

Las principales acciones, como:

- Confirmar asistencia.
- Registrar goles.
- Registrar asistencias.
- Deshacer un evento.
- Finalizar un partido.

deben ubicarse preferentemente en zonas accesibles del tercio inferior de la pantalla.

### Cero tipeo durante un partido

Durante un partido en curso, la aplicación debe evitar el uso del teclado siempre que sea posible.

El registro de eventos debe realizarse mediante:

- Botones grandes.
- Avatares de jugadores.
- Escudos de equipos.
- Opciones predefinidas.
- Selecciones mediante Bottom Sheets.

El objetivo es que registrar una acción durante el partido requiera la menor cantidad posible de interacciones.

---

## 2. Accesibilidad (Accessibility)

El diseño visual debe garantizar que la información importante sea fácilmente perceptible por la mayor cantidad posible de usuarios y en diferentes condiciones de uso.

### Contraste

El Tema Oscuro definido en el Sistema de Diseño utiliza fondos oscuros y textos claros para facilitar la lectura.

Los elementos críticos, como:

- Marcadores.
- Estados de partidos.
- Botones principales.
- Resultados.
- Estadísticas.

deben mantener un contraste suficiente respecto de su fondo.

### Independencia del Color

La información crítica nunca debe depender exclusivamente del color.

Por ejemplo, una racha no debe representar únicamente:

- 🟢 Verde = Victoria.
- ⚪ Blanco = Empate.
- 🔴 Rojo = Derrota.

También debe utilizar una representación textual o icónica:

- **V** = Victoria.
- **E** = Empate.
- **D** = Derrota.

De esta manera, la información continúa siendo comprensible incluso para usuarios con dificultades para distinguir determinados colores.

### Tamaño de Texto

Los marcadores y estadísticas principales deben utilizar tamaños suficientemente grandes para poder identificarse rápidamente durante un partido.

La aplicación debe respetar las capacidades de escalado de texto proporcionadas por Android e iOS siempre que esto no comprometa la estructura de las interfaces.

---

## 3. Retroalimentación (Feedback)

El sistema debe comunicar al usuario cuándo una acción fue recibida, procesada o requiere esperar.

### Feedback Háptico

Las acciones relevantes pueden utilizar retroalimentación háptica para confirmar que una interacción fue registrada.

Por ejemplo:

- Registrar un gol.
- Registrar una asistencia.
- Finalizar un partido.
- Confirmar una acción importante.

La intensidad y frecuencia de estas vibraciones deben ser moderadas para evitar molestias durante el uso prolongado.

### Feedback Visual Inmediato

Los elementos interactivos deben reaccionar inmediatamente al ser presionados.

Por ejemplo:

- Cambio de opacidad.
- Animación breve.
- Cambio de estado.
- Indicador de carga.

Cuando una acción requiera procesamiento, el botón debe mostrar un estado de carga y bloquear interacciones duplicadas hasta completar la operación.

### Toasts

Las acciones secundarias que no requieren interrumpir el flujo principal pueden confirmarse mediante mensajes temporales.

Ejemplos:

- "Enlace de invitación copiado."
- "Equipo actualizado."
- "Solicitud enviada."

Estos mensajes deben desaparecer automáticamente después de unos segundos.

---

## 4. Prevención y Recuperación de Errores (Error Prevention)

Dado que los partidos finalizados pasan a formar parte del historial deportivo del grupo, la interfaz debe prevenir modificaciones accidentales y facilitar la recuperación ante errores.

### Deshacer Rápido (Undo)

Después de registrar un evento durante un partido en curso, la interfaz debe ofrecer temporalmente una opción **"Deshacer"**.

Por ejemplo:

```text
┌─────────────────────────────────────┐
│ ⚽ Gol registrado: Lauti            │
│                           Deshacer  │
└─────────────────────────────────────┘
```

La opción estará disponible durante un período breve, permitiendo corregir rápidamente errores de interacción sin necesidad de eliminar manualmente el evento.

### Confirmación de Finalización

Finalizar un partido es una acción importante porque modifica su estado a **Finalizado** y lo incorpora al historial.

Por este motivo, antes de confirmar la operación debe mostrarse una pantalla de resumen que permita revisar:

- Marcador final.
- Equipos.
- Goleadores.
- Asistencias.
- Otros eventos relevantes.

El usuario deberá realizar una confirmación explícita mediante una acción como:

**"Sí, Finalizar Partido"**

### Sistema de Correcciones

Si el error se detecta después de finalizar el partido, el usuario no podrá modificar directamente el encuentro.

Deberá utilizar el mecanismo de **Solicitud de Corrección**, definido en la arquitectura y en los flujos de usuario.

La modificación será aplicada únicamente cuando se cumplan las condiciones de aprobación establecidas por el sistema.

---

## 5. Consistencia (Consistency)

La aplicación debe mantener comportamientos y patrones visuales consistentes en todas sus pantallas.

### Consistencia Visual

Los colores definidos en el Sistema de Diseño deben mantener un significado estable.

Por ejemplo:

- **Verde:** confirmación, acción positiva o victoria.
- **Amarillo:** advertencia, empate o estado que requiere atención.
- **Rojo:** error, eliminación, acción destructiva o derrota.
- **Gris:** información secundaria o estado deshabilitado.

### Consistencia de Componentes

Los mismos tipos de acciones deben utilizar componentes visuales equivalentes.

Por ejemplo:

- Las acciones principales utilizan botones primarios.
- Las acciones secundarias utilizan botones secundarios.
- Las acciones destructivas utilizan acciones claramente identificadas como tales.
- Las acciones rápidas durante un partido utilizan componentes accesibles desde la zona inferior.

### Consistencia de Navegación

Mientras el usuario se encuentre dentro de un grupo, la navegación principal debe mantenerse estable.

La navegación mediante **Bottom Tabs** debe conservar:

1. Cartelera.
2. Historial.
3. Ranking.
4. Miembros.

Las pantallas secundarias deben conservar una acción clara de retroceso.

---

## 6. Navegación (Navigation)

La navegación debe ser simple y predecible. El usuario debe poder acceder rápidamente a las funciones principales sin atravesar múltiples niveles innecesarios de menús.

### Regla de los 3 Taps

Las acciones y consultas frecuentes deberían poder alcanzarse en un máximo aproximado de tres interacciones desde la pantalla principal del grupo.

Ejemplos:

- Consultar las estadísticas de un jugador.
- Revisar el último partido.
- Confirmar asistencia.
- Consultar el ranking.
- Acceder al detalle de un partido.

Esta regla funciona como criterio de diseño y no como una restricción absoluta cuando una operación requiere pasos adicionales para garantizar seguridad o evitar errores.

### Identidad Espacial

El usuario debe saber en todo momento en qué grupo se encuentra.

Para ello, las pantallas internas del grupo deben mantener una referencia visual al contexto actual mediante:

- Nombre del grupo.
- Escudo o imagen del grupo.
- Identificador visual consistente.

Esto funciona como un ancla permanente que responde a la pregunta:

> **"¿En qué grupo estoy ahora?"**

---

## 7. Uso en Contextos Reales

Fulbito está pensado para utilizarse principalmente en contextos sociales y deportivos, por lo que la interfaz debe considerar condiciones de uso diferentes a las de una aplicación convencional.

### Durante el partido

La interfaz debe priorizar:

- Marcador.
- Registro rápido de eventos.
- Botones grandes.
- Información esencial.
- Mínima cantidad de texto.
- Acciones accesibles con una mano.

### Antes del partido

La interfaz debe priorizar:

- Confirmación de asistencia.
- Cantidad de jugadores confirmados.
- Organización de equipos.
- Fecha y horario.
- Acceso rápido a la información del encuentro.

### Después del partido

La interfaz debe priorizar:

- Resultado final.
- Estadísticas.
- Goleadores.
- Asistencias.
- Historial.
- Ranking actualizado.

---

## 8. Principios de Experiencia

A partir de los criterios anteriores, Fulbito adopta los siguientes principios generales:

- **Fricción casi nula:** las acciones frecuentes deben requerir la menor cantidad posible de pasos.
- **Acciones visibles:** las funciones importantes no deben estar ocultas en menús innecesarios.
- **Feedback inmediato:** toda acción relevante debe producir una respuesta perceptible.
- **Prevención antes que corrección:** la interfaz debe evitar errores antes de depender de mecanismos de recuperación.
- **Contexto permanente:** el usuario debe saber siempre en qué grupo y sección se encuentra.
- **Consistencia:** los mismos componentes y colores deben mantener los mismos significados.
- **Accesibilidad:** la información importante debe poder percibirse independientemente de la capacidad del usuario para distinguir colores o utilizar ambas manos.
- **Diseño orientado al contexto:** la interfaz debe adaptarse a las necesidades del usuario antes, durante y después del partido.

---

> ## Resumen de Usabilidad
>
> La experiencia de Fulbito se basa en una interacción rápida, clara y predecible. La aplicación prioriza las acciones esenciales, minimiza el tipeo y utiliza componentes grandes y accesibles para facilitar su utilización durante un partido.
>
> La combinación de feedback inmediato, prevención de errores, navegación simple y mecanismos de recuperación permite mantener una experiencia fluida sin sacrificar la integridad del historial deportivo.