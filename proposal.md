# Propuesta TP DSW

## Grupo
### Integrantes
* 53908 - Cappa, Giuliano Martín
*  - Rojas, Alexis Julian

### Repositorios
* [frontend app](https://github.com/Martiks38/frontend_dsw)
* [backend app](https://github.com/Martiks38/backend_dsw)
*Nota*: si utiliza un monorepo indicar un solo link con fullstack app.

## Tema
### Descripción

El sistema gestiona una plataforma de colección de cartas y juegos basado en Pokémon. Permite a los usuarios adquirir cartas mediante un marketplace, gestionar su colección y realizar intercambios con otros jugadores. Además, intrega un sistema para registrar y visualizar las estadísticas de rendimiento en cada uno de los diferentes modos de juego.

### Modelo
![imagen del modelo](/assets/Modelo%20de%20dominio.jpg)

## Alcance Funcional 

### Alcance Mínimo

Regularidad:
|Req|Detalle|
|:-|:-|
|CRUD simple|1. CRUD User<br>2. CRUD Game_Mode<br>3. CRUD Pokemon|
|CRUD dependiente|1. CRUD Card {depende de} CRUD Pokemon<br>2. Guess_Attempts {depende de} CRUD User, CRUD Game_Mode y CRUD Pokemon<br>3. CRUD Player_Stats {depende de} CRUD User, CRUD Game_Mode<br>4. CRUD Daily_Challenge {depende de} CRUD Pokemon<br>5. CRUD Pack {depende de} CRUD Card y CRUD Card_Pack<br>6. CRUD Card_Pack_Inventory {depende de} CRUD Inventory y CRUD Card_Pack|
|Listado<br>+<br>detalle| 1. Listado de los modos de juego: muestra los diferentes modos de juego<br>2. Listado de Marketplace: muestra el precio y contendio de los sobres y packs de cartas. Filtrado por set y precio => detalle CRUD Pack y Card_Pack<br>3. Listado de Inventario de sobres para abrir => detalle muestra cantidad de sobres e imagen del set<br> 4. Listado de cartas obtenidas: muestra una imagen de las cartas del set del sobre abierto => CRUD Card_Pack y CRUD Card|
|CUU/Epic|1. Resolver desafío diario<br>2. Comprar producto del marketplace<br>3. Abrir sobres|


Adicionales para Aprobación
|Req|Detalle|
|:-|:-|
|CRUD |1. CRUD Exchange_Request<br>2. CRUD Offer<br>3. CRUD Wanted_card/Offered_Card<br>|
|CUU/Epic|1. Proponer un intercambio: El usuario selecciona qué cartas quiere y qué cartas ofrece, validando que las tenga en su inventario<br>2. Finlizar Intercambio: Al aceptar una oferta, el sistema debe realizar el traspaso de propiedad de las cartas entre los inventarios<br>3. Gestión de Wallet/Saldo: validación de fondos para compras en el marketplace<br>4. Configuración del Desafío diario: el admin selecciona manualmente o automática qué Pokémon/Carta es el objetivo del día y qué recompensas otorga|


### Alcance Adicional Voluntario

|Req|Detalle|
|:-|:-|
|Listados |1. Ranking (Leaderboard): Listado de jugadores con más desafíos diarios ganados<br>2. Historial de Intercambios: Filtrado por fecha y estado mostrando las cartas que entraron y salieron|
|CUU/Epic|1. Sistema de Sobres/Packs de Regalo: poder comprar un producto y enviarlo a otro usuario como regalo<br>2. Sincronización con APIs: servicio automático que cargue los datos necesarios consumiendo el endpoint externo, asegurando datos oficiales|
|Otros|1. Notificaciones en tiempo real: Aviso al usuario cuando su oferta de intercambio fue aceptada/rechazada<br>2. Estadísticas globales de los jugadores: El admin puede visualizar que juegos son los más jugados y como es el rendimiento de sus jugadores<br>3. Logs de compras: el admin puede ver un historial de todas las transacciones de dinero y cartas|

