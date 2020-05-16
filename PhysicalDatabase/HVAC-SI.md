# HVAC Systems Information

## Physical Database


<hr>

[Physical Design](PhysicalDesign.md)


## Database Implementation (SQL Script)

### Drop And Create Database
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
