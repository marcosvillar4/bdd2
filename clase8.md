
    2025-04-28

    Bases de datos 2 

_________________________

# Bases de datos Tabulares

    - Tambien llamadas bases de datos columnares

El modelo de datos esta conformao por un modelo tabular, donde cada fila puede tener una configuracion diferente de columnas 

Las bases de datos tabulares son buenas en:

    - Gestion de tamaño 
    - Cargas de escrituras masivas orientadas al stream 
    - Alta disponibilidad
    - MapReduce

## RDBMS vs Tabulares

| RDBMS               | Cassandra       |
|-------------------- | --------------- |
| Database Instance   | Cluster         |
| Database            | Keyspace        |
| Table               | Column family   |
| Row                 | Row             |
| Column              | Column          |

Son buena opcion para registrar informacion de eventos como estado de las aplicaciones o errores encontradas en estas 

Utilizando familias de columnas, puede almacenar entradas de blog con etiquetas, categorias, enlaces y vinculos de referencia en diferentes columnas 

## Casos no aptos

    1) Sistemas de transacciones ACID

    2) Consultas con agregacion de datos (SUM, AVG) deben ser client-side

    3) No es ideal para prototipos tempranos donde no se tenga con seguridad sobre los patrones de Consultas

# Cassandra

Almacenamiento de estructuras clave-valor; Altamente escalable

    - Iniciado por Facebook
    - Codigo abierto en 2008
    - Proyecto apache en 2009
    - Licencia: Apache 2.0
    - Escrito en Java
    - Multiplataforma
    
## Caracteristicas

    - Esquema dinamico: El esquema puede ser modificado en runtime
    - No hay unico punto de fallo: Replicacion automatica a varios nodos
    - Alta disponibilidad: Alta disponibilidad debido a la replicacion 
    - Particionado de los datos: Se distribuyen los datos para evitar cuello de botella en el acceso de datos
    - Escalabilidad horizontal 

## Arquitectura

    - Varios nodos independientes comunicados mediante un protocolo P2P
    - No hay nodos principales, cliente se conecta a cualquier nodo 

## Replicacion de datos 

En Cassandra, uno o mas nodos de un cluster actuan como replicas de un dato dado. Si se detecta que algunos de los nodos respondieron con un valor desactualizado, Cassandra devolvera el valor mas reciente al cliente.
Despues de devolver el valor mas reciente, Cassandrarealiza una reparacion e lectura en segundo plano para actualizar los valores obsoletos

## Terminos

    - Nodo: Lugar de almacenamiento de datos 
    - Datacenter: Coleccion de nodos relacionados
    - Cluster: Componente con uno o mas Datacenter
    - Commit log: Registro de confirmacion, es un mecanismo de recuperacion de fallas en Cassandra. Cada operacion de escritura se escribe en el registro de confirmacion
    - MemTable: Estructura de datos residente en la memoria. Despues del CommitLog, Los datos se escribiran en la SStable desde MemTable
    - SSTable: Archivo de disco donde se almacena el contenido de MemTable. Ficheros inmutables
    - Filtros de Bloom: Algoritmos no deterministas para probar si un elemento es miembro de un conjunto. Tipo especial de cache 

## Actividad de escritura 

    - Cada actividad deescritura de nodos es capturada por el CommitLog
    - Mas tarde los datos son capturados y almacenados en MemTable
    - Cuando la MemTable se llena, Se escriben los datos en SStable
    - Todas las escrituras se parten y replican en el cluster
    - Cassandra consolida periodicamente los SStable, descartando datos innecesarios

    1) Escritura en CommitLog
    2) Escritura en MemTable
    3) Flush de MemTable
    4) Escritura en SStable
    5) Compactacion de SStable

## Peticiones de escritura a nivel de cluster de nodos 

    - Nodo coordinador envia una peticion de escritura a todas las replicas 
    - Datos se escriben en todos los nodos 
    - El nivel de consistencia determina cuantos nodos han de responder a la peticion para determinar que la escritura se ha realizado con exito
    - El exito de la escritura significa que fue escrito en el CommitLog y el MemTable
    - Todos los nodos pueden ser coordinaores en una peticion
    - La seleccion de nodo coordinador en cada peticion se realiza en base a una politica predefinida en la base de datos

## Peticiones de borrado

    - Cuando una fila es eliminada, no se borra del disco (SSTable inmutables), sino que se marca como eliminada (tombstone)
    - Durante la compactacion, las filas marcadas se eliminan
    - Tiempo de gracia: Si un nodo que contiene la fila a ser eliminada se encuentra caido en ese momento, aun tiene la marca 
    - Posible de recibir la orden de eliminacion si se recupera en un intervalo de tiempo predefinido
    - Si no se recupera en ese tiempo, la fila no sera marcada como eliminada, por lo que pueden existir inconsistencias en las replicas. Por ello, se recomienda que los administradores realicen cada cierto tiempo tareas de mantenimiento en Cassandra
    - LLTS: Los datos pueden tener una "fecha de vencimiento"

## Peticiones de lectura

    - Cada nodo que recibe la lectura, se accede mediante "key" a la MemTable para recuperar los datos requeridos
    - Si los datos no se encuentran en la MemTable, se accede a las SSTable
    - Si no se ha realizado la tarea de compactacion recientemente, es probable que haya que leer varias SSTables para recuperar las filas 
    - Se define un nivel de consistencia (Consistency level) en la recuperacion de informacion. Defina cuantos nodos tienen que responder para tomar la respuesta comocorrecta
    - Mecanismos de obtencion de datos:
        - Direct read request: El nodo coordinador contacta un nodo que contenga la replica requerida y la recupera
        - Digest request: Contacta con tantos nodos con replicas como se determine el nivel de consistencia. Se comprueba la consistencia de datos retornados por el nodo contactado mediante Direct Read request
        - Background read repair request: En caso de existir inconsistencias en las replicas, las replicas con un timestamp mas antiguo son sobreescritas con datos mas recientes

## seguridad

    - seguridad a nivel de usuarios: Logins, permisos de gestion, administracion mediante GRANT/REVOKE
    - Opciones de encriptacion tanto entre clientes y clusters como entre nodos
    - Varias opciones de backup
    - Herramientas externas para seguridad adicional

## Consistencia 
    
Ante la posibilidad de encontrar datos inconsistentes en los nodos a pesar de tombstones, se recomienda realizar tareas rutinarias de mantenimiento

## Estructura de datos

    - Columna (Columns): Unidad basica de almacenamiento 
    - Filas (Rows): Conjunto de columnas de una familia con valor asignado
    - Familias de columnas (ColumnFamilies): Contenedor de multiples columnas, se acceden a filas individuales mediante la clave de fila 
    - Keyspace: Agrupacion de familias de columnas, Normalmente un Keyspace por aplicacion
    - Cluster: Los nodos de una instancia de Cassandra, pueden contener multiples Keyspaces

### Columna

Estructura:

    - Name: Nombre de la columna 
    - Value: Valor almacenado 
    - timestamp: Fecha y hora de escritura 

### Fila (Row)

    - Agregacion de columnas con un nombre para referenciarlo (rowKey)
    - Equivalente a la fila en el modelo relacional

### rowKey

Es el equivalente de la clave primaria en el modelo relacional. Tiene dos partes:

    - Partition Key: Las filas con el mismo valor de clave de particionado se almacenan en la misma particion del disco
    - Clustering key: Determina el orden fisico en el que se almacenan las filas

### KeySpace 

Equivalente a el esquema del modelo relacionas, generalmente uno por aplicacion, atributos basicos:

    - Replication_factor: Cuanto queres replicar los datos 
    - Replica placement strategy: Como se colocan las replicas en el anillo (SimpleStrategy, NetworkTopologyStrategy)

Cassandra ogrece soporte para particionado distribuido de datos:

    - RandomPartitioner: Buen balanceo de Cargas
    - OrderPreservingPartitioner: Permite ejecutar consultas de rangos, pero exige mas trabajo eligiendo node tokens.

### Indices

#### Primarios

    - Unicos por filas 
    - Filas asignadas a cada nodo por el partitioner y el replica placement strategy 
    - Dado que cada nodo conoce que rango de valores del indice primario almacena, las consultas pueden realizarse de forma eficiente escaneando unicamente los indices de las replicas de las filas buscadas

### Secundarios
    
    - Se permiten en Cassandra
    - Son indices sobre columnas
    - Se implementan como una tabla oculta, separada de la tabla que contiene los valores originales
    - No se recomienda para gran volumen de dato

### Secundarios
    
    - Se permiten en Cassandra
    - Son indices sobre columnas
    - Se implementan como una tabla oculta, separada de la tabla que contiene los valores originales
    - No se recomienda para gran volumen de dato

### Secundarios
    
    - Se permiten en Cassandra
    - Son indices sobre columnas
    - Se implementan como una tabla oculta, separada de la tabla que contiene los valores originales
    - No se recomienda para gran volumen de dato

### Secundarios
    
    - Se permiten en Cassandra
    - Son indices sobre columnas
    - Se implementan como una tabla oculta, separada de la tabla que contiene los valores originales
    - No se recomienda para gran volumen de datos
