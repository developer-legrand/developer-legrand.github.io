
# **SQL Server : MAIN COMMANDS**

### **This tutorial uses a relationship between the 'EMPLOYEE' table, the 'SERVICE' table and the 'PROJECT' table**

# **I - COMMAND 'CREATE'**

- ## **CREATE DATABASE**

```SQL server
-- Create DataBase 'SALARIE' if not exists
IF NOT EXISTS (SELECT * FROM sys.databases WHERE name = 'SALARIE')
BEGIN
CREATE DATABASE SALARIE;
END

-- WARNING : IMPORTANT TO TARGET (DATABASE MASTER TARGET BY DEFAULT)  
USE SALARIE;
```
***
- ## **CREATE TABLE**
``` sql server
-- Create Table 'DEPARTEMENT' if not exists
IF NOT EXISTS (SELECT * FROM sys.sysobjects WHERE name = 'DEPARTEMENT')
BEGIN
	CREATE TABLE DEPARTEMENT
	(DPTNO TINYINT NOT NULL,
	DPTNOM NVARCHAR(50) NOT NULL,
	DPTLOC NVARCHAR(50) NOT NULL,
	CONSTRAINT PK_DPTNO PRIMARY KEY (DPTNO)
	)
END
```

``` sql server
-- Create Table 'EMPLOYE' if not exists
IF NOT EXISTS (SELECT * FROM sys.sysobjects WHERE name = 'EMPLOYE') 
BEGIN
	CREATE TABLE EMPLOYE
	(EMPNO SMALLINT NOT NULL,
	EMPNOM NVARCHAR(50) NOT NULL,
	EMPMETIER NVARCHAR(50) NOT NULL,
	EMPEMBDATE DATE NOT NULL,
	EMPSAL MONEY NOT NULL,
	EMPCOMM MONEY,
	MGRNO SMALLINT,
	DPTNO TINYINT NOT NULL,
	CONSTRAINT PK_EMPNO PRIMARY KEY (EMPNO),
	CONSTRAINT FK_EMP_DPTNO FOREIGN KEY (DPTNO) REFERENCES DEPARTEMENT (DPTNO),
	CONSTRAINT FK_MGR_EMPNO FOREIGN KEY (MGRNO) REFERENCES EMPLOYE (EMPNO)
	);
END
```

``` SQL SERVER
--Create Table 'PROJET' if not exists
IF NOT EXISTS ( SELECT * FROM sys.sysobjects.SALARIE where name = 'PROJET')
BEGIN 
	CREATE TABLE PROJET(
	PROJNO INT IDENTITY(101,1),
	PROJNOM VARCHAR(5) NOT NULL,
	PROJBUDGET INT NOT NULL);
END
```


# **II - COMMAND 'INSERT'**

- INSERT VALUES IN 'EMPLOYE'
``` SQL SERVER
--INSERT INTO [nameOfTable] (column1, column2, ...)
--VALUES
INSERT INTO EMPLOYE (EMPNO, EMPNOM, EMPMETIER, MGRNO,  EMPEMBDATE,  EMPSAL, EMPCOMM, DPTNO)
VALUES
(7839,	'KING',		'PRESIDENT',	NULL,	'1981-11-17',	5000,	NULL,	10),
(7698,	'BLAKE',	'MANAGER',		7839,	'1981-05-01',	2850,	NULL,	30),
(7782,	'CLARK',	'MANAGER',		7839,	'1981-06-09',	2450,	NULL,	10),
(7566,	'JONES',	'MANAGER',		7839,	'1981-04-02',	2975,	NULL,	20),
(7844,	'TURNER',	'SALESMAN',		7698,	'1981-09-08',	1500,	0,		30),
(7900,	'JAMES',	'CLERK',		7698,	'1981-12-03',	950,	NULL,	30),
(7521,	'WARD',		'SALESMAN',		7698,	'1981-02-22',	1250,	500,	30),
(7654,	'MARTIN',	'SALESMAN',		7698,	'1981-09-28',	1250,	1400,	30),
(7934,	'MILLER',	'CLERK',		7782,	'1982-01-23',	1300,	NULL,	10),
(7788,	'SCOTT',	'ANALYST',		7566,	'1982-12-09',	3000,	NULL,	20),
(7902,	'FORD',		'ANALYST',		7566,	'1981-12-03',	3000,	NULL,	20),
(7876,	'ADAMS',	'CLERK',		7788,	'1983-01-12',	1100,	NULL,	20),
(7369,	'SMITH',	'CLERK',		7902,	'1980-12-17',	800,	NULL,	20),
(7499,	'ALLEN',	'SALESMAN',		7698,	'1981-02-20',	1600,	300,	30);
--SELECT * FROM EMPLOYE
```
![image](C:\Users\Stagiaire\Documents\GitHub\Merise\Employe\Salarie\SALARIE_EMPLOYE.png)


- INSERT VALUES IN 'DEPARTEMENT'
``` SQL SERVER
--INSERT INTO [nameOfTable] (column1, column2, ...)
--VALUES
	INSERT INTO DEPARTEMENT (DPTNO, DPTNOM, DPTLOC)
	VALUES 
	(10, 'ACCOUNTING', 'NEW YORK'), 
	(20, 'RESEARCH', 'DALLAS'), 
	(30, 'SALES', 'CHICAGO'), 
	(40, 'OPERATIONS', 'BOSTON');
--SELECT * FROM DEPARTEMENT
```
![image](C:\Users\Stagiaire\Documents\GitHub\Merise\Employe\Salarie\SALARIE_DEPARTEMENT.png)

- INSERT VALUES IN 'PROJET'
``` SQL SERVER
--INSERT INTO [nameOfTable] (column1, column2, ...)
--VALUES
INSERT INTO PROJET (PROJNOM, PROJBUDGET) 
VALUES
('ALPHA', 96000),
('BETA',  82000),
('GAMMA', 15000);
--SELECT * FROM PROJET
```
![image](C:\Users\Stagiaire\Documents\GitHub\Merise\Employe\Salarie\SALARIE_PROJET.png)


# **III - COMMAND 'PRIMARY KEY', 'FOREIGN KEY', 'CONSTRAINT' & 'ADD'**
### **DEFINITION :**
-  Une clé primaire d'une table permet d'identifier chaque enregistrement d'une table d'une base de données.Une table ne peut avoir qu'une et une seule clé primaire. Cependant une clé primaire peut contenir plusieurs colonnes. Pour cela, lors de la déclaration de la clé primaire, il suffit de mettre entre parenthèses toutes les colonnes désirées.


- Les clés étrangères font référence à des clés primaires de tables voisines.
Lorsque l'on utilise les mots clé FOREIGN KEY, il faudra ensuite donner la colonne de la table qui va devenir clé étrangère. Ensuite on donne la clé primaire de la table référencée. Comme les clés primaires, les clés étrangères peuvent être ajoutés comme contrainte ou non et peuvent être mise en place lors de la création de la table ou via modification de la table.


- ## **Constraint Primary Key & Foreign Key**

```SQL SERVER
-- ALTER TABLE [nameOfTable] 
-- ADD CONSTRAINT [FK_nameOfForeignKey] FOREIGN KEY (nameOfForeignKey) 
-- REFERENCES [nameReferencesTable] (nameOfPrimaryKey);
BEGIN
	CREATE TABLE EMPLOYE
	(EMPNO SMALLINT NOT NULL,
	EMPNOM NVARCHAR(50) NOT NULL,
	EMPMETIER NVARCHAR(50) NOT NULL,
	EMPEMBDATE DATE NOT NULL,
	EMPSAL MONEY NOT NULL,
	EMPCOMM MONEY,
	MGRNO SMALLINT,
	DPTNO TINYINT NOT NULL,
	CONSTRAINT PK_EMPNO PRIMARY KEY (EMPNO),
	CONSTRAINT FK_EMP_DPTNO FOREIGN KEY (DPTNO) REFERENCES DEPARTEMENT (DPTNO),
	CONSTRAINT FK_MGR_EMPNO FOREIGN KEY (MGRNO) REFERENCES EMPLOYE (EMPNO)
	);
END
```

- ## **Add constraint of primary key**

```SQL SERVER
-- ALTER TABLE [nameOfTable] 
-- ADD CONSTRAINT [PK_nameOfPrimaryKey] PRIMARY KEY (nameOfPrimaryKey);
BEGIN
	ALTER TABLE DEPARTEMENT
	ADD CONSTRAINT PK_DPTNO PRIMARY KEY (DPTNO)
END
```

- ## **Add column in existing table**
```SQL SERVER
-- ALTER TABLE [nameOfTable] 
-- ADD [nomDeLaColonne] [typeOfValue];
BEGIN
	ALTER TABLE EMPLOYE
	ADD PROJNO INT;
END
```
![image](C:\Users\Stagiaire\Documents\GitHub\Merise\Employe\Salarie\ADD_COLUMN.png)


# **III - COMMAND ORDER CLASSIFICATION 'GROUP BY', 'ORDER BY', ASC, DESC**


<style>
h3 {
    color:red;
}
</style>