# ⚽ Seguridad y Privacidad (Security)

> **Versión:** 1.0  
> **Estado:** En elaboración

---

# Historial de versiones

| Versión | Fecha | Autor | Cambios |
|---------|--------|--------|----------|
| 1.0 | 13/08/2026 | Lautaro Amado | Creación del documento |

---

Este documento describe los mecanismos de seguridad, las políticas de privacidad y las estrategias de control de acceso implementadas en **Fulbito** para proteger la información de los usuarios y garantizar la integridad de los datos.

Dado que la arquitectura del sistema se basa en un modelo **Serverless (Backend as a Service)**, la aplicación móvil interactúa directamente con los servicios de Firebase. Por este motivo, la seguridad se apoya principalmente en **Firebase Authentication**, **Firestore Security Rules** y **Cloud Functions**, que actúan conjuntamente para autenticar usuarios, controlar permisos y validar operaciones críticas.

---

# 1. Autenticación

Todo acceso a la plataforma requiere una identidad válida ante Firebase Authentication, aunque esto no implica que el usuario deba completar un formulario de registro antes de usar la aplicación.

## Principios

- No existe acceso anónimo *a nivel de Firestore*: toda operación de lectura o escritura requiere un `request.auth.uid` válido.
- Sin embargo, la aplicación no exige que el usuario complete un registro tradicional (email/contraseña u OAuth) para comenzar a usarla.
- Al abrir la aplicación por primera vez, el cliente crea automáticamente una **sesión anónima** mediante Firebase Authentication (`signInAnonymously()`), sin mostrar ninguna pantalla de login. Esta sesión ya posee un `uid` único y válido, lo que permite crear grupos, registrar partidos y utilizar el resto de las funcionalidades del MVP desde el primer momento.
- La creación de una cuenta "completa" (vinculada a un email, Google o Apple) es **opcional** y no un prerrequisito de uso. Su propósito es permitir sincronizar los datos entre dispositivos y recuperarlos si el usuario cambia o reinstala la aplicación.

## Vinculación de cuenta (Account Linking)

Cuando un usuario con una sesión anónima decide crear una cuenta completa, el sistema utiliza el mecanismo de **vinculación de credenciales** de Firebase (`linkWithCredential`).

Esto permite que:

- El `uid` original se mantenga sin cambios.
- Todos los grupos, partidos y estadísticas asociados a ese `uid` permanezcan intactos.
- El usuario pase de tener una sesión anónima a tener una cuenta persistente, sin perder ningún dato.

## Métodos de autenticación

El sistema podrá admitir:

- Sesión anónima (por defecto, al abrir la aplicación).
- Correo electrónico y contraseña.
- Google.
- Apple.

Cada solicitud enviada a Firestore incluirá automáticamente un **JSON Web Token (JWT)** emitido por Firebase, permitiendo identificar al usuario mediante `request.auth.uid`, independientemente de si su sesión es anónima o vinculada a una cuenta completa.

---

# 2. Autorización

La autenticación únicamente identifica al usuario.

La autorización determina qué acciones puede realizar.

Fulbito aplica el **principio de menor privilegio** y un **modelo horizontal de permisos**: no existe un rol jerárquico permanente (como "administrador"). Todos los integrantes de un grupo tienen el mismo nivel de acceso, salvo por un conjunto reducido de acciones críticas que requieren aprobación colectiva mediante votación.

En términos generales:

- Solo los miembros de un grupo pueden acceder a su información.
- Solo los integrantes pueden registrar partidos y eventos.
- Cualquier integrante puede modificar la configuración general del grupo (nombre, imagen, modalidad predeterminada).
- Ningún usuario puede modificar directamente las estadísticas históricas.
- Las acciones críticas (expulsar a un integrante, eliminar el grupo, aprobar correcciones sobre partidos finalizados) requieren aprobación colectiva mediante votación, conforme a lo definido en `03-requirements/03-08-voting-rules.md`.
- Una sesión anónima posee exactamente los mismos permisos funcionales que una sesión vinculada a una cuenta completa; la única diferencia entre ambas es la persistencia y la posibilidad de sincronización entre dispositivos.

El detalle completo de qué puede hacer cada integrante se encuentra documentado en `03-requirements/03-07-roles-permissions.md`.

---

# 3. Privacidad y Aislamiento de Datos

Uno de los principios fundamentales de Fulbito es preservar la privacidad de cada grupo.

Para ello se implementan las siguientes políticas:

- Cada grupo constituye un espacio privado e independiente.
- Los usuarios únicamente pueden visualizar los grupos a los que pertenecen.
- No existe un directorio público de jugadores.
- Las estadísticas deportivas solo son visibles para los integrantes del grupo correspondiente.
- La información de un grupo nunca puede ser consultada por usuarios externos.

---

# 4. Firestore Security Rules

Las **Firestore Security Rules** representan la primera línea de defensa del sistema.

Estas reglas se ejecutan antes de cualquier operación de lectura o escritura y determinan si una solicitud debe ser aceptada o rechazada.

Entre las principales políticas se encuentran:

- Permitir la lectura únicamente a miembros del grupo.
- Permitir crear partidos únicamente a integrantes.
- Permitir registrar eventos únicamente cuando el partido se encuentre **En Curso**.
- Impedir modificar partidos **Finalizados**.
- Permitir modificar la configuración general del grupo (nombre, imagen, modalidad) a cualquier integrante.
- Restringir la ejecución de acciones críticas (expulsión de un integrante, eliminación del grupo) exclusivamente a Cloud Functions, que verifican que la votación correspondiente alcanzó el criterio de aprobación antes de aplicarlas.

Estas reglas garantizan que ningún cliente pueda omitir las restricciones del sistema, incluso si modifica la aplicación o intenta acceder directamente a Firestore.

---

# 5. Acceso mediante Invitaciones

El ingreso de nuevos integrantes se realiza exclusivamente mediante invitaciones.

Las invitaciones cumplen las siguientes características:

- Generación de códigos aleatorios de alta entropía.
- Posibilidad de revocar una invitación en cualquier momento.
- Validación del código antes de permitir el ingreso.
- Posibilidad de implementar invitaciones de un solo uso o con vencimiento.

La incorporación de un nuevo miembro se procesa mediante **Cloud Functions**, evitando que el cliente pueda alterar directamente la estructura de integrantes del grupo.

---

# 6. Integridad de la Información

Uno de los objetivos principales del sistema es garantizar que el historial deportivo no pueda ser manipulado.

Para ello:

- Los partidos finalizados son inmutables desde el cliente.
- Las estadísticas individuales no pueden editarse manualmente.
- Toda modificación histórica debe originarse mediante una Solicitud de Corrección.
- Las estadísticas siempre son recalculadas automáticamente por el backend.

De esta forma, el historial permanece consistente incluso cuando existen modificaciones posteriores.

---

# 7. Correcciones y Votaciones

Las solicitudes de corrección constituyen el único mecanismo válido para modificar información histórica.

El proceso funciona de la siguiente manera:

1. Un integrante crea una solicitud de corrección.
2. Los demás integrantes emiten su voto.
3. Cuando se alcanza el criterio de aprobación establecido, una **Cloud Function**:
   - actualiza el partido correspondiente;
   - recalcula las estadísticas afectadas;
   - actualiza el historial del grupo;
   - marca la solicitud como resuelta.

El cliente nunca modifica directamente los partidos finalizados.

---

# 8. Protección de Datos

Fulbito procura minimizar la cantidad de información personal almacenada.

Se conservarán únicamente los datos necesarios para el funcionamiento de la aplicación, entre ellos:

- nombre;
- correo electrónico;
- fotografía de perfil (opcional);
- información deportiva;
- pertenencia a grupos.

No se almacenarán datos sensibles que no aporten valor al funcionamiento del sistema.

---

# 9. Seguridad de la Infraestructura

La seguridad de la infraestructura es delegada principalmente a Firebase.

Entre las garantías proporcionadas por la plataforma se encuentran:

- autenticación segura;
- transmisión de datos mediante HTTPS/TLS;
- almacenamiento administrado por Google Cloud;
- disponibilidad y escalabilidad automáticas;
- replicación de datos;
- administración centralizada de identidades.

Esto permite concentrar el esfuerzo de desarrollo en la lógica del negocio sin implementar mecanismos propios de infraestructura.

---

# 10. Principios de Seguridad

El diseño de seguridad de Fulbito se basa en los siguientes principios:

- Autenticación obligatoria.
- Principio de menor privilegio.
- Privacidad por defecto.
- Integridad del historial deportivo.
- Validación de operaciones críticas en el backend.
- Defensa en profundidad mediante Firebase Authentication, Firestore Security Rules y Cloud Functions.
- Separación entre autenticación, autorización y lógica de negocio.

---

> **Resumen de Seguridad**
>
> La seguridad de Fulbito se basa en un enfoque de múltiples capas. Firebase Authentication garantiza la identidad de los usuarios, Firestore Security Rules controla el acceso a los datos y Cloud Functions ejecuta de forma segura las operaciones críticas del sistema. Esta combinación permite proteger la privacidad de los grupos, preservar la integridad del historial deportivo y mantener una arquitectura escalable sin sacrificar la seguridad.