
    2025-04-21

    Base de datos 2 

__________________________________

# Base de datos Clave-valor

## Caracteristicas generales

    - Base de datos no relacional

    - Utiliza un par Clave-valor para almacenar datos 

    - Tanto las claves como los valores pueden ser cualquier cosa

    - Son altamente divisibles y permiten escalado horizontal muy bueno

    - Similar a una tabla hash

    - Indicada cuando todos los accesos a la base de datos se hacen por una clave 

    - El valor no es analizado por el producto, lo trata como una unidad

## La clave 

    - Identificador unico

    - Productos como redis tienen un tamaño de clave maximo de 512MB 

    - Cualquier secuencia binaria como clave 

    - Por cuestiones de rendimiento se evitan claves muy largas

## Diseñado de claves 

    - La seleccion se complica si queremos guarar info de varias entidades con el mismo nombre de clave (por ejemplo: numero de cliente, numero de proveedor, etc.)

    - Una estrategia es añadir a las claves un prefijo con el nombre de la entidad 

    - Por ejemplo, podriamos utilizar cli.Numero y prov.Numero para no mezclar claves

## El valor 

    - El valor puede ser cualquier cosa en formato binario

    - Algunos KVDBMS permiten especificar tipo de datos para la clave o valor 

## Diseño 

    - Como estructurar las claves: Un buen diseño facilita la definicion de los conceptos

    - Que informacion capturar: Solo lo justo y necesario

    - Introducir abstracciones: Patrones de diseño para crear estructuras de mas alto nivel

## Ventajas

    - Altamente efectivas

    - Escalan facilmente

    - Capacidad de manejar gran volumen de datos

    - Info dispuesta en forma clara

## Desventajas

    - Solo se accede a traves de una clave 

    - No se pueden realizar busquedas complejas

## Cuando usarlas
    
    - Almacenar informacion de sesion

        - El sessionID de la sesion web de un usuario

        - Todo en una sesion puede almacenarse con un solo PUT o ser recuperado con un solo GET 

        - Toda la informacion de la sesion se almacena en un unico objeto

    - Perfiles de usuario/preferencias

        - Cada usuario tiene un unico userID

        - Todos los atributos pueden recuperarse en un unico acceso 

        - Todos los atributos se almeacenan en un unico objeto
    
    - Carritos de compra

        - En sitios de E-commerce se asocian carritos a los usuarios

        - La informacion de carrito se puede almacenar como unico objeto

## Cuando no usarlas 

    - Consultas que relacionen datos 

    - Transacciones multi-operacion

    - Consultas por valores almacenados dentro del campo valor

    - Operaciones que abarcan diferentes conjuntos de datos 

# redis DB 

    - Almacena en memoria

    - Se utiliza como DB, caché, broker de mensajes y motor de streaming

    - Posee estructuras de datos

    - Puede ejecutar operaciones atomicas sobre esos tipos

    - Puede calcular operaciones sobre conjuntos o conseguir el miembro con la clasificacion mas alta en un conjunto ordenado

## Caracteristicas

    - Replicacion integrada

    - Secuencias de comandos LUA

    - Liberacion de claves temporales

    - Transacciones

    - Niveles de persistencia

    - Alta disponibilidad (Redis Sentinel)

    - Particion automatica (Redis Cluster)

    - Trabajo completo en memoria

    - Conversion dedatos a permantente mediante volcado periodico en disco

    - Replicacion asincrona 

## Replicacion 

Proceso que permite crear copias exactas de un servidor llamado "maestro" (master) en uno o varios servidores llamados "esclavos" (slave), proporcionando redundancia y escalabilidad al sistema.

    - Empezamos estableciendo el servidor maestro

    - Configuramos servidores esclavos para conectarse al maestro

    - El maestro envia datos que cambian a los servidore esclavos a traves de un flujo de datos TCP utilizando el protocolo de replicacion de Redis 

    - Cuando un esclavo se conecta a un maestro por primera vez (inicializacion) el maestro envia una copia completa de los datos (snapshot) y los comandos que se ejecutaron en el maestro desde el inicio del proceso hasta el momento en que se finalizo la copia 

## Persistencia

Redis proporciona distintas formas de persistencia:
    
    - La persistencia de tipo RDB que realiza snapshots de su conjunto de datos en intervalos especificados 

    - La persistencia de AOF registra cada operacion de escritura recibida en el master, permitiendo reconstruir el conjunto de daots

    - Puede sobreescribir en segundo plano

    - Se puede desactivar completamente la persistencia 

    - Es posible usar AOF y RDB en la misma instancia

    - Si existe archivo AOF en start-up, recupera la base de datos por ahi 

## RDB

### Ventajas 

    - Representacion compacta

    - Perfecta para copias de seguridad

    - Bueno para recuperacion de desastres

    - Maximiza el rendimiento
    
    - Permite reinicios mas rapidos

### Desventajas

    - No es bueno para minimizar perdida de dedatos
    
    - No almacena los ultimos minutos de datos

    - Consume gran cantidad de tiempo

## AOF

    - 3 politicas de sincronizacion

        - always: Escribe cada comando en el archivo AOF en ejecucion

        - everysec: Escribe los comandos en archivo AOF de forma asincronica 

        - no: Escribe de manera asincronica sin ningun tipo de sincronizacion con el disco 

### Ventajas

    - Registro de append, recuperacion facil 

    - Optimizacion de operaciones 

    - Registro secuencial de operaciones

### Desventajas

    - Archivos mas grandes que RDB 

    - Mas lento que RDB

    - Actualizacion incrementales en vez de re-escritura

## Seguridad 

Los principales problemas de seguridad son:

    - Control de acceso

    - Seguridad de codigo 

    - Entradas maliciosas

Un cliente puede autenticarse enviando el comando AUTH seguido de la contraseña 

La capa de autenticacion proporciona opcionalmente una capa de redundancia al mecanismo implementado

No soporta encriptado por lo que se debe implementar una capa adicional de proteccion como un proxy SSL

Como no posee el concepto de secuencias de escape en cadenas, la inyeccion de codigo es imposible al igual que en los scripts LUA ejecutados por los comando EVAL y EVALSHA 
