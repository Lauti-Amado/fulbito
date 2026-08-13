# ⚽ Requisitos No Funcionales (Non-Functional Requirements)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento define los atributos de calidad, restricciones técnicas y estándares que el sistema debe cumplir. A diferencia de los requisitos funcionales, estos especifican **cómo debe comportarse** el sistema y qué niveles de calidad debe alcanzar.

---

## NFR-001 — Usabilidad

- El registro completo de un partido no debería requerir más de 60 segundos para un usuario promedio.
- Registrar un evento durante un partido debe requerir un máximo de tres interacciones.
- La interfaz debe estar optimizada para dispositivos móviles y permitir un uso cómodo con una sola mano.

---

## NFR-002 — Rendimiento

- Las actualizaciones de un partido en curso deben reflejarse en los dispositivos de los integrantes del grupo en menos de 2 segundos bajo condiciones normales de conexión.
- La carga del historial y de las estadísticas debe completarse en menos de 2 segundos bajo condiciones normales de uso.

---

## NFR-003 — Compatibilidad

- La aplicación debe funcionar correctamente tanto en Android como en iOS.
- La interfaz debe adaptarse correctamente a distintos tamaños de pantalla de teléfonos móviles.

---

## NFR-004 — Seguridad

- Toda operación sobre la aplicación requiere una sesión válida de Firebase Authentication, ya sea anónima o vinculada a una cuenta completa (email, Google o Apple). No existe acceso a los datos sin una sesión autenticada.
- La creación de una sesión anónima debe ser automática y transparente para el usuario, sin requerir ninguna acción manual ni pantalla de login previa al uso de la aplicación.
- Cada usuario únicamente podrá acceder a la información de los grupos a los que pertenece, independientemente del tipo de sesión que posea.
- Los enlaces o códigos de invitación deberán ser suficientemente aleatorios para evitar accesos no autorizados.

---

## NFR-005 — Disponibilidad

- El sistema deberá estar disponible la mayor parte del tiempo para permitir el registro de partidos cuando los usuarios lo requieran.
- Ante pérdidas temporales de conexión, la aplicación deberá conservar los datos ingresados y sincronizarlos automáticamente cuando la conexión sea restablecida.

---

## NFR-006 — Escalabilidad

- El sistema deberá soportar el crecimiento del número de usuarios, grupos y partidos sin degradar significativamente su rendimiento.
- Las estadísticas deberán mantenerse disponibles en tiempos de respuesta aceptables aun cuando un grupo acumule una gran cantidad de partidos históricos.

---

## NFR-007 — Mantenibilidad

- La aplicación deberá diseñarse siguiendo principios que faciliten su mantenimiento y evolución.
- El código deberá favorecer la reutilización y la separación de responsabilidades.
- El sistema deberá registrar errores de ejecución para facilitar su diagnóstico y corrección.

---

## NFR-008 — Confiabilidad

- Las estadísticas mostradas por el sistema deberán ser consistentes con la información registrada en los partidos.
- El sistema no deberá perder información ante cierres inesperados de la aplicación o fallos temporales de conectividad.

---

## NFR-009 — Escalabilidad de datos

- El historial de partidos y las estadísticas deberán mantenerse íntegros durante toda la vida útil del grupo.
- El crecimiento del historial no deberá afectar perceptiblemente la experiencia del usuario.