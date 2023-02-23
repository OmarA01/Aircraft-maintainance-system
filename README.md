# Princess Sumaya University for Technology

## Aircraft maintenance, repair and over whole Company

## (MRO)

```
Omar Asaad 20200411
Dalia Tamimi 20200288
Lujain Alfaqir 2019050 4
```
## Contents

##### ABSTRACT : ............................................................................................................................................................. 2

###### REQUIREMENTS.......................................................................................................................................................... 3

**_ER mode................................................................................................................................................................. 5_**
**Relational schema ................................................................................................................................................................... 6
SQL code: ................................................................................................................................................................................ 7
Inserting records: ................................................................................................................................................................... 11
database relational instance:.................................................................................................................................................. 13
MODIFYING: ........................................................................................................................................................................... 16
DELETE: .................................................................................................................................................................................. 16**


### Abstract :

#### Our application is a system that organizes Aircrafts maintenance projects.

#### It shows a clear pipeline of the maintenance process, as it covers the supply chain,

#### schedules, Hungers, projects, Employees requirements, departments and

#### customer needs.

#### This app aims to connect mechanics, project manager and hunger manager while

#### keeping them up to date with each other’s work. Furthermore, the system will

#### track the process since the craft checks in, and making daily updates on the

#### working hours, finished work and used products until the check out.

#### Why we chose it:

#### After looking into many possible ideas for the project, we started looking

#### into airports systems and decided to go with a MRO database system to be more

#### specified (as airports needs very large database).

#### In addition, we were able to meet a specialist on the subject who gave us enough

#### information about the system.

#### Team roles:

#### All tasks were worked equally, from generating entities to designing the EER

#### diagram and writing this document.


## Requirements

#### An Aircraft MRO have a (Name->unique, Location->complex (County, City and

#### address) , number of hungers). Each Aircraft MRO has many Hungers, every

#### hunger got (ID-> partial key), Capability, size, Manager ID-> foreign key), a

#### Hungers belongs to one MRO

#### Departments with (department manager name->unique, Number of employees)

#### can be several types (Supply chain, Store, Quality assurance, Management)

#### A Department belongs to one Aircraft MRO , but Aircraft MRO has many

#### Departments

#### Employee has (Employee_ID->unique, Name, Salary, Birth_date, Gender,

#### Department) , and can be several types (Hunger manager, Project manager,

#### Engineers, Mechanics, Supervisor), a Supervisor will supervise Mechanics

#### Hunger manager takes products from Store, which has many Products, each

#### product (Type->unique, cost, quantity, company_Name)

#### Supplier has (Company_Name->unique, Store_location), which supplies to the

#### Supply chain , and the Supply chain stores the products in Store

#### Customer has (Name->unique, Type, Number of aircrafts) which make deals with

#### the Management department, also Customers gives a Project that needs

#### maintenance

#### Project has (project ID -> unique, Aircraft type, start date, End date, Manager ID->

#### foreign key), each Project has many Mechanics and Engineers working on it


#### A Project manager who has (Manager ID->unique) will be assigned to a project by

#### Management department and each Project manager will be supervised by the

#### Hunger manager

#### Each Project manager manages a Project and plan a Schedule for every Project

#### A Schedule has (Check in, Duration, Check out->derived, Manpower, Maintenance

#### type, Meetings->composed (Date, Type, Duration))


# ER mode


### Relational schema


### SQL code:
sql```
###### CREATE TABLE AIRCRAFT_MRO(

name VARCHAR2(20) PRIMARY KEY,
numberOfHunger NUMBER(2),
country VARCHAR2(15),
city VARCHAR2(15),
address VARCHAR2(15));
CREATE TABLE HUNGAR(ID NUMBER(3),
NAME VARCHAR2(20) CONSTRAINT craftName REFERENCES AIRCRAFT_MRO(name),
MANAGER_ID NUMBER(3) CONSTRAINT manager_id REFERENCES HUNGAR_MANAGER(ID),
SIZE_meters NUMBER(3),
CAPABILITY NUMBER(1),
PRIMARY KEY (ID, NAME));
CREATE TABLE EMPLOYEE (employeeID NUMBER(3) PRIMARY KEY,
name VARCHAR2(20),
salary NUMBER(4),
birth_date DATE,
gender VARCHAR2(1),
departmentID NUMBER(1) CONSTRAINT dp_id REFERENCES DEPARTMENT(departmentID),
craft_name VARCHAR2(20) constraint cr_name REFERENCES Aircraft_MRO(name));
CREATE TABLE HUNGAR_MANAGER(
ID NUMBER(3) PRIMARY KEY,
departmentID NUMBER(1) CONSTRAINT storeid REFERENCES STORE(ID),
CONSTRAINT hungar_manager_ID FOREIGN KEY(ID) REFERENCES EMPLOYEE(employeeID));
CREATE TABLE ENGINEERS(
ID NUMBER(3) PRIMARY KEY,
CONSTRAINT engineers_ID FOREIGN KEY(ID) REFERENCES EMPLOYEE(employeeID));


###### CREATE TABLE MECHANICS(

###### ID NUMBER(3) PRIMARY KEY,

supervisorID NUMBER(3) CONSTRAINT supervisorID REFERENCES SUPERVISOR(ID),
CONSTRAINT mechanics_ID FOREIGN KEY(ID) REFERENCES EMPLOYEE(employeeID));
CREATE TABLE SUPERVISOR(
ID NUMBER(3) PRIMARY KEY,
CONSTRAINT supervisor_ID FOREIGN KEY(ID) REFERENCES EMPLOYEE(employeeID));
CREATE TABLE PROJECT_MANAGER(
ID NUMBER(3) PRIMARY KEY,
hungarManagerID NUMBER(3) CONSTRAINT hungerMID REFERENCES HUNGAR_MANAGER(ID),
departmentID NUMBER(1) CONSTRAINT managementid REFERENCES MANAGEMENT(ID),
CONSTRAINT project_manager_ID FOREIGN KEY(ID) REFERENCES EMPLOYEE(employeeID));
CREATE TABLE EN_WORKS(
engineerID NUMBER(3) CONSTRAINT engineer_id REFERENCES ENGINEERS(ID),
projectID NUMBER(3) CONSTRAINT proj_id REFERENCES PROJECT(projectID),
PRIMARY KEY (engineerID,projectID));
CREATE TABLE MECH_WORKS(
mechanicID NUMBER(3) CONSTRAINT mechanic_id REFERENCES MECHANICS(ID),
projectID NUMBER(3) CONSTRAINT pr_id REFERENCES PROJECT(projectID),
PRIMARY KEY(mechanicID,projectID));
CREATE TABLE DEPARTMENT(
departmentID NUMBER(1) PRIMARY KEY,
NumberOfEmployees NUMBER(2),
aircraftName VARCHAR2(20) CONSTRAINT craft_name REFERENCES AIRCRAFT_MRO(name));
CREATE TABLE SUPPLY_CHAIN(
ID NUMBER(1) PRIMARY KEY,
CONSTRAINT sup_ID FOREIGN KEY(ID) REFERENCES DEPARTMENT(departmentID));


###### CREATE TABLE STORE(

###### ID NUMBER(1) PRIMARY KEY,

CONSTRAINT st_ID FOREIGN KEY(ID) REFERENCES DEPARTMENT(departmentID));
CREATE TABLE QUALITY_ASSURENCE(
ID NUMBER(1) PRIMARY KEY,
CONSTRAINT qu_ID FOREIGN KEY(ID) REFERENCES DEPARTMENT(departmentID));
CREATE TABLE MANAGEMENT(
ID NUMBER(1) PRIMARY KEY,
CONSTRAINT ma_ID FOREIGN KEY(ID) REFERENCES DEPARTMENT(departmentID));
CREATE TABLE SUPPLIER(
Company_name VARCHAR2(20) PRIMARY KEY,
Store_locatoin VARCHAR2(20),
departmentID NUMBER(1) CONSTRAINT supply_id REFERENCES SUPPLY_CHAIN(ID));
CREATE TABLE CUSTOMER(
name VARCHAR2(20) PRIMARY KEY,
type VARCHAR2(20),
NumberOfAircraft NUMBER(3),
departmentID NUMBER(1) CONSTRAINT management_id REFERENCES MANAGEMENT(ID));
CREATE TABLE SCHEDULE(
S_number NUMBER(4) PRIMARY KEY,
Check_in DATE,
Duration_hours NUMBER(4),
Manpower NUMBER(3),
Maintenance_Type VARCHAR2(20),
Meeting_date DATE,
Meeting_type VARCHAR2(20),
Meeting_duration NUMBER(4),
ProjectManagerID NUMBER(3) CONSTRAINT pr_manager_id REFERENCES PROJECT_MANAGER(ID),
ProjectID NUMBER(2) CONSTRAINT project_id REFERENCES PROJECT(projectID));


###### CREATE TABLE PRODUCT(

type VARCHAR2(20) PRIMARY KEY,
cost NUMBER(3),
quantity NUMBER(3),
companyName VARCHAR2(20),
departmentID NUMBER(1) CONSTRAINT store_id REFERENCES STORE(ID));
CREATE TABLE PROJECT(
projectID NUMBER(3) PRIMARY KEY,
startdate DATE,
AIRCRAFTtype VARCHAR2(20),
projectmanagerid NUMBER(3) constraint managerid REFERENCES PROJECT_MANAGER(ID),
customerName VARCHAR2(20) constraint name REFERENCES CUSTOMER(name));
```

## Inserting records:

##### INSERT INTO AIRCRAFT_MRO VALUES('JoramCo',2,'Jordan','Amman','ST.17');

##### INSERT INTO AIRCRAFT_MRO VALUES('JBsteel',4,'America','Newyork','BT.10');

##### INSERT INTO AIRCRAFT_MRO VALUES('SAEI',5,'SaudiArabia','Riadh','ST.20');

##### INSERT INTO AIRCRAFT_MRO VALUES('OMDB',7,'Emirates','AbuDabi','ST.40');

##### INSERT INTO AIRCRAFT_MRO VALUES('MASCO',3,'Lebanon','Bairute','ST.17');

##### INSERT INTO HUNGAR VALUES(1,'MASCO',203,75,7);

##### INSERT INTO EMPLOYEE VALUES(201,'Sara',520,'15-oct-1980','F',5,'MASCO');

##### INSERT INTO EMPLOYEE VALUES(202,'Mohammad',800,'16-mar-1990','M',1,'MASCO');

##### INSERT INTO EMPLOYEE VALUES(203,'Ahmad',1000,'17-sep-1995','M',1,'MASCO');

##### INSERT INTO EMPLOYEE VALUES(204,'Noor',650,'6-jan-1999','F',3,'MASCO');

##### INSERT INTO EMPLOYEE VALUES(205,'Ali',700,'17-feb-1985','M',3,'MASCO');

##### INSERT INTO HUNGAR_MANAGER VALUES(203,4);

##### INSERT INTO PROJECT_MANAGER VALUES(202,203,1);

##### INSERT INTO ENGINEERS VALUES(205);

##### INSERT INTO MECHANICS VALUES(204,201);

##### INSERT INTO SUPERVISOR VALUES(201);

##### INSERT INTO EN_WORKS VALUES(205,1);

##### INSERT INTO MECH_WORKS VALUES(204,2);

##### INSERT INTO DEPARTMENT values(1,40,'MASCO');

##### INSERT INTO DEPARTMENT values(2,50,'MASCO');

##### INSERT INTO DEPARTMENT values(3,55,'MASCO');

##### INSERT INTO DEPARTMENT values(4,30,'MASCO');

##### INSERT INTO DEPARTMENT values(5,20,'MASCO');

##### INSERT INTO SUPPLY_CHAIN values(2);

##### INSERT INTO STORE values(4);

##### INSERT INTO STORE values(5);

##### INSERT INTO QUALITY_ASSURENCE values(3);

##### INSERT INTO MANAGEMENT values(1);

##### INSERT INTO SUPPLIER VALUES('ARC','AMMAN',2);

##### INSERT INTO SUPPLIER VALUES('ZIC','ZARQA',2);

##### INSERT INTO SUPPLIER VALUES('LALO','MAFRAQ',2);

##### INSERT INTO SUPPLIER VALUES('KITO','IRBID',2);

##### INSERT INTO SUPPLIER VALUES('ELMO','KARAK',2);


##### INSERT INTO CUSTOMER VALUES('Ahmad','indevedual',1,1);

##### INSERT INTO CUSTOMER VALUES('Jordan Airlines','company',15,1);

##### INSERT INTO CUSTOMER VALUES('hiba','indevedual',2,1);

##### INSERT INTO CUSTOMER VALUES('US Airlines','company',25,1);

##### INSERT INTO CUSTOMER VALUES('sam','indevedual',3,1);

##### INSERT INTO SCHEDULE VALUES(20,'17-jan-2022',500,50,'all','18-jan-2022','online',2,202,1);

##### INSERT INTO SCHEDULE VALUES(30,'18-mar-2022',900,15,'all','20-mar-2022','online',3,202,2);

##### INSERT INTO SCHEDULE VALUES(15,'29-jun-2022',50,30,'Tires','1-jun-2022','online',5,202,3);

##### INSERT INTO SCHEDULE VALUES(19,'25-jul-2022',240,18,'Winges','27-jul-2022','online',1,202,4);

##### INSERT INTO SCHEDULE VALUES(5,'19-sep-2022',144,10,'Engine','25-sep-2022','online',9,202,5);

##### INSERT INTO PRODUCT VALUES('Body',500,1,'ARC',5);

##### INSERT INTO PRODUCT VALUES('Engine',600,2,'ZIC',5);

##### INSERT INTO PRODUCT VALUES('Tire',300,3,'LALO',4);

##### INSERT INTO PRODUCT VALUES('Window',50,4,'KITO',4);

##### INSERT INTO PRODUCT VALUES('Winge',900,5,'ELMO',5);

##### INSERT INTO PROJECT VALUES(1,'17-jan-2022','Passenger',202,'Jordan Airlines');

##### INSERT INTO PROJECT VALUES(2,'18-mar-2022','Passenger',202,'US Airlines');

##### INSERT INTO PROJECT VALUES(3,'29-jun-2022','JET',202,'Ahmad');

##### INSERT INTO PROJECT VALUES(4,'25-jul-2022','JET',202,'sam');

##### INSERT INTO PROJECT VALUES(5,'19-sep-2022','JET',202,'hiba');


### database relational instance:




#### MODIFYING:

##### UPDATE AIRCRAFT_MRO SET address='ST.19' WHERE name='MASCO';

##### ALTER TABLE AIRCRAFT_MRO

##### RENAME COLUMN name TO AIRCRAFT_NAME;

#### DELETE:

##### DELETE FROM AIRCRAFT_MRO WHERE country='Jordan';

##### DELETE FROM PRODUCT WHERE cost<200;

#### Join:

##### SELECT customerName,Duration_hours

##### FROM PROJECT p,SCHEDULE s

##### WHERE p.projectID=s.projectID;

#### join and aggregate functions:

SELECT name,E.departmentID
FROM EMPLOYEE E,DEPARTMENT D
WHERE E.departmentID=D.departmentID AND salary> (SELECT AVG(salary) FROM EMPLOYEE);


