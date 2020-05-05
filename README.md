# Cursores en Oracle

## Introducción
Nos referimos a un cursor en bases de datos como una estructura de control utilizada para el recorrido y posterior procesamiento individual de las filas devueltas en una consulta. Aunque técnicamente, los cursores son fragmentos de memoria reservados para procesar los resultados de una consulta SELECT.
Se pretende mediante tres ejemplos de procedimientos almacenados, ilustrar el uso de los cursores implícitos y explícitos. Para ello previamente se crean dos tablas a las cuales les introducimos datos. Debes tener creada previamente una base de datos en Oracle.

## Creación de tablas

```
CREATE TABLE departamento
(
  CodDep NUMBER NOT NULL,
  NomDep VARCHAR2(50) NOT NULL,
  PRIMARY KEY (CodDep)
);

CREATE TABLE empleado
(
  CodEmp NUMBER NOT NULL,
  NomEmp VARCHAR2(50) NOT NULL,
  ApeEmp VARCHAR2(80) NOT NULL,
  CodDep NUMBER NOT NULL,
  PRIMARY KEY (CodEmp),
  FOREIGN KEY (CodDep) REFERENCES departamento(CodDep)
);
```

## Inserción datos en las tablas

´´´console
INSERT ALL 
INTO departamento  VALUES (1,'DEP1') 
INTO departamento  VALUES (2,'DEP2') 
INTO departamento  VALUES (3,'DEP3') 
SELECT * FROM dual;

INSERT ALL 
INTO empleado  VALUES (1,'EMP1','APELLIDOS 1',1) 
INTO empleado  VALUES (2,'EMP2','APELLIDOS 2',1) 
INTO empleado  VALUES (3,'EMP3','APELLIDOS 3',2)
INTO empleado  VALUES (4,'EMP4','APELLIDOS 4',2) 
INTO empleado  VALUES (5,'EMP5','APELLIDOS 5',3) 
INTO empleado  VALUES (6,'EMP6','APELLIDOS 6',3) 
SELECT * FROM dual;
´´´


