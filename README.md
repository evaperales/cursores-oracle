# Cursores en Oracle

## Introducción
Nos referimos a un cursor en bases de datos como una estructura de control utilizada para el recorrido y posterior procesamiento individual de las filas devueltas en una consulta. Aunque técnicamente, los cursores son fragmentos de memoria reservados para procesar los resultados de una consulta SELECT.
Se pretende mediante tres ejemplos de procedimeintos almacenados, ilustrar el uso de los cursores implícitos y explícitos. Para ello previamente se crean dos tablas a las cuales le introduciremos datos. Debes tener creada previamente una base de datos Oracle.

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
