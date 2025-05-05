
    2025-03-31

    Base de datos 2    

---------------------------------

## MongoDB

### Replicacion

La replicacion es el copiado de un archivo en multiples nodos para evitar perdida de datos. Un archivo va a tener un nodo primario y multiples nodos secundarios

### Replicacion

El sharding es un metodo para distribuir datos a traves de multiples maquinas. Utiliza fragmentacion para permitir impletemtaciones con conjuntos de datos muy grandes y operaciones de alto rendimiento

Acepta dos metodos de escalado
    
    - Escalado Vertical
    - Escalado horizontal 

Cuando uno realiza una consulta, procesos especiales llamados Routers (mongos) te redirigen al shard correcto
Su funcionamiento es similar a un Router de internet

La replicacion esta compuesta por

    - Fragmento: Cada fragmento contiene un subconjunto de los atos fragmentados. Cada fragmento se puede implementar como un conjunto de replicas

    - mongos: Enrutador de consultas, proporcionando una interfaz entre las aplicaciones del cliente y el cluster fragmentado 

    - servidor de configuracion: Almacenan metadata y ajuste de configuracion para el cluster, se deben implementar como un conjunto de replica (CSRS - Configuration server replica set)

### Manejo de la fragmentacion
    
    - Shard Keys: La clave de fragmento se utiliza para distribuir los deocumentos de la coleccion entre fragmentos 

    - La clavede fragmento consiste en un campo o campos que existen en cada documento enla coleccion de ddestino

    - Hay que ser cuidadoso al elegir la clave de fragmento al fragmentar una coleccion ya que la eleccion no se puede cambiar despues de fragmentar y afectara al rendimiento
    
    - Para fragmentar una coleccion no vacia, la coleccion debe tener un indice que comience con la clave de fragmento

### Shard keys funcionamiento

    - Clave para agrupar logicamente documentos que MongoS se encargara de distribuir entre los distintos fragments, cada uno de destos grupos logicos se le denomina chunk

    - El campo o campos utilizados para definir la shard key son los utilizados para definir el valor minimo inclusivo y maximo exclusivo para ubicar un documento en un chunk
    
    - Todos los documentos de una sharded collection tienen un valor

    - Las operaciones de lectura utilizan la shard key para recuperar directamente el dato del chunk correcto
    
    - De no ser asi, mongos tendra que consultar todos los chunks para comprobar que los documentos cumplan la condicion de busqueda

### Criterio de seleccion

Cardinalidad: Numero de elementos unicos que la clave puede contener, es necesaria una alta Cardinalidad

Frecuencia: Las claves escogidas deben tener baja frecuencia, es decir, cada valor deberia repetirse el menor numero de veces posible

Cambios monotonos: Se debe evitar  que los cambios entre el valor de la clave en un documento y el valor de la clave en el siguiente documento, tengan una tasa estable y predecibl - La clavede fragmento consiste en un campo o campos que existen en cada documento enla coleccion de ddestino

    - Hay que ser cuidadoso al elegir la clave de fragmento al fragmentar una coleccion ya que la eleccion no se puede cambiar despues de fragmentar y afectara al rendimiento
    
    - Para fragmentar una coleccion no vacia, la coleccion debe tener un indice que comience con la clave de fragmento

### Shard keys funcionamiento

    - Clave para agrupar logicamente documentos que MongoS se encargara de distribuir entre los distintos fragments, cada uno de destos grupos logicos se le denomina chunk

    - El campo o campos utilizados para definir la shard key son los utilizados para definir el valor minimo inclusivo y maximo exclusivo para ubicar un documento en un chunk
    
    - Todos los documentos de una sharded collection tienen un valor

    - Las operaciones de lectura utilizan la shard key para recuperar directamente el dato del chunk correcto
    
    - De no ser asi, mongos tendra que consultar todos los chunks para comprobar que los documentos cumplan la condicion de busqueda

### Criterio de seleccion

Cardinalidad: Numero de elementos unicos que la clave puede contener, es necesaria una alta Cardinalidad

Frecuencia: Las claves escogidas deben tener baja frecuencia, es decir, cada valor deberia repetirse el menor numero de veces posible

Cambios monotonos: Se debe evitar  que los cambios entre el valor de la clave en un documento y el valor de la clave en el siguiente documento, tengan una tasa estable y predecible

### Hashed Shard Keys

    - Calculan el valor hash de la clave y lo utilizan para decidir donde almacenar el documento, no cambian el valor sustituyendolo por su hash sino utilizandolo en el indice correspondiente facilitando una mejor distribuicion de documento entre chunks

    - Las utilizamos cuando las claves incumplen las condiciones antes mencionadas.si aplicamos una funcion hash, facilitamos la alta Cardinalidad y baja frecuencia

    - Consideraciones

        - Se corre el riesgo de que los documentos se distribuyan entre todos los hunks y se deba consultar todos ellos para obtener un dato concreto

        - No podemos utilizar operaciones de lectura aisladas geograficamente

        - El hash debe calcularse sobre un unico campo y que no sea un array

### Base de datos, colecciones y documentos

    - Los documentos (objetos) corresponden a tipos de datos nativos en muchos lenguajes de programacion

    - Los documentos y arreglos integrados reducen la necesidad de join

    - Se almacenan en colecciones

Los documentos se almacenan como registros de datos en colecciones y las colecciones en bases dedatos. El formato de los mismo se denomina BSON

