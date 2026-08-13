# ⚽ Visión General del Sistema (System Overview)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento presenta la arquitectura de alto nivel elegida para el desarrollo del MVP de **Fulbito**.

El objetivo de esta arquitectura es proporcionar una aplicación móvil rápida, escalable y sencilla de mantener, aprovechando un enfoque **Serverless (Backend as a Service - BaaS)** que reduzca la complejidad operativa y permita concentrar el desarrollo en la experiencia del usuario.

---

# 1. Arquitectura General

La solución se compone de los siguientes elementos principales:

```text
                ┌─────────────────────────────┐
                │   React Native + Expo       │
                │      (Cliente móvil)        │
                └──────────────┬──────────────┘
                               │
                               ▼
                ┌─────────────────────────────┐
                │ Firebase Authentication     │
                └──────────────┬──────────────┘
                               │
                               ▼
                ┌─────────────────────────────┐
                │      Cloud Firestore        │
                └──────────────┬──────────────┘
                               │
                  Disparadores (Triggers)
                               │
                               ▼
                ┌─────────────────────────────┐
                │     Cloud Functions         │
                └──────────────┬──────────────┘
                               │
                               ▼
                ┌─────────────────────────────┐
                │      Cloud Firestore        │
                └───────┬───────────┬─────────┘
                        │           │
                        ▼           ▼
             Firebase Storage     Firebase Cloud
                                  Messaging (FCM)
```

---

# 2. Componentes de la Arquitectura

Cada componente cumple una responsabilidad específica dentro del sistema.

## Cliente móvil — React Native + Expo

**Rol**

Implementar toda la interfaz de usuario y la lógica de presentación de la aplicación.

**Justificación**

React Native permite desarrollar una única aplicación para Android e iOS utilizando una sola base de código, reduciendo el esfuerzo de mantenimiento y garantizando una experiencia consistente entre plataformas.

Expo simplifica el desarrollo, las pruebas y la distribución durante las primeras etapas del proyecto, acelerando la construcción del MVP.

---

## Autenticación — Firebase Authentication

**Rol**

Gestionar la identidad de los usuarios y controlar el acceso a la aplicación.

**Justificación**

Firebase Authentication proporciona un sistema seguro y ampliamente probado para el registro e inicio de sesión de usuarios, además de permitir autenticación mediante correo electrónico o proveedores externos sin necesidad de implementar un sistema propio.

Este componente satisface principalmente los requisitos funcionales relacionados con el acceso de usuarios y contribuye a los requisitos de seguridad del sistema.

---

## Base de Datos — Cloud Firestore

**Rol**

Almacenar toda la información principal de Fulbito.

Entre otros:

- Usuarios
- Grupos
- Partidos
- Equipos
- Eventos
- Estadísticas

**Justificación**

Cloud Firestore es una base de datos NoSQL altamente escalable que ofrece sincronización en tiempo real.

Gracias a sus listeners, cualquier modificación realizada por un integrante del grupo se refleja automáticamente en los dispositivos del resto de los participantes sin necesidad de recargar la aplicación.

Esta característica resulta fundamental para mantener actualizado el marcador durante un partido.

---

## Lógica de Negocio — Cloud Functions

**Rol**

Ejecutar procesos del servidor que no deben depender del cliente.

Por ejemplo:

- Actualizar estadísticas históricas.
- Recalcular rankings.
- Procesar correcciones aprobadas.
- Ejecutar lógica sensible para la integridad de los datos.

**Justificación**

Aunque la aplicación funciona principalmente sobre Firestore, ciertas operaciones no deberían ejecutarse desde el dispositivo del usuario para evitar manipulaciones o inconsistencias.

Las Cloud Functions permiten centralizar esta lógica en un entorno seguro que se ejecuta automáticamente ante determinados eventos de la base de datos.

---

## Almacenamiento de Archivos — Firebase Storage

**Rol**

Almacenar archivos generados o utilizados por los usuarios.

Por ejemplo:

- Fotos de perfil.
- Imágenes de los grupos.
- Recursos compartidos.

**Justificación**

Firebase Storage se integra de forma nativa con el resto del ecosistema Firebase y permite almacenar archivos de forma segura y escalable sin aumentar la complejidad del backend.

---

## Notificaciones Push — Firebase Cloud Messaging (FCM)

**Rol**

Enviar notificaciones a los dispositivos móviles.

**Justificación**

Las notificaciones permiten mantener informados a los integrantes del grupo sobre eventos importantes, como:

- creación de un nuevo partido;
- invitaciones a un grupo;
- solicitudes de corrección;
- aprobación de cambios.

Esto mejora la interacción entre los miembros sin requerir que la aplicación permanezca abierta.

---

# 3. Flujo General del Sistema

En términos generales, el funcionamiento de Fulbito sigue el siguiente flujo:

1. El usuario inicia sesión mediante Firebase Authentication.
2. La aplicación obtiene los datos almacenados en Cloud Firestore.
3. El usuario interactúa con la aplicación (crear grupos, registrar partidos, consultar estadísticas, etc.).
4. Los cambios se almacenan inmediatamente en Firestore.
5. Cuando ocurre un evento que requiere procesamiento adicional, una Cloud Function ejecuta la lógica correspondiente.
6. Las modificaciones vuelven a almacenarse en Firestore.
7. Los dispositivos conectados reciben automáticamente la información actualizada gracias a la sincronización en tiempo real.

---

# 4. Beneficios de la Arquitectura

La arquitectura seleccionada proporciona las siguientes ventajas para el proyecto:

- Desarrollo rápido del MVP.
- Escalabilidad automática sin administrar servidores.
- Sincronización de datos en tiempo real.
- Reducción de costos operativos.
- Integración nativa entre todos los servicios utilizados.
- Alta mantenibilidad gracias a la separación de responsabilidades.
- Facilidad para incorporar nuevas funcionalidades en futuras versiones.

---

# 5. Responsabilidades

| Componente | Responsabilidad |
|------------|-----------------|
| React Native | Presentación |
| Authentication | Identidad |
| Firestore | Persistencia |
| Cloud Functions | Lógica del negocio |
| Storage | Archivos |
| FCM | Comunicación |

---

> **Resumen Arquitectónico**
>
> La arquitectura de Fulbito prioriza la simplicidad, la escalabilidad y la experiencia en tiempo real. Al delegar la infraestructura en servicios administrados de Firebase, el proyecto puede concentrarse en ofrecer una aplicación móvil rápida, intuitiva y confiable para registrar y conservar la historia deportiva de cada grupo de fútbol.