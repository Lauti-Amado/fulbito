# ⚽ Sistema de Diseño (Design System)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento define la base visual y estructural de **Fulbito**. Actúa como la única fuente de la verdad para el diseño de la interfaz de usuario, garantizando consistencia, acelerando el desarrollo en React Native y asegurando que la aplicación transmita la estética profesional y premium que el proyecto exige.

---

## 1. Colores (Color Palette)

Para emular la estética del fútbol profesional y reducir la fatiga visual en entornos de alto contraste (como una cancha de noche o a pleno sol), la aplicación utilizará un **Tema Oscuro (Dark Mode)** como identidad visual principal.

### Colores de Marca y Fondo

- **Fondo Principal (Background):** `#121212` (Negro asfalto). Base de toda la aplicación.
- **Superficies (Cards / Modals):** `#1E1E1E` a `#2C2C2C`. Para crear jerarquía y elevación sin utilizar sombras pesadas.
- **Color Principal (Primary/Brand):** `#00D05E` (Verde Césped Vibrante). Se utiliza para acciones principales, botones de confirmación y para representar estados positivos.

### Colores Semánticos y Deportivos

- **Victoria / Aprobación:** `#00D05E` (Verde).
- **Empate / Advertencia / Tarjeta Amarilla:** `#FFC107` (Amarillo).
- **Derrota / Error / Tarjeta Roja / Eliminar:** `#FF4C4C` (Rojo Intenso).
- **Texto Principal:** `#FFFFFF` (Blanco puro) para máxima legibilidad.
- **Texto Secundario / Deshabilitado:** `#9E9E9E` (Gris medio).

### Regla de Accesibilidad

Los colores semánticos **no deben utilizarse como único medio para transmitir información**. Los estados importantes deben acompañarse de texto, iconografía o ambos.

Por ejemplo:

- `V` + verde = Victoria.
- `E` + amarillo = Empate.
- `D` + rojo = Derrota.

---

## 2. Tipografías (Typography)

La tipografía debe ser limpia, moderna y, sobre todo, **excelente para leer números**, dado que las estadísticas y los marcadores son elementos centrales de la aplicación.

La aplicación utilizará **Inter** como familia tipográfica principal por su buena legibilidad en números, estadísticas y marcadores.

### Jerarquía

- **Marcadores y números grandes:** `Bold` o `Black`, con tamaños entre `48px` y `64px`, adaptándose al contexto de la pantalla.
- **Títulos de pantalla:** `SemiBold`, `24px`.
- **Títulos de tarjetas:** `Medium`, `16px`.
- **Texto de cuerpo:** `Regular`, `14px`.
- **Metadatos:** `Regular`, `12px`, utilizando el color de texto secundario.

### Reglas tipográficas

- Se evitará utilizar múltiples familias tipográficas dentro de la aplicación.
- Los tamaños y pesos deberán mantenerse consistentes entre pantallas equivalentes.
- Los marcadores y estadísticas tendrán prioridad visual sobre información secundaria.
- El texto deberá mantener un contraste suficiente con respecto al fondo.

---

## 3. Iconografía (Iconography)

Los iconos deben ser sólidos, reconocibles al instante y de trazo suficientemente grueso para facilitar su lectura en pantallas pequeñas.

- **Estilo:** Rellenos (`Solid`) para acciones activas y contorneados (`Outline`) para acciones inactivas.
- **Biblioteca:** Se utilizará una biblioteca de iconos consistente en toda la aplicación para evitar diferencias visuales entre plataformas.
- **Emojis:** No se utilizarán emojis como iconografía principal de la interfaz.

### Metáforas deportivas

- **Gol:** Icono de balón de fútbol.
- **Asistencia:** Icono de botín o calzado deportivo.
- **Racha:** Indicadores acompañados por las iniciales:
  - `V` = Victoria.
  - `E` = Empate.
  - `D` = Derrota.
- **Historial:** Icono de calendario o historial.
- **Ranking:** Icono de trofeo o estadísticas.
- **Miembros:** Icono de grupo/personas.
- **Configuración:** Icono de engranaje.

### Tamaño táctil

Aunque un icono pueda medir visualmente `24x24px`, su **área táctil (hitbox) nunca deberá ser menor a `48x48px`**.

---

## 4. Botones (Buttons)

Los botones en Fulbito siguen el principio de **Thumb-Zone Design**, priorizando acciones grandes y fáciles de presionar con una sola mano.

### Botón Primario (Primary)

- **Uso:** Acción principal de la pantalla.
- **Ejemplos:** `Finalizar Partido`, `Crear Grupo`, `Confirmar Gol`, `Voy`.
- **Fondo:** `#00D05E`.
- **Texto:** Blanco, `Bold`.
- **Border Radius:** `12px`.
- **Altura mínima:** `56px`.

### Botón Secundario (Secondary)

- **Uso:** Acciones alternativas o de menor prioridad.
- **Ejemplos:** `Editar Formación`, `Compartir`, `No Voy`.
- **Fondo:** Transparente.
- **Borde:** `2px` Verde o Blanco.
- **Texto:** Del mismo color que el borde.
- **Altura mínima:** `56px`.

### Botón Terciario (Ghost)

- **Uso:** Acciones de baja prioridad.
- **Ejemplos:** `Cancelar`, `Volver`, `Salir del grupo`.
- **Estilo:** Solo texto, sin fondo ni borde.
- **Color:** Texto secundario o rojo cuando la acción sea destructiva.

### Regla de jerarquía

Cada pantalla debe tener **una acción primaria claramente identificable**. No se deben presentar múltiples botones primarios con el mismo nivel de importancia visual.

---

## 5. Tarjetas (Cards)

Las tarjetas agrupan información relacionada y permiten separar visualmente los elementos del fondo.

### Match Card (Tarjeta de Partido)

Debe mostrar:

- Fecha y hora.
- Modalidad del partido.
- Equipo A.
- Equipo B.
- Escudos o identificadores visuales de los equipos.
- Marcador cuando el partido haya finalizado.
- Estado del partido cuando corresponda.

**Estilo:**

- Fondo: `#1E1E1E`.
- Border Radius: `16px`.
- Padding interno: `16px`.
- Separación entre tarjetas: `16px`.

### Player Stats Card (Ficha de Jugador)

Debe mostrar:

- Avatar.
- Nombre.
- Posición en el ranking.
- Estadísticas relevantes.
- Racha visual.

**Estilo:**

- Layout horizontal (`Row`).
- Alineación vertical centrada.
- Permite tocar la tarjeta para acceder al detalle del jugador.

---

## 6. Campos de Texto (Inputs)

La entrada de datos debe ser mínima y estar limitada principalmente a acciones como crear o editar un grupo.

### Estado por defecto

- Fondo: `#2C2C2C`.
- Texto: `#FFFFFF`.
- Sin bordes visibles.

### Estado activo (Focus)

- Borde inferior o contorno completo en `#00D05E`.

### Estado de error

- Borde: `#FF4C4C`.
- Mensaje de error explícito debajo del campo.
- Ejemplo: `El nombre es obligatorio`.

### Dimensiones

- Altura mínima: `56px`.
- Área táctil cómoda para uso con una sola mano.

### Regla de uso

Durante un **partido en curso** no se deberá solicitar al usuario ingresar información mediante teclado. El registro de eventos debe realizarse mediante opciones predefinidas.

---

## 7. Espaciados (Spacing & Grid)

El sistema utilizará una **grilla estricta de 8 puntos (8pt Grid System)** para garantizar un ritmo vertical y horizontal predecible en React Native.

| Token | Valor | Uso |
| --- | ---: | --- |
| `xs` | `4px` | Separación mínima, por ejemplo entre un icono y su etiqueta. |
| `sm` | `8px` | Separación entre elementos muy relacionados. |
| `md` | `16px` | Padding estándar de pantallas y tarjetas. |
| `lg` | `24px` | Separación entre secciones relacionadas. |
| `xl` | `32px` | Separación entre bloques lógicos grandes. |
| `xxl` | `48px` | Espacio reservado para evitar que el contenido choque con la navegación inferior. |

### Regla general

Los márgenes, paddings y separaciones entre componentes deberán utilizar preferentemente valores pertenecientes a esta escala.

---

## 8. Estados Visuales (Visual States)

El sistema debe reaccionar a cada interacción del usuario para transmitir claramente que la acción fue registrada.

### Pressed (Presionado)

Los botones y tarjetas deberán proporcionar feedback inmediato al ser tocados mediante:

- Reducción de opacidad hasta aproximadamente `70%`.
- O una reducción de escala sutil mediante una animación.

El efecto deberá ser breve y no interferir con la interacción.

### Disabled (Deshabilitado)

- Fondo: `#424242`.
- Texto: `#9E9E9E`.
- Sin interacción disponible.

### Loading (Carga)

Para acciones que requieren procesamiento:

- El texto del botón primario será reemplazado temporalmente por un spinner.
- El tamaño del botón deberá mantenerse.
- El botón deberá bloquear interacciones adicionales para evitar acciones duplicadas.

Para cargas de pantalla:

- Se utilizarán **Skeleton Loaders** con la estructura aproximada del contenido.
- Se evitará depender exclusivamente de un spinner central.

### Empty States (Estados Vacíos)

Cuando no existan datos, se deberá mostrar:

1. Un icono o ilustración representativa.
2. Un mensaje breve y amigable.
3. Una explicación opcional.
4. Una acción primaria cuando corresponda.

Ejemplo:

> **Aún no hay partidos jugados.**  
> Registrá tu primer partido para comenzar a construir el historial.

---

## 9. Navegación (Navigation)

La navegación debe ser simple, consistente y orientada al contexto en el que se encuentra el usuario.

### Navegación Global

La aplicación tendrá una navegación global para acceder a:

- Inicio.
- Perfil.
- Grupos del usuario.

El usuario podrá seleccionar un grupo para ingresar a su contexto privado.

### Navegación dentro de un Grupo

Una vez dentro de un grupo, la navegación principal se realizará mediante una **Bottom Tab Navigation**.

Las cuatro secciones principales serán:

1. **Cartelera**
2. **Historial**
3. **Ranking**
4. **Miembros**

La barra inferior deberá:

- Mantener siempre la misma posición.
- Utilizar iconos consistentes.
- Mostrar claramente la pestaña activa.
- Mantener áreas táctiles de al menos `48x48px`.

### Headers

Las pantallas internas de un grupo deberán mostrar:

- Nombre del grupo.
- Acción de retroceso cuando corresponda.
- Acciones contextuales relevantes.

El header deberá permitir al usuario identificar rápidamente en qué grupo se encuentra.

### Aislamiento de contexto

Los datos mostrados dentro de un grupo pertenecen exclusivamente a ese grupo.

Para cambiar de grupo, el usuario deberá regresar a la navegación global y seleccionar otro grupo.

---

## 10. Modales y Bottom Sheets

Las interfaces emergentes deberán utilizarse para acciones que no requieren abandonar completamente el contexto actual.

### Bottom Sheets

Se utilizarán principalmente para acciones rápidas relacionadas con el partido:

- Seleccionar autor de un gol.
- Seleccionar autor de una asistencia.
- Votar una solicitud de corrección.
- Seleccionar opciones rápidas.

### Full-Screen Modals

Se utilizarán para procesos que requieren mayor concentración:

- Crear un grupo.
- Finalizar un partido.
- Revisar el resumen de un partido.
- Editar la distribución de los equipos.

Los modales deberán contar siempre con una forma clara de cerrarse o cancelarse.

---

## 11. Accesibilidad (Accessibility)

La interfaz deberá poder utilizarse en diferentes condiciones ambientales y por usuarios con distintas necesidades.

### Contraste

Se utilizará un contraste elevado entre texto y fondo.

La información importante deberá permanecer legible:

- En interiores.
- En exteriores.
- Bajo luz solar directa.
- Durante un partido nocturno.

### Independencia del color

Los estados no deberán depender exclusivamente del color.

Por ejemplo:

- `V` + verde = Victoria.
- `E` + amarillo = Empate.
- `D` + rojo = Derrota.

### Tamaño del texto

Los elementos críticos, especialmente:

- Marcadores.
- Botones principales.
- Estadísticas.
- Fechas importantes.

deberán utilizar tamaños suficientemente grandes para poder identificarse rápidamente.

### Áreas táctiles

Los elementos interactivos deberán disponer de un área táctil mínima de `48x48px`, incluso cuando el elemento visual sea más pequeño.

---

## 12. Feedback e Interacción

Toda acción importante debe proporcionar una respuesta inmediata al usuario.

### Feedback háptico

Se utilizará vibración breve para acciones relevantes, por ejemplo:

- Registrar un gol.
- Confirmar la finalización de un partido.
- Confirmar una acción importante.

### Feedback visual

Los componentes interactivos deberán cambiar visualmente al ser presionados.

Las operaciones que requieran procesamiento deberán mostrar claramente su estado de carga.

### Toasts

Las acciones secundarias podrán utilizar mensajes temporales en la parte inferior de la pantalla.

Ejemplos:

- `Enlace copiado`.
- `Grupo creado correctamente`.
- `Solicitud enviada`.

Los Toasts no deberán interrumpir la navegación ni requerir una acción adicional del usuario.

---

## 13. Prevención y Recuperación de Errores

Fulbito debe minimizar los errores humanos, especialmente durante un partido.

### Deshacer rápido

Después de registrar un gol deberá aparecer temporalmente una acción:

> **Deshacer**

Esta opción permitirá corregir rápidamente un registro accidental.

### Confirmación de acciones críticas

Las acciones que modifiquen permanentemente el estado del partido deberán requerir confirmación explícita.

Ejemplo:

> **¿Finalizar partido?**  
> Revisá el marcador y los eventos antes de confirmar.

### Correcciones posteriores

Una vez finalizado un partido, los cambios importantes no podrán realizarse unilateralmente.

El sistema utilizará el mecanismo de **solicitudes de corrección y votación colectiva** definido en la arquitectura funcional de Fulbito.

---

## 14. Principios Generales de Diseño

Todos los componentes y pantallas de Fulbito deberán respetar los siguientes principios:

1. **Fricción casi nula:** las acciones frecuentes deben requerir la menor cantidad posible de pasos.
2. **Mobile First:** la interfaz se diseña específicamente para dispositivos móviles.
3. **Thumb-Zone:** las acciones críticas deben poder realizarse cómodamente con una sola mano.
4. **Contexto claro:** el usuario debe saber siempre en qué grupo y sección se encuentra.
5. **Jerarquía visual:** la información más importante debe destacar inmediatamente.
6. **Consistencia:** un mismo componente debe comportarse de la misma manera en toda la aplicación.
7. **Accesibilidad:** la información no debe depender exclusivamente del color o de elementos pequeños.
8. **Feedback inmediato:** toda acción importante debe comunicar su resultado.
9. **Prevención de errores:** las acciones irreversibles deben requerir confirmación.
10. **Cero tipeo durante el partido:** registrar eventos deportivos debe ser rápido y táctil.

---

> **Principio de Diseño**
>
> Fulbito debe sentirse como una aplicación deportiva moderna y profesional: información clara, acciones rápidas y una interfaz que permita registrar lo importante durante un partido sin distraer al usuario.
>
> **La mejor interfaz es la que no se nota. En Fulbito, el diseño no compite con el partido; lo inmortaliza.**