1. [Ejercicios DDL](#ddl)
    - [Proyectos de Investigación](#ddl1)
    - [Naves Espaciales](#ddl2)

## Ejercicios DDL<a name="ddl"></a>
### Proyectos de Investigación<a name="ddl1"></a>
---
```SQL
CREATE SCHEMA Investigacion;

CREATE TABLE Investigacion.Sede(
	Nome_Sede VARCHAR(30),    
	Campus VARCHAR(30) NOT NULL,
	CONSTRAINT PK_Sede
	  PRIMARY KEY (Nome_Sede)
);

CREATE TABLE Investigacion.Departamento(
	Nome_Departamento VARCHAR(30) PRIMARY KEY,
	Telefono CHAR(9) NOT NULL,
	Director CHAR(9);
);

CREATE TABLE Investigacion.Ubicacion(
	Nome_Sede VARCHAR(30),
	Nome_Departamento VARCHAR(30),
	PRIMARY KEY (Nome_Sede, Nome_Departamento),
	CONSTRAINT FK_Sede_Ubicacion
		FOREIGN KEY (Nome_Sede) REFERENCES Investigacion.Sede (Nome_Sede) 
		ON DELETE CASCADE 
		ON UPDATE CASCADE,
	CONSTRAINT FK_Departamento_Ubicacion  
		FOREIGN KEY (Nome_Departamento) REFERENCES Investigacion.Departamento (Nome_Departamento)
 		ON DELETE CASCADE 
		ON UPDATE CASCADE
);

CREATE TABLE Investigacion.Grupo(
	Nome_Grupo VARCHAR(30),
	Nome_Departamento VARCHAR(30),
	Area VARCHAR(30) NOT NULL,
	Lider CHAR(9),
	PRIMARY KEY (Nome_Grupo, Nome_Departamento),
	FOREIGN KEY (Nome_Departamento) REFERENCES Investigacion.Departamento (Nome_Departamento)
		ON DELETE CASCADE
		ON UPDATE CASCADE
);

CREATE TABLE Investigacion.Profesor(
	DNI CHAR(9) PRIMARY KEY,
	Nome_Profesor VARCHAR(30) NOT NULL,
	Titulacion VARCHAR(20) NOT NULL,
	Experiencia INTEGER,
	N_Grupo VARCHAR(30),
	N_Departamento VARCHAR(30),
	FOREIGN KEY (N_Grupo, N_Departamento) REFERENCES Investigacion.Grupo (Nome_Grupo, Nome_Departamento)
		ON DELETE SET NULL
		ON UPDATE CASCADE
);

ALTER TABLE Investigacion.Departamento(
	ADD CONSTRAINT FK_Profesor_Departamento
		FOREIGN KEY (Director) REFERENCES Profesor (DNI)
		ON DELETE SET NULL
		ON UPDATE CASCADE
);


CREATE TABLE Investigacion.Participa(
	DNI CHAR(9),
	Codigo_Proyecto INTEGER,
	Fecha_Inicio DATE NOT NULL,
	Fecha_Cese DATE,
	Dedicacion INTEGER NOT NULL,
	PRIMARY KEY (DNI, Codigo_Proyecto),
    CHECK (Fecha_Cese > Fecha_Inicio)
);

CREATE TABLE Investigacion.Proyecto(
	Codigo_Proyecto INTEGER PRIMARY KEY,
	Nombre_Proyecto VARCHAR(30) NOT NULL,
	Orzamento MONEY NOT NULL,
	Data_Inicio DATE NOT NULL,
	Data_Fin DATE,
	N_Gr VARCHAR(30),
	N_Dep VARCHAR(30),
   	Unique(Nome_Proyecto),
   	CHECK(Data_Inicio < Data_Fin)
);

ALTER TABLE Investigacion.Proyecto
	ADD CONSTRAINT FK_Grupo_Proyecto
		FOREIGN KEY (N_Gr, N_Dep) REFERENCES Grupo (Nome_Grupo, Nome_Departamento)
		ON DELETE SET NULL,
		ON UPDATE CASCADE;
 
CREATE TABLE Investigacion.Programa(
	Nome_Programa VARCHAR(30) PRIMARY KEY
);

CREATE TABLE Investigacion.Financia(
	Nome_Programa VARCHAR(30),
	Codigo_Proyecto INTEGER,
	Numero_Programa INTEGER NOT NULL,
	Cantidad_Financiada MONEY NOT NULL,
	PRIMARY KEY (Nome_Programa, Codigo_Proyecto)
);

ALTER TABLE Investigacion.Financia
	ADD CONSTRAINT FK_Programa_Financia
		FOREIGN KEY (Nome_Programa) REFERENCES Proyecto(Codigo_Proyecto)
		ON DELETE CASCADE
		ON UPDATE CASCADE;
		
ALTER TABLE Investigacion.Financia 
	ADD CONSTRAINT FK_Proyecto_Financia 
		FOREIGN KEY (Codigo_Proyecto)REFERENCES Investigacion.Proyecto (Codigo_Proyecto) 
		ON DELETE CASCADE 
		ON UPDATE CASCADE;

ALTER TABLE Investigacion.Participa 
	ADD FOREIGN KEY (Codigo_Proxecto) REFERENCES Investigacion.Proxecto (Codigo_Proxecto) 
	ON UPDATE CASCADE;

ALTER TABLE Investigacion.Participa 
	ADD FOREIGN KEY (Dni) REFERENCES Investigacion.Profesor (Dni)
	ON UPDATE CASCADE;

ALTER TABLE Investigacion.Grupo 
	ADD FOREIGN KEY (Lider) REFERENCES Investigacion.Profesor (Dni) 
	ON DELETE SET NULL 
	ON UPDATE CASCADE;
```
[Volver al índice](#indice)
### Naves Espaciales<a name="ddl2"></a>
---
```SQL
CREATE SCHEMA Naves;

CREATE TABLE Naves.Servicio(
	Clave_Servicio INTEGER,
	Nombre_Servicio VARCHAR(30),
	PRIMARY KEY (Clave_Servicio,Nombre_Servicio)
);

CREATE TABLE Naves.Dependencia(
	Codigo_Dependencia INTEGER PRIMARY KEY,
	Nombre_Dependencia VARCHAR(30) NOT NULL,
	Clave_Servicio INTEGER NOT NULL,
	Nombre_Servicio VARCHAR(30) NOT NULL,
	Funcion VARCHAR(50),
	Localizacion VARCHAR(100),
	UNIQUE (Nombre_Dependencia),
	FOREIGN KEY (Clave_Servicio, Nombre_Servicio) REFERENCES Naves.Servicio(Clave_Servicio, Nombre_Servicio)
		ON DELETE CASCADE
		ON UPDATE CASCADE
);

CREATE TABLE Naves.Camara(
	Codigo_Dependencia INTEGER PRIMARY KEY,
	Categoria VARCHAR(50),
	Capacidad INTEGER NOT NULL,
	FOREIGN KEY (Codigo_Dependencia) REFERENCES Naves.Dependencia(Codigo_Dependencia)
		ON DELETE CASCADE
		ON UPDATE CASCADE
);

CREATE TABLE Naves.Tripulacion(
	Codigo_Tripulacion INTEGER PRIMARY KEY,
	Nombre_Tripulacion VARCHAR(30) NOT NULL,
	Codigo_Camara INTEGER NOT NULL,
	Codigo_Dependencia INTEGER NOT NULL,
	Categoria VARCHAR(50) NOT NULL,
	Antigüedad INTEGER DEFAULT 0,
	Procedencia VARCHAR(50) NOT NULL,
	Adm VARCHAR(50) NOT NULL,
	FOREIGN KEY (Codigo_Camara) REFERENCES Naves.Camara(Codigo_Dependencia)
		ON DELETE CASCADE
		ON UPDATE CASCADE
	FOREIGN KEY (Codigo_Dependencia) REFERENCES Naves.Dependencia(Codigo_Dependencia)
		ON DELETE CASCADE
		ON UPDATE CASCADE
);

CREATE PLANETA Naves.Planeta(
	Codigo_Planeta INTEGER PRIMARY KEY,
	Nombre_Planeta VARCHAR(30) NOT NULL UNIQUE,
	Galaxia VARCHAR(30) NOT NULL,
	Coordenadas INTEGER NOT NULL,
	UNIQUE(Coordenadas)
);

CREATE TABLE Naves.Visita(
	Codigo_Tripulacion INTEGER,
	Codigo_Planeta INTEGER,
	Fecha_Visita DATE,
	Tiempo INTEGER NOT NULL,
	PRIMARY KEY (Codigo_Tripulacion,
	Codigo_Planeta,Fecha_Visita),
	FOREIGN KEY (Codigo_Tripulacion) REFERENCES Naves.Tripulacion(Codigo_Tripulacion)
		ON DELETE CASCADE
		ON UPDATE CASCADE,
	FOREIGN KEY (Codigo_Planeta) REFERENCES Naves.Planeta(Codigo_Planeta)
		ON DELETE CASCADE
		ON UPDATE CASCADE
);

CREATE TABLE Naves.Raza(
	Nombre_Raza VARCHAR(30) PRIMARY KEY,
	Altura INTEGER NOT NULL,
	Anchura INTEGER NOT NULL,
	Peso INTEGER NOT NULL,
	Poblacion_Total INTEGER NOT NULL
);

CREATE TABLE Naves.Habita(
	Codigo_Planeta INTEGER,
	Nombre_Raza VARCHAR(30),
	Poblacion_Parcial INTEGER NOT NULL,
	PRIMARY KEY (Codigo_Planeta,Nombre_Raza),
	FOREIGN KEY (Codigo_Planeta) REFERENCES Naves.Planeta(Codigo_Planeta)
		ON DELETE CASCADE
		ON UPDATE CASCADE,
	FOREIGN KEY (Nombre_Raza) REFERENCES Naves.Raza(Nombre_Raza)
		ON DELETE CASCADE
		ON UPDATE CASCADE
);

ALTER TABLE Naves.Camara
	ADD CONSTRAINT Capacidade_maior_de_cero
		CHECK (Capacidad > 0);
```
[Volver al índice](#indice)

