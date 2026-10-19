# Propuesta TP DSW

## Grupo

### Integrantes

- 53908 - Cappa, Giuliano Martín

### Repositorios

- [frontend app](https://github.com/Martiks38/frontend_dsw)
- [backend app](https://github.com/Martiks38/backend_dsw)

_Nota_: si utiliza un monorepo indicar un solo link con fullstack app.

## Tema

### Descripción

**MarinaGuard** es un sistema de gestión para una **guardería náutica**. Los socios consultan sus embarcaciones y solicitan servicios; el administrador da de alta clientes, empleados y embarcaciones, y agenda y asigna cada solicitud a un operario; el operario ejecuta el servicio y registra la salida y el ingreso de la embarcación.

No existe una embarcación sin contrato: cada embarcación se crea junto con un contrato que la vincula a una cuna de la guardería.

### Roles

Se derivan de `isEmployee` y `employeeType`.

| Rol        | Qué hace                                                                                              |
| :--------- | :---------------------------------------------------------------------------------------------------- |
| `MEMBER`   | Ve sus embarcaciones, solicita servicios y cancela las suyas mientras estén pendientes.               |
| `OPERATOR` | Ejecuta las solicitudes que se le asignan, edita el comentario interno y registra salidas e ingresos. |
| `ADMIN`    | Da de alta clientes, empleados y embarcaciones, y agenda, asigna y cancela solicitudes.               |

### Tecnologías

- **Frontend:** Next.js 16, desplegado en Vercel.
- **Backend:** NestJS, desplegado en Render (plan gratuito), en un dominio distinto al del frontend.
- **Base de datos:** PostgreSQL en Neon, con Prisma.
- **Autenticación:** JWT en cookie httpOnly. Al cliente solo viaja el `publicId`; el `userId` interno nunca sale del backend.
- **Mail:** Brevo por HTTPS con `fetch` (contraseñas temporales y recuperación de contraseña).

### Modelo

![imagen del modelo](/assets/DER.png)

| Entidad                 | Descripción                                                                                                                                                           |
| :---------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `users`                 | Cuenta de acceso: `publicId`, email, contraseña, teléfono, documento, `isActive`, `isEmployee`.                                                                       |
| `members`               | Socio: nombre y apellido o razón social (`businessName`). Se relaciona 1:1 con `users`.                                                                               |
| `employees`             | Empleado: nombre, apellido, legajo (`employeeNumber`) y `EmployeeType` (`ADMIN` u `OPERATOR`). Se relaciona 1:1 con `users`.                                          |
| `boat_types`            | Tipo de embarcación, con la operación requerida para moverla (`OperationType`: `MANUAL` o `AUTOMATIC`).                                                               |
| `boats`                 | Embarcación: nombre, modelo, matrícula, descripción. Pertenece a un socio y tiene un tipo.                                                                            |
| `cradles`               | Cuna de la guardería: código y estado.                                                                                                                                |
| `contracts`             | Vincula una embarcación con una cuna. El contrato activo es el que tiene `endDatetime` en `null`; `plannedEndDate` es la salida prevista.                             |
| `service_types`         | Catálogo de servicios que se pueden solicitar.                                                                                                                        |
| `service_requests`      | Solicitud de un servicio para una embarcación, con estado (`ServiceStatus`), fecha y hora agendadas, observaciones del socio, comentario interno y operario asignado. |
| `boat_departures`       | Salida de una embarcación ligada a una solicitud: hora de salida, regreso estimado y regreso real.                                                                    |
| `password_reset_tokens` | Tokens de recuperación de contraseña (hash SHA-256, vencen en 1 hora).                                                                                                |

## Reglas de negocio

- **Estados de una solicitud:** `PENDING → SCHEDULED` (lo asigna el admin, a mano) `→ IN_PROGRESS → COMPLETED` (estas dos las marca el operario asignado). También existe `CANCELED`.
- **Cancelar:** el socio solo cancela las suyas mientras estén `PENDING`. El admin cancela en `PENDING` o `SCHEDULED`.
- **Comentario interno:** lo editan operario y admin; el socio nunca lo ve.
- **Contratos:** no existe una embarcación sin contrato. La embarcación y su contrato se crean juntos.
- **Estado de una embarcación (vista admin)**, por prioridad: `EN_MANTENIMIENTO` (otro servicio en curso), luego `EN_AGUA` (salida abierta) y, por defecto, `EN_GUARDIA`.
- **Altas por el admin:** clientes, empleados y embarcaciones. La contraseña es temporal, se genera sola y se envía por mail. En empleados el legajo se autogenera y el documento es siempre DNI. En clientes el tipo de documento sale del tipo de cliente.

## Operaciones (salida e ingreso de embarcaciones)

Módulo que **crea y actualiza las salidas y los ingresos** de las embarcaciones (`BoatDeparture`) según el avance de las solicitudes de botadura y retiro:

1. **Salida:** al pasar la solicitud a `IN_PROGRESS`, se pide la hora estimada de regreso y se crea la `BoatDeparture`.
2. **Ingreso:** al pasar la solicitud a `COMPLETED`, se cierra el regreso real de esa salida.
3. Mientras la salida tenga el regreso abierto, la embarcación figura `EN_AGUA`.

## Alcance Funcional

| Área        | Detalle                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Backend     | Dashboards por rol. Listado de solicitudes (paginado, con filtros). Detalle, asignación, cancelación y actualización de estado de solicitudes, y creación de varias solicitudes a la vez. Embarcaciones del socio, vista admin y vista de la guardería. Clientes y empleados: alta, listado y activar o desactivar. Perfil con cambio de contraseña, recuperación de contraseña e invitaciones por mail. Seed con 12 cunas, contratos activos y fechas de salida prevista. Tests de auth y empleados, y Swagger con DTOs de respuesta. |
| Frontend    | Dashboards, perfil (socio y operario) y tarjetas de embarcaciones. Tablas de solicitudes para socio y operario, y para admin (con pestañas y búsqueda). Detalle de solicitud con diálogo de confirmación. Catálogo de servicios y asistente de solicitud con selección múltiple. Páginas admin de Embarcaciones, Guardería, Clientes, Empleados y Servicios. Formularios de alta, recuperación de contraseña y página 404.                                                                                                             |
| Operaciones | Creación y actualización de salidas e ingresos de embarcaciones (ver sección anterior).                                                                                                                                                                                                                                                                                                                                                                                                                                                |
