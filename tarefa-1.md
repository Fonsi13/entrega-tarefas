# Apuntes Sublenguajes SQL 
## Índice <a name="indice"></a>
 1. [Sublenguajes](#sub)
 2. [DDL](#DDL)
    - [Crear bases de datos](#database)
    - [Crear tablas](#tabla)
      - [Tipos de Datos](#datos)
      - [Restricciones](#rest)
        - [Clave primaria](#pk)
        - [Clave secundaria](#fk)
        - [Unicidad](#uni)
        - [Check](#check)
     - [Borrado](#drop)
       - [Borrar bases de datos](#drop1)
       - [Borrar tablas](#drop2)
     - [Modificación](#alter)
	     - [Añadir columna](#alter1)
	     - [Borrar columna](#alter3)
	     - [Añadir restricción](#alter4)
	     - [Borrar restricción](#alter5)
 3. [DML](#DML)
	 - [Insertar datos](#insert)
	 - [Actualizar datos](#up)
	 - [Borrar datos](#del)

 

## Sublenguajes de SQL <a name="sub"></a>

 - DDL (Data Definition Language)
   - Manipula objetos de la base de datos como tablas, columnas, filas, etc.
   - Comandos:
     - CREATE
     - ALTER
     - DROP
 - DML (Data Manipulation Language)
   - Actúa sobre los datos de la base de datos
   - Comandos:
     - INSERT
     - UPDATE
     - DELETE
 - DQL (Data Query Language)
   - Recupera información de los objetos de la base de datos
   - Comandos:
     - SELECT (antes se incluía en DML)
 - TCL (Transaction Query Language)
   - Aplican cambios de forma permanente que se guardan en la base de datos
   - Comandos:
     - COMMIT
     - ROLLBACK
     - SAVEPOINT
 - DCL (Data Control Language)
   - Dan permisos para acceder a datos restringidos o compartir información entre usuarios
   - Comandos:
     - GRANT
     - REVOKE  
 - SCL (Session Control Language)
   -  Gestiona dinámicamente las propiedades de una sesión de usuario
   - Comandos:
     -  ALTER SESSION
     - SET ROLL

[Volver al Índice](#indice)
## DDL (Data Definition Language) <a name="DDL"></a>
Opera sobre los objetos de la base de datos.
### CREATE DATABASE | SCHEMA<a name="database"></a>
---
Si queremos crear una base de datos desde cero debemos usar la siguiente sintaxis:
```SQL
CREATE (DATABASE|SCHEMA) [IF NOT EXISTS] <nombreBD> [CHARACTER SET <nomCharset>];
```
Para crearla podemos usar DATABASE o SCHEMA, la diferencia reside principalmente en los permisos en el momento de crear la Base de Datos.
Ejemplos:
```SQL
CREATE DATABASE bd_1;
```
```SQL
CREATE SCHEMA bd_2;
```
Además existen opciones:
- `IF NOT EXISTS`: Comprueba que no exista otra base de datos con el mismo nombre en el SGBD.
- `CHARACTER SET`:  Precisa el conjunto de caracteres que se va a usar.

[Volver al Índice](#indice)
### CREATE TABLE <a name="tabla"></a>
---
Para poder añadir tablas a la base de datos hay que usar la siguiente sintaxis:
```SQL
CREATE TABLE nom_tabla(
	<nom_atributo1> tipo_Dato [CONSTRAINT],
	<nom_atributo1> tipo_Dato,
	[CONSTRAINT]
);
```
Si se quiere se puede añadir restricciones a los atributos definidos, llamadas **CONSTRAINT**.

Más adelante se profundizará en ese campo.

Ejemplo:
```SQL
CREATE TABLE jugadores(
	nombre VARCHAR(100) PRIMARY KEY,
	goles INTEGER NOT NULL,
	equipo VARCHAR(200),
	FOREIGN KEY (equipo) REFERENCES equipo (nombre)
);
```
[Volver al Índice](#indice)
#### Tipos de Datos <a name="datos"></a>
---
Cuando se declara un atributo a la hora de crear una tabla se le puede asignar muchos tipos de datos y diferentes restricciones.

Hay una gran variedad de tipos de datos, por eso solo vamos a mencionar los más destacados:
 - Numéricos
   - INTEGER
   - DECIMAL (Preciso)
   - REAL (No preciso)
   - BIGINT
   - SMALLINT
  - Texto 
    - CHAR (Longitud fija)
    - VARCHAR (Longitud variable)
    - TEXT (Longitud ilimitada variable)
   - Fecha
     - DATE 
     - TIME
     - TIMESTAMP (Incluye DATE y TIME)
    - Booleano
      -  BOOLEAN (true, false, null)
   - Moneda
      - MONEY
    - Otros
       - UUID (Identificador Único)
       - JSON
       - CIDR
       - INET

A la hora de crear una base de datos se pueden declarar muchos atributos del mismo tipo, y con el fin de ahorrar un poco de código y simplificarlo se pueden crear al principio como variables.
Ejemplo:
```SQL
CREATE DOMAIN Tipo_Nombre VARCHAR(100)
```
```SQL
CREATE TABLE jugadores(
	nombre Tipo_Nombre,
	goles INTEGER,
	equipo VARCHAR(200)
);
```
Si quiere también añadir una **CONSTRAINT**  a un atributo se puede especificar de dos maneras diferentes, una extendida y otra simplificada.
- Extendida:
```SQL
nombre VARCHAR(100), 
CONSTRAINT PK_Nombre
	PRIMARY KEY (nombre)
```
- Simplificada:
```SQL
nombre VARCHAR(100) PRIMARY KEY 
```
Es más recomendable la primera, para tenerlo todo mejor estructurado y no provocar confusiones.

A continuación hablaremos de los diferentes tipos de **RESTRICCIONES**

[Volver al Índice](#indice)
#### CONSTRAINTS  <a name="rest"></a>
---
##### PRIMARY KEY<a name="pk"></a>
Cada vez que creamos un tabla nueva hay que asignar una clave principal a uno o varios atributos.
- Un atributo:
```SQL
CREATE TABLE jugadores(
	nombre VARCHAR(100) PRIMARY KEY,
	...
);
```
o
```SQL
CREATE TABLE jugadores(
	nombre VARCHAR(100),
	...
	[CONSTRAINT PK_Nombre] PRIMARY KEY (nombre)
);
```

> Lo que va entre corchetes nos obligatorio incluirlo, pero en algunos casos ayuda
- Dos o más atributos
```SQL
CREATE TABLE jugadores(
	nombre VARCHAR(100),
	DNI INTEGER,
	...
	[CONSTRAINT PK_Nombre] PRIMARY KEY (nombre, DNI)
```
> Cuando la clave principal es más de un atributo siempre se indica al final y se coloca entre paréntesis y separados por una coma el nombre de los atributos

Por último, la **PRIMARY KEY** no puede ser nunca null

[Volver al Índice](#indice)
##### FOREIGN KEY<a name="fk"></a>
La clave foránea nos permite establecer conexiones entre tablas, y su sintaxis es la siguiente:
```SQL
[CONSTRAINT <nomRestricción>]
	FOREIGN KEY (<nomAtributo>)	REFERENCES <tablaAjena>(<nombreAtributoAjeno>)
	[ON DELETE CASCADE | NO ACTION | SET NULL | SET DEFAULT]
	[ON UPDATE CASCADE | NO ACTION | SET NULL | SET DEFAULT]
	[MATCH FULL | MATCH PARTIAL]
```
A la hora de crear la clave foránea tenemos varios parámetros opcionales:
- **ON DELETE**: indica el tipo de acción que se realizará sobre la tabla a la que referencia la clave foránea en caso de borrado 
- **ON UPDATE**: indica el tipo de acción que se realizará sobre la tabla a la que referencia la clave foránea en caso de modificación de la clave principal
- **MATCH FULL**: no permite que ningún elemento tome el valor **NULL**, ni de la tabla con la clave primaria ni la que hace referencia la clave foránea 
- **MATCH PARTIAL**:  permite que los elementos tomen el valor **NULL**

Para **ON DELETE** y **ON UPDATE** existen cuatro valores que pueden ocupar:
- **NO ACTION**: valor por defecto, indica que no se realizará ningún tipo de acción en caso de borrado o modificación
- **CASCADE**: si se borra una tupla que queramos, se borrarán automáticamente todas aquellas que hagan referencia a ella; se aplica la misma lógica al modificado, si se modifica una, automáticamente cambian el resto
-  **SET NULL**: establece el valor en la tabla ajena en *NULL* en caso de borrado o modificación
- **SET DEFAULT**: establece el valor por defecto en la tabla ajena en caso de borrado o modificación

[Volver al Índice](#indice)
##### UNIQUE <a name="uni"></a>
Se aplica sobre las claves candidatas de un objeto,  aquellas que pueden ser primarias pero hay otra que se identifica mejor.

No permite que se repita los valores de un atributo.

Sintaxis:
```SQL
[CONSTRAINT <nomRestricción>]
	UNIQUE(<nom_atributo>)
```
Ejemplos:
```SQL
CREATE TABLE jugadores(
	nombre VARCHAR(100) PRIMARY KEY,
	DNI INTEGER, 
	goles INTEGER,
	equipo VARCHAR(200),
	CONSTRAINT Unique_Nombre
		UNIQUE(DNI)
);
```
o
```SQL
CREATE TABLE jugadores(
	nombre VARCHAR(100) PRIMARY KEY,
	DNI INTEGER UNIQUE, 
	goles INTEGER,
	equipo VARCHAR(200),
);
```
[Volver al Índice](#indice)
##### CHECK <a name="check"></a>
Es una restricción de comprobación, solo se puede modificar(borrado, inserción o modificación) la tupla cuando la comprobación del predicado devuelve true.

Tiene diferentes opciones:
-  **[ NOT DEFERRABLE]**: *NOT DEFERRABLE INITIALLY INMEDIATE* es el valor por defecto, lo que indica que la comprobación no se puede aplazar, antes de realizar la operación. 
-  **[DEFERRABLE]**: *DEFERRABLE INITIALLY DEFERRABLE*, esta opción permite posponer la comprobación

Sintaxis:
```SQL
[CONSTRAINT <nom_Restricción>]
	CHECK (predicado)
	[[NOT] DEFERRABLE ]
	[INITIALLY INMEDIATE | DEFERABLE]
```
[Volver al Índice](#indice)
### DROP<a name="drop"></a>
---
#### DROP SCHEMA <a name="drop1"></a>
---
La utilizamos para eliminar una base de datos.
```SQL
DROP SCHEMA [IF EXISTS] <nom_bd>;
```

> **IF EXISTS** asegura que la base de datos exista, en caso de que no, devuelve false

#### DROP TABLE <a name="drop2"></a>
---
La utilizamos para eliminar una tabla
``` SQL
DROP TABLE [IF EXISTS] <nom_Tabla> [CASCADE | RESTRICT];
```
> **CASCADE** significa que borras todo, tanto la tabla como las que tienen dependencia con ella
> **RESTRICT** significa no dejar borrar la tabla si existen tablas hijas que tienen dependencias

[Volver al Índice](#indice)
### ALTER<a name="alter"></a>
---
A la hora de alterar una tabla nos referimos a modificar las columnas, añadir o eliminar una, o modificar las restricciones, añadir o eliminar una, con la notación **ALTER TABLE**.
##### ADD [COLLUMN]  <a name="alter1"></a>
Sirve para añadir una columna a la tabla.
``` SQL
ALTER TABLE <nom_Tabla> ADD [COLLUMN] <atributo> <tipo_Dato>;
```
##### DROP [COLLUMN] <a name="alter3"></a>
Sirve para eliminar una columna de la tabla.
``` SQL
ALTER TABLE <nom_Tabla> DROP [COLLUMN] <atributo> [CASCADE | RESTRICT];
```
##### ADD  <a name="alter4"></a>
Sirve para añadir una restricción a la tabla.
``` SQL
ALTER TABLE <nom_Tabla> ADD <nom_Restriccion>;
```
##### DROP < restricción ><a name="alter5"></a>
Sirve para eliminar una restricción de la tabla.
``` SQL
ALTER TABLE <nom_Tabla> DROP <nom_Restriccion>;
```

En resumen, la estructura de **ALTER TABLE** es la siguiente:
``` SQL
ALTER TABLE <nombre-da-tabla> 
  ADD [COLLUMN] <atributo> <tipo_Dato> ...
  DROP [COLLUMN] <atributo> [CASCADE | RESTRICT]
  ADD <restricción> 
  DROP <restricción>;
```
[Volver al Índice](#indice)
## DDL (Data Manipulation Language) <a name="DML"></a>
Como su nombre indica, opera sobre los datos de la base de datos.
### INSERT<a name="insert"></a>
---
Sintaxis:
```SQL
INSERT INTO <nombreTabla> [ (<atributo1>, <atributo2>, ...) ] VALUES (<valor1>, <valor2>, ...) | SELECT ...;
```
Podemos introducir varias filas a la vez:
```SQL
INSERT INTO <nombreTabla> [ (<atributo1>, <atributo2>, ...) ] 
VALUES (<valor1>, <valor2>),
	   (<valor3>, <valor4>),
	   (<valor5>, <valor6>);
```
En el caso de que queramos introducir datos sin especificar el orden den entrada, los datos entrarán en el orden en el que se han creado las columnas  y si nos equivocamos va a dar error, por eso la opción más segura es siempre especificar el orden.

Si vamos a usar el `SELECT` hay que indicar el mismo número de columnas, la misma orden y el mismo tipo de datos que la tabla destino.

[Volver al Índice](#indice)
### UPDATE  <a name="up"></a>
---
Sintaxis:
```SQL
UPDATE <nom_Tabla> SET <atributo1> = <valor1>, <atributo2> = <valor2>, ...
	[WHERE <predicado>];
```
Para asegurarnos que solo se modifica los valores de las tuplas que queremos es recomendable utilizar el **WHERE** con una condición.

Ejemplo:
```SQL
UPDATE alumnos SET mayorEdad = 'si', 
	WHERE fechaActual - fechaNacimiento = 18;
```
[Volver al Índice](#indice)
### DELETE <a name="del"></a>
---
Sintaxis:
```SQL
DELETE FROM <nom_Tabla> [WHERE <predicado>];
```
Se utiliza para borrar datos específicos de una tabla, pero si no ponemos una condición con el **WHERE** se borrarían absolutamente todos los datos de ella.

Ejemplo:
```SQL
DELETE FROM profesor WHERE jubilado = 'si';
```
[Volver al Índice](#indice)

