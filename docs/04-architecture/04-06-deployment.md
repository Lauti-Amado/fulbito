# ⚽ Despliegue (Deployment)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento describe la estrategia de despliegue del MVP de **Fulbito**, incluyendo la publicación de la aplicación móvil, la configuración del backend y la administración de los distintos entornos.

La arquitectura basada en **React Native (Expo)** y **Firebase** permite construir y desplegar el sistema sin necesidad de administrar servidores propios, reduciendo la complejidad operativa y facilitando la evolución del producto.

---

# 1. Arquitectura de Despliegue

El proceso de despliegue contempla dos componentes principales: la aplicación móvil y los servicios en la nube.

```text
                 Repositorio de Código
                         │
          ┌──────────────┴──────────────┐
          │                             │
          ▼                             ▼
 Aplicación Móvil                Backend Firebase
 (React Native + Expo)      (Firestore, Functions,
                             Storage y Auth)
          │                             │
          ▼                             ▼
      EAS Build                 Firebase Deploy
          │                             │
          ▼                             ▼
 Android / iOS                Servicios en Producción
```

---

# 2. Despliegue de la Aplicación Móvil

La aplicación será distribuida utilizando el ecosistema de Expo.

## Expo Application Services (EAS)

### Responsabilidad

Generar los paquetes nativos necesarios para Android e iOS a partir del código fuente de React Native.

### Beneficios

- Compilación en la nube.
- Una única base de código para Android e iOS.
- Simplificación del proceso de publicación.
- Integración con las tiendas oficiales.

### Artefactos generados

Cada compilación produce:

- Android App Bundle (`.aab`) para Google Play.
- Archivo (`.ipa`) para App Store.

---

# 3. Despliegue de Firebase

Todos los servicios backend son administrados mediante Firebase.

Durante el despliegue pueden publicarse de forma independiente:

- Cloud Firestore.
- Reglas de Seguridad (Security Rules).
- Cloud Functions.
- Firebase Storage.
- Configuración de Firebase Authentication.

Esta separación permite actualizar componentes específicos sin afectar el resto del sistema.

---

# 4. Gestión de Entornos

Para reducir riesgos durante el desarrollo se utilizarán distintos entornos.

## Desarrollo (Development)

Entorno destinado al desarrollo cotidiano del equipo.

### Características

- Base de datos independiente.
- Datos de prueba.
- Cloud Functions de prueba.
- Uso mediante Expo Go o builds internas.

---

## Producción (Production)

Entorno utilizado por los usuarios finales.

### Características

- Datos reales.
- Reglas de seguridad definitivas.
- Cloud Functions de producción.
- Aplicaciones distribuidas mediante Google Play y App Store.

---

# 5. Estrategia de Versionado

Cada nueva versión de la aplicación deberá mantener compatibilidad con el modelo de datos utilizado en producción.

Siempre que una modificación implique cambios en la estructura de Firestore, el despliegue deberá realizarse en el siguiente orden:

1. Preparar y desplegar los cambios de backend compatibles.
2. Actualizar las Security Rules cuando corresponda.
3. Publicar la nueva versión de la aplicación móvil.

De esta forma se evita que versiones antiguas de la aplicación interactúen incorrectamente con el nuevo modelo de datos.

---

# 6. Recuperación ante Fallos

La arquitectura aprovecha las capacidades administradas de Firebase para aumentar la disponibilidad del sistema.

En caso de fallos:

- Cloud Firestore mantiene la persistencia de los datos.
- Cloud Functions pueden reintentarse automáticamente cuando corresponda.
- Firebase Authentication continúa administrando la identidad de los usuarios.
- Firebase Storage conserva los archivos independientemente del cliente móvil.

Además, la persistencia local de Firestore permite que la aplicación continúe funcionando temporalmente sin conexión y sincronice automáticamente los cambios cuando el dispositivo recupere acceso a Internet.

---

# 7. Escalabilidad

La infraestructura elegida permite que la capacidad del sistema crezca automáticamente según la demanda.

No es necesario aprovisionar servidores manualmente.

Los principales servicios utilizados (Cloud Firestore, Cloud Functions, Firebase Storage y Firebase Authentication) escalan de forma administrada, permitiendo soportar el crecimiento del número de grupos, partidos y usuarios sin modificar la arquitectura del sistema.

---

> ## Resumen
>
> La estrategia de despliegue de Fulbito busca minimizar la complejidad operativa mediante servicios administrados. La separación entre entornos, el uso de infraestructura serverless y la publicación independiente del cliente móvil y del backend permiten evolucionar el sistema de forma segura, escalable y mantenible.