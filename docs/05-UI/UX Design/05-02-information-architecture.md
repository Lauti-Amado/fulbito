# ⚽ Arquitectura de la Información (Information Architecture)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento define la estructura lógica, la organización del contenido y la jerarquía de navegación de **Fulbito**.

El objetivo principal de la arquitectura de la información es garantizar que el jugador encuentre lo que busca rápidamente, manteniendo una experiencia de **fricción casi nula** y un aislamiento estricto entre los distintos grupos a los que pertenece.

La estructura diferencia claramente el contexto **global del usuario** del contexto **privado de cada grupo**, evitando mezclar información, estadísticas o acciones entre diferentes grupos.

---

# 1. Mapa de Navegación (Sitemap)

La estructura de la aplicación se divide en dos grandes niveles: la capa global, donde el usuario gestiona sus grupos y su cuenta, y la capa privada, donde interactúa con un grupo específico.

El acceso a la aplicación **no requiere autenticación obligatoria**. El usuario puede comenzar a utilizar Fulbito como invitado. La creación de una cuenta será opcional y estará orientada principalmente a conservar y sincronizar los datos.

```text
Aplicación (Raíz)
│
├── Acceso
│   ├── Continuar como invitado
│   └── Cuenta opcional
│       ├── Iniciar sesión
│       └── Crear cuenta
│
└── Contexto Global
    │
    ├── Inicio (Mis Grupos)
    │   ├── Crear Grupo
    │   ├── Unirse a Grupo por Código
    │   └── Seleccionar Grupo
    │
    ├── Perfil / Cuenta
    │   ├── Datos personales
    │   ├── Apodo
    │   ├── Foto de perfil
    │   └── Gestión de cuenta
    │
    └── Grupo (Contexto Privado)
        │
        ├── Cartelera (Dashboard)
        │   ├── Estado del Grupo
        │   ├── Próximo Partido
        │   │   └── Confirmación de Asistencia (RSVP)
        │   ├── Armado de Equipos
        │   └── Partido en Curso
        │       ├── Marcador
        │       ├── Registro de Gol
        │       ├── Registro de Asistencia
        │       └── Registro de Autogol
        │
        ├── Historial
        │   ├── Lista de Partidos
        │   └── Detalle de Partido
        │       └── Solicitud de Corrección
        │           └── Votación
        │
        ├── Ranking
        │   ├── Tabla General
        │   └── Perfil Deportivo del Jugador
        │
        └── Miembros
            ├── Lista de Jugadores
            ├── Invitar Jugadores
            └── Ajustes del Grupo
                ├── Nombre
                ├── Imagen
                └── Modalidad Predeterminada
```

---

# 2. Separación de Contextos

Una decisión fundamental de la arquitectura de información es separar el contexto **global** del usuario del contexto **privado del grupo**.

## 2.1. Contexto Global

El contexto global contiene información independiente de cualquier grupo.

Incluye:

- Lista de grupos a los que pertenece el usuario.
- Creación de nuevos grupos.
- Ingreso a grupos mediante invitaciones.
- Perfil personal.
- Configuración de la cuenta.
- Acceso opcional a autenticación y sincronización.

En este nivel no se muestran estadísticas deportivas pertenecientes a un grupo específico.

### Acceso sin cuenta

Fulbito prioriza la reducción de fricción inicial, por lo que **no se requiere crear una cuenta para comenzar a utilizar la aplicación**.

El usuario podrá:

- Abrir la aplicación.
- Crear o unirse a un grupo.
- Participar en partidos.
- Registrar resultados y eventos.
- Consultar estadísticas.

La cuenta será una funcionalidad opcional para quienes quieran conservar y sincronizar sus datos entre dispositivos.

```text
Abrir Fulbito
      │
      ▼
Continuar como invitado
      │
      ▼
Inicio
      │
      ├── Crear Grupo
      ├── Unirse a Grupo
      └── Seleccionar Grupo
```

Si el usuario decide crear una cuenta posteriormente, podrá hacerlo desde su perfil o desde una acción contextual de guardado/sincronización.

---

## 2.2. Contexto del Grupo

Cuando el usuario selecciona un grupo, la aplicación entra en un contexto completamente independiente.

Dentro de este contexto se encuentran:

- Partidos.
- Jugadores.
- Ranking.
- Historial.
- Eventos deportivos.
- Solicitudes de corrección.
- Configuración del grupo.

Toda la información mostrada pertenece exclusivamente al grupo seleccionado.

> **Principio de aislamiento:** el usuario nunca debe interpretar que está viendo información combinada de distintos grupos.

Para cambiar de grupo, el usuario deberá regresar al **Inicio Global**, seleccionar otro grupo y volver a ingresar a su contexto.

---

# 3. Paradigmas de Navegación

La navegación de Fulbito utilizará una combinación de **Stack Navigation**, **Bottom Tab Navigation**, **Modals** y **Bottom Sheets**, seleccionando cada mecanismo según el tipo de interacción.

---

## 3.1. Navegación Global

La navegación global permite desplazarse entre las áreas independientes de un grupo.

El flujo general será:

```text
                 FULBITO
                    │
                    ▼
             Inicio Global
              /         \
             ▼           ▼
      Perfil / Cuenta   Grupo
                          │
                          ▼
                  Contexto del Grupo
```

Desde **Inicio Global**, el usuario podrá:

- Seleccionar un grupo.
- Crear un nuevo grupo.
- Unirse mediante un código de invitación.
- Acceder a su perfil o cuenta.

El usuario no accederá directamente al contenido de un grupo sin haber seleccionado previamente el contexto correspondiente.

La navegación global no utilizará las mismas Bottom Tabs que existen dentro del grupo, ya que se trata de un contexto diferente.

---

# 4. Navegación del Grupo — Bottom Tabs

Una vez dentro de un grupo, la navegación principal se realizará mediante una **Bottom Tab Navigation**.

Las cuatro pestañas serán:

1. **Cartelera**
2. **Historial**
3. **Ranking**
4. **Miembros**

La barra inferior permanecerá consistente durante todo el recorrido dentro del grupo.

---

## 4.1. Cartelera

Es la pantalla principal del grupo y representa el estado actual de la actividad deportiva.

Su contenido puede variar según el estado del grupo:

```text
Sin partido
    │
    ▼
Partido programado
    │
    ▼
Partido en curso
    │
    ▼
Partido finalizado
```

La Cartelera prioriza siempre la información relacionada con el partido actual o el próximo encuentro.

---

## 4.2. Historial

Contiene los partidos finalizados y permite consultar su información detallada.

Los partidos se muestran ordenados cronológicamente y pueden seleccionarse para acceder a su detalle.

---

## 4.3. Ranking

Muestra las estadísticas acumuladas de los jugadores dentro del grupo.

La información pertenece exclusivamente al grupo seleccionado.

---

## 4.4. Miembros

Permite consultar a los integrantes del grupo.

Como Fulbito no posee roles jerárquicos, todos los integrantes acceden a las mismas acciones desde esta sección:

- Invitaciones.
- Configuración general del grupo.

Las acciones consideradas críticas (expulsar a un integrante, eliminar el grupo) se inician desde aquí, pero su aplicación depende de alcanzar la aprobación mediante votación colectiva, no de un permiso individual (ver `03-requirements/08-voting-rules.md`).

---

# 5. Navegación Secundaria

Dentro de cada pestaña podrán existir pantallas secundarias.

Por ejemplo:

```text
Historial
   │
   ├── Lista de Partidos
   │
   └── Detalle de Partido
           │
           └── Solicitud de Corrección
                    │
                    └── Votación
```

Y:

```text
Ranking
   │
   ├── Tabla General
   │
   └── Perfil Deportivo
```

Estas vistas utilizarán **Stack Navigation**, permitiendo avanzar hacia un nivel de mayor detalle y regresar mediante el botón nativo de retroceso.

Las pantallas secundarias deberán mantener visible el contexto del grupo mediante el header correspondiente.

---

# 6. Flujos Emergentes (Modals & Bottom Sheets)

Las acciones rápidas o aquellas que requieren una interacción puntual utilizarán elementos emergentes para evitar una navegación innecesaria.

---

## 6.1. Bottom Sheets

Los **Bottom Sheets** aparecerán desde la parte inferior de la pantalla y se utilizarán principalmente para acciones rápidas.

Ejemplos:

- Seleccionar autor de un gol.
- Seleccionar autor de una asistencia.
- Registrar un autogol.
- Emitir un voto sobre una corrección.
- Seleccionar un jugador.
- Realizar acciones rápidas relacionadas con el partido.

Su objetivo es mantener al usuario dentro del contexto actual y reducir la cantidad de navegación necesaria.

---

## 6.2. Modals

Los **Modals** se utilizarán para acciones que requieran introducir información o confirmar una operación sin abandonar completamente la pantalla actual.

Ejemplos:

- Crear un grupo.
- Unirse mediante código.
- Editar información del grupo.
- Crear un partido.
- Confirmar determinadas acciones.
- Configurar información puntual.

Los modales no deberán utilizarse para procesos que requieran una navegación extensa o una gran cantidad de información.

---

## 6.3. Full-Screen Views

Las operaciones que requieran concentración o presenten una cantidad considerable de información podrán utilizar una vista de pantalla completa.

Ejemplos:

- Configuración de equipos.
- Resumen de un partido finalizado.
- Flujo de finalización de partido.
- Procesos complejos de edición.

Estas vistas deberán mantener siempre una acción clara para cancelar o regresar.

---

# 7. Jerarquía de Contenidos por Pantalla

La información se organizará siguiendo una jerarquía basada en **frecuencia de uso, importancia y urgencia**.

---

## 7.1. Cartelera (Dashboard)

Es la pantalla más dinámica del grupo.

La información principal ocupará la zona superior de la pantalla.

### Sin partido próximo

Se mostrará un **Empty State** indicando que todavía no existe un partido programado.

Ejemplo:

> "Todavía no hay partidos programados."

Y una acción principal:

> **Crear partido**

---

### Partido programado

La prioridad será mostrar:

1. Fecha y hora.
2. Modalidad.
3. Estado de asistencia.
4. Cantidad de jugadores confirmados.
5. Acción para confirmar o rechazar asistencia.
6. Acceso al armado de equipos cuando corresponda.

---

### Partido en curso

El marcador y las acciones relacionadas con el registro de eventos tendrán máxima prioridad.

Se mostrará:

1. Marcador.
2. Equipos.
3. Jugadores.
4. Eventos recientes.
5. Acción para registrar eventos.
6. Acción para finalizar el partido.

---

# 8. Historial

El Historial representa el archivo deportivo del grupo.

Se utilizará una lista vertical de **Match Cards**, ordenadas cronológicamente.

Cada tarjeta mostrará como mínimo:

- Fecha.
- Modalidad.
- Equipos.
- Marcador.
- Resultado.
- Principales eventos del partido.

La información suficiente deberá estar disponible en la tarjeta para que el usuario pueda reconocer rápidamente el partido sin necesidad de abrirlo.

Al tocar una tarjeta se accederá al **Detalle del Partido**.

---

# 9. Detalle de Partido

El detalle permite consultar toda la información registrada durante un encuentro.

La información se organizará en secciones:

```text
Detalle del Partido
│
├── Fecha y modalidad
│
├── Resultado
│
├── Equipo A
│   └── Jugadores
│
├── Equipo B
│   └── Jugadores
│
├── Eventos
│   ├── Goles
│   ├── Asistencias
│   └── Autogoles
│
└── Correcciones
    └── Solicitar corrección
```

Los partidos finalizados serán tratados como parte del historial y no podrán modificarse directamente desde esta pantalla.

Si un usuario detecta un error, deberá utilizar el flujo de **Solicitud de Corrección**.

---

# 10. Ranking

El Ranking presenta las estadísticas acumuladas de los integrantes del grupo.

La información principal se organizará de manera compacta para facilitar la comparación.

```text
Pos. | Jugador | PJ | G | A | Racha
--------------------------------------
  1  | Lauti   | 15 | 24| 8 | V V E V V
  2  | Juan    | 14 | 21| 5 | V E V V D
```

Los principales datos serán:

- Posición.
- Avatar.
- Nombre o apodo.
- Partidos jugados.
- Goles.
- Asistencias.
- Racha.

La racha deberá utilizar las letras **V, E y D**, acompañadas por una representación visual que no dependa exclusivamente del color.

Al seleccionar un jugador se accederá a su **Perfil Deportivo dentro del grupo**.

> Las estadísticas mostradas corresponden exclusivamente al grupo seleccionado y nunca se combinan con estadísticas de otros grupos.

---

# 11. Miembros

La sección Miembros permite consultar a los integrantes del grupo.

Fulbito no posee roles jerárquicos (ver `03-requirements/07-roles-permissions.md`), por lo que la información y las acciones disponibles son las mismas para todos los integrantes. Para cada jugador se mostrará:

- Avatar.
- Nombre.
- Apodo.

Cualquier integrante puede acceder, sin restricciones adicionales, a:

- Generación de invitaciones.
- Configuración general del grupo (nombre, imagen, modalidad predeterminada).

Las únicas acciones restringidas son las consideradas críticas (expulsar a un integrante, eliminar el grupo), que no dependen de un permiso individual sino de alcanzar la aprobación mediante votación colectiva definida en `03-requirements/08-voting-rules.md`.

---

# 12. Perfil Global / Cuenta

El perfil global representa la identidad del usuario dentro de toda la aplicación.

Contendrá:

- Foto de perfil.
- Nombre.
- Apodo.
- Correo electrónico, si existe una cuenta.
- Opciones de edición.
- Estado de sincronización de datos.
- Opción para crear o vincular una cuenta.

Las estadísticas deportivas no se mostrarán como estadísticas globales, ya que pertenecen al contexto de cada grupo.

Para consultar las estadísticas deportivas, el usuario deberá ingresar al Ranking del grupo correspondiente.

### Cuenta opcional

La cuenta no será obligatoria para utilizar las funcionalidades principales de Fulbito.

Su finalidad será permitir:

- Guardar los datos de forma persistente.
- Sincronizar la información entre dispositivos.
- Recuperar los datos en caso de cambiar de dispositivo.
- Mantener la identidad del usuario asociada a sus grupos.

El usuario podrá comenzar como invitado y posteriormente decidir si desea crear o vincular una cuenta.

---

# 13. Principios de Organización de la Información

La arquitectura de información de Fulbito seguirá los siguientes principios:

### 1. Contexto visible

El grupo seleccionado deberá estar claramente identificado mientras el usuario se encuentre dentro de él.

### 2. Jerarquía clara

La información más importante debe aparecer primero y ocupar mayor espacio visual.

### 3. Fricción mínima

Las acciones frecuentes deberán requerir la menor cantidad posible de pasos.

### 4. Aislamiento de grupos

Nunca se deberán mezclar datos, partidos o estadísticas pertenecientes a diferentes grupos.

### 5. Consistencia

Los mismos tipos de información deberán presentarse siempre de la misma manera.

### 6. Acciones contextuales

Las acciones deberán aparecer cerca de la información sobre la que actúan.

### 7. Feedback inmediato

Toda acción realizada por el usuario deberá producir una respuesta visual que confirme su ejecución.

### 8. Accesibilidad táctil

Los elementos interactivos deberán respetar las dimensiones mínimas definidas en el Design System, especialmente durante el registro de eventos en un partido.

### 9. Acceso sin fricción

La aplicación no deberá exigir una cuenta para comenzar a utilizar sus funcionalidades principales.

### 10. Cuenta como funcionalidad opcional

La autenticación se utilizará principalmente para persistencia, recuperación y sincronización de datos, no como barrera de entrada.

---

# 14. Resumen de Navegación

La estructura general puede resumirse de la siguiente manera:

```text
                         FULBITO
                            │
                            ▼
                     Inicio Global
                      /          \
                     /            \
                    ▼              ▼
             Perfil / Cuenta    Mis Grupos
                                  │
                       ┌──────────┼──────────┐
                       │          │          │
                  Crear Grupo   Unirse    Seleccionar
                                              Grupo
                                                │
                                                ▼
                                      ┌─────────────────┐
                                      │      GRUPO      │
                                      └────────┬────────┘
                                               │
                    ┌──────────────┬───────────┼──────────────┐
                    │              │           │              │
                Cartelera      Historial    Ranking       Miembros
                    │              │           │              │
                    │              │           │              ├── Jugadores
                    │              │           │              ├── Invitaciones
                    │              │           │              └── Ajustes
                    │              │           │
                    │              │           └── Perfil Deportivo
                    │              │
                    │              └── Detalle de Partido
                    │                         │
                    │                         └── Corrección / Votación
                    │
                    ├── Próximo Partido
                    │      └── RSVP
                    │
                    ├── Armado de Equipos
                    │
                    └── Partido en Curso
                           │
                           ├── Marcador
                           ├── Registro de Gol
                           ├── Registro de Asistencia
                           └── Registro de Autogol
```

---

> ## Resumen
>
> La arquitectura de información de Fulbito separa claramente la identidad global del usuario del contexto privado de cada grupo. Dentro de cada grupo, las cuatro secciones principales —**Cartelera, Historial, Ranking y Miembros**— permiten acceder rápidamente a las funcionalidades centrales de la aplicación.
>
> La aplicación prioriza un acceso **sin registro obligatorio**, permitiendo comenzar como invitado y ofreciendo una cuenta opcional para guardar, recuperar y sincronizar la información.
>
> La combinación de **Bottom Tabs, Stack Navigation, Modals y Bottom Sheets** busca minimizar la cantidad de pasos necesarios para realizar las acciones frecuentes, especialmente durante un partido, donde la velocidad y la facilidad de interacción son prioritarias.
>
> Esta estructura servirá como base para la definición de los wireframes, prototipos y flujos de usuario de la documentación de UI/UX.