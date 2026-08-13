# ⚽ Reglas de Votación

> **Versión:** 1.0
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

# 1. Objetivo

Este documento define cómo se aprueban las acciones críticas de Fulbito (ver `07-roles-permissions.md`), dado que el sistema no cuenta con un rol individual habilitado para decidir unilateralmente sobre ellas.

---

# 2. Tipos de votación

## 2.1. Corrección de un partido finalizado

- **Quiénes votan (`eligibleVoters`):** únicamente los jugadores que participaron de ese partido específico (los que figuran en `players` del documento del `match`), ya que son quienes están en condiciones de confirmar lo ocurrido.
- **Criterio de aprobación:** mayoría simple de los votantes elegibles.
- **Fórmula:** `requiredToPass = floor(eligibleVoters.length / 2) + 1`

## 2.2. Expulsión de un integrante

- **Quiénes votan:** todos los integrantes activos del grupo, excluyendo al integrante sobre el cual se vota.
- **Criterio de aprobación:** mayoría simple de los votantes elegibles.
- **Fórmula:** `requiredToPass = floor(eligibleVoters.length / 2) + 1`

## 2.3. Eliminación completa de un grupo

- **Quiénes votan:** todos los integrantes activos del grupo.
- **Criterio de aprobación:** mayoría simple de los votantes elegibles.
- **Fórmula:** `requiredToPass = floor(eligibleVoters.length / 2) + 1`

---

# 3. Cálculo del umbral

El valor de `requiredToPass` **no es un número fijo**: se calcula dinámicamente mediante una Cloud Function en el momento en que se crea la solicitud de votación, en base a la cantidad de votantes elegibles en ese momento (`eligibleVoters.length`).

Esto evita el problema de tener un umbral fijo (por ejemplo, "5 votos") que resulte imposible de alcanzar en grupos con menos integrantes.

---

# 4. Empates y casos borde

- Con **mayoría simple**, un empate exacto no puede ocurrir salvo con cantidades de votantes elegibles que permitan una división exacta a favor de la mitad; en ese caso, la fórmula `floor(n/2) + 1` ya exige más de la mitad, por lo que un empate real nunca alcanza para aprobar la solicitud.
- Si `eligibleVoters.length` es 1 (por ejemplo, un partido con un único jugador confirmado), la solicitud requiere ese único voto a favor para aprobarse.

---

# 5. Resolución de la votación

1. Un integrante habilitado crea la solicitud (corrección, expulsión o eliminación de grupo).
2. El sistema calcula `eligibleVoters` y `requiredToPass`.
3. Los integrantes habilitados emiten su voto (a favor o en contra).
4. Al alcanzarse `requiredToPass` votos a favor, una Cloud Function aplica el cambio automáticamente:
   - Recalcula estadísticas (si corresponde).
   - Expulsa al integrante o elimina el grupo (si corresponde).
   - Marca la solicitud como **Aprobada**.
5. Si se alcanza un número de votos en contra que hace matemáticamente imposible llegar a `requiredToPass`, la solicitud se marca como **Rechazada**.

---

> ## Nota de diseño
>
> Ningún integrante puede aplicar una acción crítica de forma unilateral. El sistema siempre exige el consenso definido en este documento antes de ejecutar cambios permanentes sobre el grupo o su historial.