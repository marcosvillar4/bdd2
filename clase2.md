    
    2025-03-17

    Base de datos 2

---------------------------------

Almacenamiento

Historia:

    - 1951 / Creacion de la cinta magnetica 
    
    - 1956 / Se crea el IBM 350 Disk File - Primer disco duro

    - 1971 / Primer disco flexible, capacidad de 1972.

    - 1980 / Se masifican los disco duro (HDD)

    - 1991 / Creacion del Disco de estado solido (SSD)

Arquitecturas de Almacenamiento:

    - DAS: Direct attached storage 
    - NAS: Network attached storage
    - SAN: Storage area Networ

Concepto de la base de datos:

    - El Concepto de la base de datos aparece en los fines de los 60's

    - Retoma durante los fines de los 90

    - Objetivo: Bases de datos que administran grandes volumenes de informacion
    
    - Alta interaccion, multiples usuarios concurrentes y operacion continua

Bases de datos distribuidas:
    
    - Mezcla de tecnologias
    
    - tecnologia de base de datos

    - tecnologia de redes y comunicacion

    - Ya no se arman bases centralizadas gigantes

    - Descentralizacion de procesos

    - Integracion de la informacion dentro'de bases de datos ubicadas en posiciones geograficas diferentes

    - Cada nodo proporciona un entorno de ejecucion tanto local como global

    - Posee un esquema logico global unico

    - Datos integrados permiten la recuperacion y la actualizacion

    - Unica operacion para accedder a datos replicados

    - Transacciones locales

    - Transacciones globales

Cluster

    - Grupo de servidores independientes interconectados a traves de una red dedicada que trabajan como un unico recurso de procesamiento


    - Ventajas
        
        - Distribuyen la carga entre los servidores interconectados

        - Mejoran la disponibilidad de los usuarios

        - Mejoran la performance y tolerancia de fallas

        - Un servidor caido no afecta el sistema

Escalamiento

    - Escalamiento vertical

        - Mejoras de hardware interno

        - Es el mas simple de implementar

        - Elimina la complejidad de distibuir datos en diferentes servidores

        - Algunas bases de datos solo escalan de manera vertical (Grafos)

    - Escalamiento horizontal

        - Se agregan mas equipos al cluster
        
        - Replicacion

            - Replicacion Maestro-Esclave (Master/Slave)

            - Replicacion Esclavo-Maestro-Esclavo (Slave/Master/Slave)

            - Replicacion entre pares

        - Particionamiento

        - Hibrida (Replicacion y Particionamiento)

Arquitectura Share Nothing

    - Arquitectura distribuida, cada nodo es independiente y autosuficiente

    - Alta escalabilidad

    - Replicacion de datos

    - No SPOF (Single point of failure)

    - Crecimiento casi sin limite

    - No hay cuello de botella en el sistema

    - No existe un unico punto de contencion

    - Tecnologias:
    
        - Grids

        - HADOOP

Arquitectura shared disks

    - Cada nodo tiene acceso exclusivo a su propia memoria

    - Todos los nodos pueden acceder al Almacenamiento

    - Cada nodo tiene su propia copia del sistema operativos

    - Viable para aplicaciones y servicios con un modesto acceso a datos

    - Aplicaciones que son dificiles de particionar

Computacion distribuida

    - Dividde problemas grandes e inmanejables en piezas mas pequeñas que se resuelven en forma coordinada

    -   Se aprovecha mejor la potencia del procesador en la resolucion de tareas complejas

    - Cada elemento de procesamiento se puede gestionar en forma independiente en el desarollo de tareas locales

Consistencia de datos

    - Desafio: gestionar la consistencia de datos
    
    - Mantener la informacion uniforme a lo largo de una red o aplicaciones

    - Validar de acuerdo a reglas o modelos predefinidos

    - Una de las propiedades ACID

    - Bases de datos relacionales tienen alta consistencia
    
    - En NoSQL aparecen terminos como "Teorema CAP" y "Consistencia eventual"

    - La consistencia no es evaluada en terminos absolutos, si no en base a necesidades

    - CAP: Consistency-Availability-Partition tolerance

CAP

    - Consistency:
        
        - El acceso a datos ees considerado plenamente cosistente cuando una actualizacion escrita en un nodo esta inmediatamente disponible en todos los otros nodos del cluster

        - A traves de esquema de transacciones distribuidas

        - Demoras y llazos mortales

    - Availability:

        - Garantiza respuestas para todos los requerimientos que recibe

        - Una base de datos con un solo nodo no es posible (SPOF)
        
        - Distribucion de datos en varios nodos

    - Partition tolerance:

        - Los nodos pueden estar fisicamente separados pero conectados

        - Particion de red: Laps en el que no se pueden alcanzar entre

        - Durante una desconexion, los nodos deberian poder leer y escribir

        - El sistema automaticamente reconcilia las actualizaciones tan pronto como cada nodo puede enlazar (escenario ideal)

    - Un sistema distribuido solo puede proveer a lo sumo dos de las tres propiedades

    - Este enfoque fue utilizado por sistemas de bases de datos basadas en quorums que presentan algunos de los motores NoSQL como MongoDB

    - Espacio N/A 

        - Este espacio es un  conjunto vacio, ya que contradice al teorema CAP

    - Espacio CA

        - Privilegia la consistencia y disponibilidad

        - No existe distribucion de datos por lo cual no hay que preocuparse por el particionamiento

    - Espacio CP 
        
        - Se privilegia la tolerancia a las particiones y la consistencia

        - Algunos ofrecen consistencia fuerte en desmedro de cierto nivel de indisponibilidas (MongoDB,HBase, BigTable)

        - Ante una particion de red,estas bases de datos pueden no llegar a responder a cierto tipo de consultas

    - Espacio AP 
    
        - Se privilegia la disponibilidad y la tolerancia a particiones

        - Ofrecen alguna forma de consistencia relativa (Cassandra, Dynamo, CouchDB, Riak)

        - Estos motores forecen replicacion de datos, pero no garantizan consistencia entre dos o mas nodos


Parametros NRW

    - N: Cantidad de nodos
    
    - R: Cantidad de replicas requeridas para considerar una operacion de lectura exitosa en un cluster

    - W: Representa un valor por defecto para el cluster completo y en algunas bases de datos puede definirse como para cada una operacion de escritura

    - Al escibir datos (Inserción o actualización) el parametro W me indicara cuantas replicas deben escribirse para que la operacion sea considerada exitosa (W <= N)

    - Un valor alto de W 

        - Mayor consistencia de los datos escritos
        
        - Mayor posibilidad de fallas

        - Mayor latencia de red

    - Un valor bajo de W:

        - Afecta la consistencia de datos

        - La operacion puede ser considerada exitosa al recibir confirmacion de una cantidad de nodos menor

        - Involucra menos nodos, por lo que impllica un tiempo menor de red
        
        - Las lecturas en otras replicas pueden devolver un valor desactualizado

    - Al leer datos, el parametro R indicara cuantas replicas deben leer para que la copia sea considerada exitosa

    - Los primeros R nodos en devolver el mismovalor requerido forman parte del quorum de lectura

    - Cuanto mas alto R, mas nodos deben devolver el mismo valor

    - Valor R alto  

        - Nivel de consistencia mas fuerte

        - Menor performance

        - Aun con un valor W bajo, fuerza a reconciliar piezas de datos no actualizadas que aparecen escritas con cierta inconsistencia

    - Valor R bajo

        - Menos problemas de disponibilidad

    - Consistencia de escrituras

        - La consistencia por escrituras se detoermina por los valores W=N y R=1

        - Consistencia fuerte

        - Utilizado por RDBMS

        - Garantiza lecturas mas rapidas pero realentiza las escrituras

    - Consistencia por lecturas

        - Determinada por W=1 y R=N

        - Implica asegurar la escritura a un solo nodo pero leer todos ellos

        - Pueden leerse varias versiones de un archivo

    - Consistencia eventual

        - Si no existen nuevas actualizaciones que se hagan eventualmente a un objeto, todos los accesos van a devolver el valor mas actualizado al ultimo termino

        - Un ejemplo son las actualizaciones a un DNS que se distribuyen en funcion a caches controlados por tiempo

        - Eventualmente el cliente puede ver o no la actualizacion, la consistencia no esta garantizada aunque existen mecanismos que la intentan maximizar

    - Consistencia  por quorums

        - Cuanto mayor la cantidad de nodos, mayor la chance de evitar inconsistencias

        - quorum de lectura: R > N/2
        
        - quorum de escritura: W > N/2

        - El quorum de lectura es un poco mas complicado poque depende de cuantos nodos se necesitan para confirmar una lectura


Bases de datos documentales

    - Base de datos orientada a documentos o almacenes de documentos

    - Los domcumentos tambien pueden ser creados utilizando bases de datos Clave/valor

    - Permite almacenar, recuperar y administrardatos estructurados o semiestructurados

    - El termino "Documento" puede referirse a un documento de multiples formatos,pero comunmente es un archivo XML o JSON

    - El esquema es variable y proporciona mucha mas flexibilidad para el modelado de coumentos grandes

    - Almacena cada registro y sus datos asociados en un solo documento conteniendo datos semiestructurados que pueden ser consultados mediante variadas herramientas de consultas

    - Modelado flexible de datos: Eliminan la necesiada de forzar modelos de datos relacionales para admitir nuevas aplicaciones

    - Los documentos se agrupan en "Colecciones" Que tienen un concepto similar a una tabla relacional.

    - Una base de datos de documentos proporciona un mecanismo de consulta para buscar colecciones de documentos con atributos particulares

    - Rendimiento de escritura rapido

    - consultas rapidas y eficientes

    - EJEMPLOS
        
        - MongoDB

        - DynamoDB

        - CouchBase

        - Azure CosmosDB

MongoDB

    - Estructura basica

        - Un registro es un documento compuesto de pares de campo y valor

        - Los documentos de MongoDB son similares a los objetos JSON

        - Los valores de los campos pueden incluir otros documentos, matrices y matrices dedocumentos

    - Caracteristicas: Seguridad

        - Autenticacion

        - Autorizacion

        - Encriptado

        - TLS/SSL

        - Kerberos, LDAP, Encriptado REST

        - Auditoria

    - Caracteristicas: Almacenamiento

        - Motor de almacenamiento --> administra los datos

        - Varios motores de almacenamiento

        - Log --> Registro que ayuda a recuperar en caso de errores

        - GridFS --> Sistema de almacenamiento versatil 
    
    - Caracteristicas: Indices

        - Estructuras de atos especiales que almacenan una pequeña porcion del conjunto de datos de la coleccion en una forma facil de recorrer

        - Almacena el valor de un campo especifico o conjunto de campos ordenados por el valor del campo

        -Admite coincidencias de igualdad eficientes y operaciones de consulta basadas en rangos 

        - Puede devolver resultados ordenados segun el orden del Indices

        - Similar a los indiced en otros sistemas de bases de datos

        - Siempre se creara un indice unico en el campo _id durante la creacion de una coleccion

        - EL indice _id evita que los clientes inserten dos documentos con el mismo valor para el campo _id

        - No se puede borrar el _id

        - TTL: Indices especiales que pueden usar para eliminar automaticamente documentos de un coleccion despues de un cierto periodo de tiempo

replicacion

    - Conjunto de replicas es un grupo de instancias mongod que mantienen el mismo conjunto de datos

    - Un conjunto de replicas contiene varios nodos que contienen datos y,opcionalemente un nodo

    - De los nodos que contienen datos, uno y solo un miembro se considera el nodo primario, mientras que los otros nodos se consideran nodos secundarios
