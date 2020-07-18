# HVAC Systems Information

## Physical Database


<hr>

[Database Tables](PhysicalDatabaseTables.md)

<hr>

## Database Implementation

### Drop And Create Database
```sql
USE master;

IF DB_ID(N'HVACSI') IS NOT NULL DROP DATABASE HVACSI;

IF @@ERROR = 3702 
   RAISERROR(N'Database cannot be dropped because there are still open connections.', 127, 127) WITH NOWAIT, LOG;

CREATE DATABASE HVACSI;
GO

USE HVACSI;
GO

CREATE SCHEMA SI AUTHORIZATION dbo;
GO
```

### Users Table
```sql
CREATE TABLE SI.USERS
(
  userid       int IDENTITY(1, 1) NOT NULL,
  username     nvarchar(50)       NOT NULL,
  email        nvarchar(40)       NOT NULL,
  password     varchar(40)        NOT NULL,
  companyname  varchar(60)        NULL,
  CONSTRAINT PK_USERS PRIMARY KEY(userid),
  CONSTRAINT UC_email UNIQUE(email)
);
CREATE NONCLUSTERED INDEX idx_nc_email ON SI.USERS(email);
GO 
```

### Manufacturers Table
```sql
CREATE TABLE SI.Manufacturers
(
  companyid    int  IDENTITY(1, 1)NOT NULL,
  companyname  nvarchar(max)      NOT NULL,
  products     int                NOT NULL,
  CONSTRAINT PK_Manufacturer PRIMARY KEY(companyid)
);
GO
```

### Furnace Table
```sql
CREATE TABLE SI.Furnaces
(
  furnaceid     int  IDENTITY(1, 1)NOT NULL,
  brand         nvarchar(50)       NOT NULL,
  companyid     int                NOT NULL,
  modelnumber   varchar(50)        NOT NULL,
  serialnumber  varchar(50)        NOT NULL,
  tonnage       int                NOT NULL,
  size          int                NOT NULL,
  year          int                NOT NULL,
 CONSTRAINT PK_Furnaces PRIMARY KEY(furnaceid),
 CONSTRAINT FK_Furnaces_Manufacturers FOREIGN KEY(companyid)
 REFERENCES SI.Manufacturers(companyid)
);
GO
```

### Humidifiers Table
```sql
CREATE TABLE SI.Humidifiers
(
  humid         int  IDENTITY(1, 1)NOT NULL,
  brand         nvarchar(50)       NOT NULL,
  companyid     int                NOT NULL,
  modelnumber   varchar(50)        NOT NULL,
  serialnumber  varchar(50)        NOT NULL,
  size          int                NOT NULL,
  year          int               NOT NULL,
  CONSTRAINT PK_Humidifiers PRIMARY KEY(humid),
  CONSTRAINT FK_Humidifiers_Manufacturers FOREIGN KEY(companyid)
  REFERENCES SI.Manufacturers(companyid)
);
GO
```

### AC Table
```sql
CREATE TABLE SI.AC
(
  airconid		int  IDENTITY(1, 1)NOT NULL,
  brand			NVARCHAR(50)       NOT NULL,
  companyid     int                NOT NULL,
  modelnumber   VARCHAR(50)        NOT NULL,
  serialnumber  VARCHAR(50)        NOT NULL,
  tonnage       int                NOT NULL,
  size          int                NOT NULL,
  year          int               NOT NULL,
  CONSTRAINT PK_AC PRIMARY KEY(airconid),
  CONSTRAINT FK_AC_Manufacturers FOREIGN KEY(companyid)
  REFERENCES SI.Manufacturers(companyid)
);
GO
```
