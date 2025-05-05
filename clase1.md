
 Base de datos 2
 10/03/2025

---------------------------------------------------
    Transaccion: Unidad logica de trabajo
        |
        |----> Completa (Commit)
        |
        |----> No se hace (Rollback)

---------------------------------------------------

    Archivo Log: Almacena historial de comandos y acciones de manera secuencial, se pueden utilizar en caso de un fallo para recuperar
    contenido perdido

---------------------------------------------------

    A-tomicity: Se hace o no se hace algo
    C-onsistency: Se pasa de un estado consistente a otro
    I-solation: Cada transaccion solo conoce sus operaciones
    D-urability: Las operaciones una vez realizadas, no se pueden revertir (A menos que se use una nueva transaccion)

---------------------------------------------------

    Bloqueo

                          |--- Compartido
    - Tipos de bloqueo----|
                          |--- Exclusivos
        
    - Afectan a las lecturas y las escrituras

---------------------------------------------------

    Granularidad

    Cantidad de datos bloqueados
    |
    |- Fila: Fila individual de una tabla
    |
    |- Clave: Fila individual de un indice
    |
    |- Pagina: Bloque de datos que se lee por operacion de lectura (4/8/16/32kb/pagina)
    |
    |- Extent: Grupo de N paginas contiguas de datos o indice (4/8/16)
    |
    |- Table: Tabla completa
    |
    |- Database: Base de datos completa

--------------------------------------------------

    Big Data

        - Volume: Gran cantidad de datos
        - Velocity: Necesidad de ser analizados rapidamente
        - Variety:Datos estructurados, no estructurados, densos y dispersos, conectados y desconectados
        - Viability: Uso eficaz del volumen de datos
        - Veracity: Veracidad y precision
        - Visualisation: Presentacion comprensible de datos
        - Value: Capacidad de extraer informacion para la toma de decisiones

--------------------------------------------------

    NoSQL

    - A contrario de una base de datos relacional, los datos se guardan como un conjunto, sin separar los datos
    - Esquema prescindible
    - Desnormalizacion
    - Escalan horizontalmente de forma natural
    - No garantizan ACID

