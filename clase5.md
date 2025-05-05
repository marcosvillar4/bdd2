
    2025-04-07

    Base de datos 2 

____________________________________________

# OODB - Base de datos orientadas a objetos

    - Crecimiento de los lenguajes de programacion orientados a objetos para la construccion de soluciones de software

    - Excelentes para datos muy complejos (anidados, compuestos)

    - Muy usadas en sistemas de apoyo al diseño industrial (cad/cam), bases de datos para genetica y mapeo de genomas, etc.

    - Adaptan como modelo a el paradigma orientado a objetos

    - Permite estructuras complejas

    - Mix entre desarollo y la gestion de datos

    - Aplica las mismas caracteristicas que el OOP (Herencia, encapsulamiento, etc)

    - Bases de datos objeto-relacionales (OORDB) y frameworks como hibernate o ibatis

    - Las OODB puras proporcionan una gestion de bases de datos orientadas a objetos a todos los niveles, desde la definicion al lenguaje de consulta

## Objetos persistentes

    - Transitorios (Desaparecen cuanto la aplicacion finalice)

    - Persistentes (Se almacenan cuando finaliza la aplicacion)

Para hacer un objeto persistente hay que marcarlo como uno, o, Asociarlo a otro objeto persistente

## Ventajas OODB 

    - Encapsula tanto estado como comportamiento

    - Almacena las relaciones que tenga con otros objetos

    - Agrupa formando objetos complejos

    - Lenguaje de consulta mas expresivo

    - Herencia para la construccion de nuevos datos

## Principales productos

    - ObjectDB: Compatible con JPA y Java Data objects (JDO)

    - DB4O: Opcion ligera y embedida de uso en dispositivos moviles y sistemas embebidos. Integracion con Java y .NET

    - Versant ODBMS: Aplicaciones de mision critica, alta disponibilidad, escalabilidad y rendimiento. Accesible desde Java, C#, C++, Smalltalk y Python

    - GemStone/S: Combina una OODB con un entorno de ejecucion basado en smalltalk 

    - ObjectStore: Permite almacenar y manipular datos como objetos. Accesible desde C++

## ObjectDB (OODBMS)

Proporciona servicios de:

    - Almacenamiento

    - Recuperacion de datos 

    - Procesamiento de consultas

    - Transacciones

    - Administracion de bloqueos

    - OQL (Object Query Language)

Caracteristicas 

    - ODBMS completamente hecho en java 

    - Administrada solo por API estandard de Java (JPA 2 / JDO 2)

    - Admite el modo Cliente-servidor y modo embedido

    - Capacidades avanzadas de consulta e indexacion

    - Testeado en entornos multiusuario exigidos
    
    - Funciona con Tomcat, Jetty, GlassFish, JBoss y Spring

ObjectDB - JPA / JDO 

    - JPA (Java Persistence API) y JDO (Java Data Objects) son dos interfaces de persistencia en Java (formas de guardar y recuperar objetos Java en un medio de almacenamiento)

    - JPA: Especificacion estandar de java EE (ahora Jakarta EE) para trabajar con bases de datos relacionales (como MySQL, PostgreSQL, SQL Server, etc)

    - JDO: Una especificacion de persistencia, pero mas generica y flexible que JPA pero menos estandarizada

JPA

    - Datos representados por clases y objetos en lugar de tablas y registros

    - Utiliza POJO para representar datos persistentes simplifica significativamente la programacion de la base de Datos

    - Posee soporte integrado eliminando el ORM

    - Lenguaje de consulta basado en el SQL

JDO

    - JDO (Java Data Objects) es otro estandar de acceso a datos utilizando POJO para representar el modelo de objetos 

    - JDO esta diseñada para usarse con bases de datos relacionales orientadas a objetos
 
    - Compatible con varias bases de objetos, incluido ObjectDB 

    - Lenguaje de consultas basado en objetos

Anotaciones

    - Ver pptx clase 5


