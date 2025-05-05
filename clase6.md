
    2025-04-14

    Base de datos 2

______________________________

# Base de datos de grafos 

    - Basada en estructura de grafos (nodos y aristas) para representar y almacenar la informacion

    - Permiten una representacion fiel de las entidades y las aristas entre estas

    - Entidades: Nodos con sus propiedades

    - Aristas: Relaciones entre nodos con sus propiedades. Dirigidas o No Dirigidas

    - Permite ser interpretado de distintas maneras en base a sus relaciones 

## Casos o aplicaciones

### Redes sociales o laborales

Relaciones entre entidades dde diferentes dominios (Identidad y gestion de accesos) en una sola base de datos hace que la informacion sea mas valiosa

### Motores de recomendaciones

Como los nodos y las relaciones se crean en elsistema, pueden ser utilizados para hacer recomendaciones como "Sus amigos tambien han comprado este producto" o "Al facturar este articulo, estos articulos suelen ser facturados" en tiempo real

### Busquedas recursivas

    - Sistemas con busqueda recursivas con N niveles

    - Busqueda de patrones para detectar el fraude en las transacciones en tiempo real 

    - Gestion de datos que naturalmente se modelan como grafos

## Cuando no se usan

Las GDBMS no se deben usar en sistemas con actualizaciones masivas sobre todas las entidades o relaciones, Sistemas que requieren una alta distribucion de datos 

# NEO4J

    - NEO4J es un sistema de base de datos orientado a grafos de codigo abierto

    - Desarollado en java

    - Utiliza como modelo de datos, los grafos basados en propiedades

    - Cuenta con un lenguaje de consulta propio: Cypher

    - Administra un servidor independiente o un cluster

    - Base de datos: Particion administrativa de un DBMS (conjunto de archivos en un directorio o carpeta, que tiene el nombre de la base de datos)

    - En un cluster hay 2 roles:

        - Nucleos

        - Replicas de lectura

## Caracteristicas del clustering

    - Seguridad: Los servidores centrales proporcionan una plataforma tolerante a fallos para el procesamiento de transacciones 

    - Escala: Las replicas de lectura son una plataforma escalablle para consultas de graficos en una topologia ampliamente distribuida

    - Consistencia causal: Cuando se invoca, se garantiza que una aplicacion cliente leera al menos sus propiedades

## Servidores centrales

    - Protege los datos mediante replicaciones de todas las transacciones 

    - Una vez que la mayoria de los servidores centrales de un cluster (N / 2 + 1) han aceptado la transaccion, es seguro reconocer el compromiso

    - Alto impacto en la latencia de escritura (a mediada que aumenta el numero de servidores centrales en el cluster, el tamaño de la mayoria tambien)

    - Debemos tener relativamente pocas maquinas en un cluster Core server 

    - La formula es M = 2F + 1 

        - M es el numero de servidores centrales necesarios para tolerar fallas

        - F el numero de cores fallidos

    - Un cluster de solo dos nucleos no sera tolerante a fallas, si uno falla el restante sera de solo lectura

    - Las replicas de lectura son las responsables de escalar las cargas de trabajo de los graficos

    - Actuan como caches para los datos de graficos que los servidores centrales protegen

    - Son capaces de ejecutar consultas y procedimientos (Solo lectura)

    - Trabajan de forma asincronica desde los servidores centrales mediante el envio del registro de transacciones

    - Muchas replicas de lectura pueden recibir datos de una cantidad relativamente pequeña de servidores centrales, lo que permite escalar una gran carga de trabajo

    - Las replicas de lectura deben ser muchas (Relativamente) y tratarse como desechables

    - Su  perdida no afecta las capacidades de tolerancia a fallas del cluster, solo su porcion de procesos

    - Es importante pensar en como las aplicaciones usaran la base de datos para realizar su trabajo (Operaciones de lectura y escritura sobre un grafo) en base a las operaciones anteriores

    - La coherencia causal es un modelo que se utiliza en la informatica distribuida

    - Garantiza que todas las instancias del sistema vean las operaciones relacionadas en el mismo orden

    - Se garantiza que las aplicaciones cliente leeran sus propias escrituras, undependientemente de las instancias con la que se comuniquen (los clientes asumen que son tratados por un unico servidor)

    - Escribimos en servidores centrales y leemos esas escrituras desde una replica 
## NEO4J Tareas administrativas

    - Backups

    - Restore

    - Actualizaciones 

    - Autorizacion y autenticacion 

    - Seguridad
