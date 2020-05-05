# Ejemplos de Cursores en Oracle

## Introducción
Nos referimos a un cursor en bases de datos como una estructura de control utilizada para el recorrido y posterior procesamiento individual de las filas devueltas en una consulta. Aunque técnicamente, los cursores son fragmentos de memoria reservados para procesar los resultados de una consulta SELECT.
Se pretende mediante tres ejemplos de procedimientos almacenados, ilustrar el uso de los cursores implícitos y explícitos. Para ello previamente se crean dos tablas a las cuales les introducimos datos. Debes tener creada previamente una base de datos en Oracle.

## Crear tablas

```console
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

## Insertar datos en las tablas

```console
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
```

## Visualizar los resultados

Recuerda activar SERVEROUTPUT, para visualizar los resultados.

```console
SET SERVEROUTPUT ON;
```

## Crear un procedimiento almacenado que contiene un cursor implícito

Muestra el nombre del departamento cuyo código se pasa como parámetro.

```console
CREATE OR REPLACE PROCEDURE EJEMPLO1 (Codigo IN NUMBER)
IS 
    Nom departamento.NomDep%TYPE; /*Toma el mismo tipo de datos que NomDep*/
BEGIN
    SELECT NomDep INTO Nom FROM departamento WHERE CodDep = Codigo;
    IF SQL%FOUND THEN
        DBMS_OUTPUT.PUT_LINE(Nom); 
    END IF;
END;
```

## Ejecutar el procedimiento almacenado EJEMPLO1

```console
BEGIN
   EJEMPLO1 (3);
END;
```

## Crear un procedimiento almacenado que contiene un cursor explícito

Muestra el nombre y apellidos de todos los empleados.

```console
CREATE OR REPLACE PROCEDURE EJEMPLO2 
IS
    CURSOR C1 IS SELECT * FROM empleado;
    Datos empleado%ROWTYPE; /* Al recuperar la fila en Datos, los tipos de datos se infieran por defecto*/
BEGIN
    /*Abrimos el cursor*/
    IF NOT C1%ISOPEN THEN
        OPEN C1;
    END IF;
   /*Recorremos el cursor*/
    LOOP
         FETCH C1 INTO Datos;
         EXIT WHEN C1%NOTFOUND;
         DBMS_OUTPUT.PUT_LINE(Datos.NomEmp||' ' ||Datos.ApeEmp); /*Muestro las columnas que deseamos*/
    END LOOP;
    /*Cerramos el cursor*/
    CLOSE C1;
END;
```

## Ejecutar el procedimiento almacenado EJEMPLO2

```console
BEGIN
   EJEMPLO2;
END;
```

## Crear un procedimiento almacenado con parámetro de entrada que contiene un cursor explícito
Muestra el nombre y apellidos de todos los empleados del departamento pasado como parámetro.

```console
CREATE OR REPLACE PROCEDURE EJEMPLO3 (NombreD IN departamento.NomDep%Type) 
/*Indicamos que el parámetro de entrada sea del mismo tipo de datos que NomDep*/
IS
    CURSOR C1 IS SELECT NomEmp, ApeEmp FROM empleado E JOIN departamento D ON (E.CodDep=D.CodDep) WHERE NomDep LIKE NombreD ;
    NombreE empleado.NomEmp%TYPE; /*Toma el mismo tipo de datos que NomEmp*/
    ApellidosE empleado.ApeEmp%TYPE; /*Toma el mismo tipo de datos que ApeEmp*/
BEGIN
    /*Abrimos el cursor*/
    IF NOT C1%ISOPEN THEN
        OPEN C1;
    END IF;
   /*Recorremos el cursor, en este caso utilizamos otro tipo de bucle*/
    FETCH C1 INTO NombreE, ApellidosE;
    WHILE C1%FOUND
    LOOP
        DBMS_OUTPUT.PUT_LINE(NombreE||' ' ||ApellidosE); /*Muestro las columnas que deseo*/
        FETCH C1 INTO NombreE, ApellidosE;
    END LOOP;
    /*Cerramos el cursor*/
    CLOSE C1;
END;
```

## Ejecutar el procedimiento almacenado EJEMPLO3

```console
BEGIN
   EJEMPLO3 ('DEP1');
END;
```


