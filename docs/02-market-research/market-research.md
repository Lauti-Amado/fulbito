# ⚽ Fulbito - Investigación de Mercado

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 06/08/2026 | Lautaro Amado | Creación del documento |

---

# 1. Objetivo del análisis

El objetivo de este documento es analizar el panorama actual de aplicaciones deportivas y herramientas de gestión relacionadas con el fútbol amateur. Buscamos entender cómo los jugadores resuelven actualmente la organización de sus partidos, identificar las falencias de las soluciones existentes y validar la propuesta de valor de Fulbito: ser la herramienta definitiva para conservar el historial y las estadísticas de los grupos de fútbol amateur.

# 2. Problema identificado

El ecosistema actual está fragmentado. Para organizar un partido, los grupos dependen de WhatsApp (que es caótico y efímero). Si falta un jugador, usan plataformas públicas para buscar desconocidos. Si quieren jugar un torneo, se someten a sistemas de gestión pesados y burocráticos. 

Sin embargo, el grupo de amigos que juega todas las semanas no tiene un espacio propio, privado y ágil para guardar lo que realmente les importa: quién ganó, quién hizo los goles, cómo quedó el historial y cuáles son las anécdotas del post-partido. Esa información hoy se pierde.

# 3. Competidores

Para entender el mercado, analizamos aplicaciones dividiéndolas en tres categorías según su enfoque:

*   **Competencia Directa:** Soluciones que intentan organizar el fútbol amateur (Picadito App, Ticas, Fubles).
*   **Competencia Indirecta:** Herramientas que resuelven partes específicas del problema, ya sea a nivel administrativo o informativo (Copa Fácil, Deporstar, OneFootball, Promiedos).
*   **Inspiración UX/UI:** Aplicaciones referentes en la gestión de equipos y dinámicas móviles (TeamSnap, Heja, Copero).

# 4. Análisis de los competidores

Cada competidor fue analizado en profundidad en documentos individuales. A continuación, se presenta la matriz de comparación que resume el panorama general:

| Aplicación | Organización | Estadísticas | Historial | Buscar rivales | Ranking | App móvil | Grupo privado | ¿Qué deja sin resolver? |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Fulbito** | ✅ | ✅ | ✅ | 🚧 | ✅ | ✅ | ✅ | - |
| **Picadito App** | ✅ | ❌ | ❌ | ✅ | ❌ | ✅ | ❌ | El registro histórico y las estadísticas del grupo cerrado. |
| **Ticas** | ✅ | ✅ | ✅ | ❌ | ❌ | ⚠️ | ✅ | Una experiencia móvil nativa, rápida y sin fricción de carga. |
| **Fubles** | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ | La privacidad. Enfocado en extraños, no en el grupo. |
| **Copa Fácil** | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | La agilidad. Son demasiado pesadas para un partido informal. |
| **Heja / TeamSnap**| ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | El aspecto lúdico del fútbol (estadísticas, goles, rachas). |

*(Para ver el análisis detallado de cada herramienta, consultar los archivos .md correspondientes en este directorio).*

# 5. Oportunidades detectadas

1.  **El vacío del "Post-Partido":** Casi todas las apps terminan su trabajo cuando el árbitro pita el inicio. Nadie capitaliza la emoción del tercer tiempo y la carga de resultados.
2.  **Fricción de carga:** Las apps que permiten llevar historiales requieren configuraciones previas (torneos, temporadas, planillas). Hay un espacio enorme para una carga de datos de "un solo toque".
3.  **Estética profesional para amateurs:** El jugador amateur consume Promiedos u OneFootball todos los días. Si le damos esa misma calidad visual a su partido de los jueves, el sentido de pertenencia y retención será altísimo.

# 6. Diferenciadores de Fulbito

*   **Privacidad por diseño:** No somos una red social abierta para buscar desconocidos. Somos el diario íntimo del grupo.
*   **Fricción casi nula:** Cargar un partido toma menos de un minuto. El diseño móvil nativo y la sincronización en tiempo real deben garantizar que la app nunca se interponga entre el usuario y la diversión.
*   **Horizontalidad:** No hay "delegados" ni "administradores de liga". Todos los amigos tienen el mismo peso para cargar goles y ver estadísticas.

# 7. Principios que guiarán el desarrollo

A partir de esta investigación se establecen los siguientes principios para el desarrollo de Fulbito:

- Priorizar siempre la simplicidad sobre la cantidad de funcionalidades.
- Evitar cualquier flujo que requiera configuraciones innecesarias.
- Diseñar primero para dispositivos móviles.
- Mantener la experiencia centrada en grupos privados.
- Inspirarse visualmente en aplicaciones deportivas profesionales.
- No competir con WhatsApp, sino complementarlo.
- Registrar un partido debe ser un proceso rápido y natural.

# 8. Conclusiones

La investigación valida fuertemente la visión de Fulbito. Mientras el mercado se pelea por organizar el *próximo* partido o administrar el torneo de los domingos, Fulbito busca convertirse en el lugar donde la historia de cada grupo permanezca en el tiempo. Para un análisis más profundo sobre las decisiones que tomaremos a partir de estos datos, referirse al archivo `conclusions.md`.

/* ## Trazabilidad

Las conclusiones de este análisis impactan directamente en los siguientes documentos:

- 03-personas.md
- 04-user-stories.md
- 05-mvp.md
- 09-user-flows.md /*