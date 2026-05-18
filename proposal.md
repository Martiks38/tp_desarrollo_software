# Propuesta TP DSW

## Grupo
### Integrantes
* 53908 - Cappa, Giuliano Martín

### Repositorios
* [frontend app](https://github.com/Martiks38/frontend_dsw)
* [backend app](https://github.com/Martiks38/backend_dsw)

*Nota*: si utiliza un monorepo indicar un solo link con fullstack app.

## Tema
### Descripción

El sistema gestiona una **guardería náutica** llamada **Puerto Oasis**. Permite a los socios registrarse, dar de alta sus embarcaciones y contratar camas náuticas en distintos sectores para su resguardo. El sistema registra las salidas y regresos de embarcaciones controlando los horarios habilitados. Además integra una **escuela náutica** donde instructores dictan actividades náuticas a través de cursos, a los que los socios pueden inscribirse.

### Modelo
![imagen del modelo](/assets/Modelo%20de%20dominio.png)

## Alcance Funcional

### Alcance Mínimo

Regularidad:
|Req|Detalle|
|:-|:-|
|CRUD simple|1. CRUD Member (Socio)<br>2. CRUD Boat_Type (Tipo de embarcación)<br>3. CRUD Activity (Actividad náutica)|
|CRUD dependiente|1. CRUD Boat {depende de} CRUD Member y CRUD Boat_Type<br>2. CRUD Boat_Slip {depende de} CRUD Sector y CRUD Boat_Type<br>3. CRUD Contract_Quota {depende de} CRUD Boat y CRUD Boat_Slip<br>4. CRUD Departure {depende de} CRUD Boat y CRUD Contract_Quota<br>5. CRUD Course {depende de} CRUD Activity y CRUD Employee<br>6. CRUD Enrollment {depende de} CRUD Member y CRUD Course|
|Listado<br>+<br>detalle|1. Listado de sectores: muestra los sectores disponibles con sus tipos de embarcación admitidos y tipo de operación (manual/automático)<br>2. Listado de camas náuticas: muestra el estado de cada cama (disponible, ocupada, mantenimiento) filtrado por sector => detalle muestra la embarcación actual y el historial de contratos<br>3. Listado de salidas activas: muestra las embarcaciones retiradas con fecha/hora de regreso estimada => detalle muestra datos del socio y la embarcación<br>4. Listado de cursos: muestra los cursos disponibles con actividad, instructor, fechas y cupo => detalle muestra horarios por día e inscriptos|
|CUU/Epic|1. Registrar salida de embarcación<br>2. Registrar regreso de embarcación<br>3. Inscribir socio a un curso|

Adicionales para Aprobación:
|Req|Detalle|
|:-|:-|
|CRUD|1. CRUD Employee (Instructor)<br>2. CRUD Sector<br>3. CRUD Course_Schedule (Horarios del curso)|
|CUU/Epic|1. Contratar cama náutica: el socio selecciona sector, cama disponible y embarcación, validando que la cama esté libre y el tipo de embarcación sea compatible con el sector<br>2. Dar de baja contrato de cama: se registra fecha de baja; la embarcación debe ser retirada o reasignada a otra cama<br>3. Crear curso con validación de instructor: al crear un curso el sistema valida que el instructor asignado esté habilitado para dictar la actividad seleccionada<br>4. Control de regresos al cierre: el sistema alerta sobre embarcaciones con regreso estimado vencido que aún no hayan registrado su vuelta|

### Alcance Adicional Voluntario

|Req|Detalle|
|:-|:-|
|Listados|1. Historial de contratos por cama: filtrado por fecha y estado, mostrando todas las embarcaciones que ocuparon cada cama|
|CUU/Epic|1. Gestión de cupo en cursos: el sistema impide inscripciones cuando el curso alcanzó su cupo máximo y ofrece lista de espera|
|Otros|1. Notificaciones en tiempo real: aviso al socio cuando su embarcación tiene regreso vencido o cuando se aprueba su inscripción a un curso<br>2. Estadísticas para el administrador: visualización de ocupación de sectores, embarcaciones más activas y cursos con mayor demanda<br>3. Logs de operaciones: el administrador puede ver un historial de todas las entradas, salidas y cambios de contrato registrados en el sistema|
