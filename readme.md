# PostgreSQL-11
### With oracle_fdw & tds_fdw (sybase & ms-sql) & pl/python 3 (plpython3u language)
**by Anton Dziavitsyn 2019**  
[a.dziavitsyn@gmail.com](mailto:a.dziavitsyn@gmail.com)
  
## How to run
You can use docker-compose.yml to build postgres+adminier for tests:
```bash
docker-compose -f pgsql-adminer.yml up
```  
  
## Using tds_fdw for MS-SQL schema access (mapping)
[Original documentation](https://github.com/tds-fdw/tds_fdw "tds_fdw GitHub repository")  
  
Activate extension:
```SQL
CREATE EXTENSION tds_fdw;
```
Connect server:
```SQL
CREATE SERVER mssql_svr
	FOREIGN DATA WRAPPER tds_fdw
	OPTIONS (servername '127.0.0.1', port '1433', database 'tds_fdw_test', tds_version '7.1');
```
Grant user access:
```SQL
GRANT USAGE ON FOREIGN SERVER mssql_svr TO postgres_username;
```
Create user mapping:
```SQL
CREATE USER MAPPING FOR postgres_username
	SERVER mssql_svr 
	OPTIONS (username 'sa', password '');
```
Import MS-SQL schema:
```SQL
IMPORT FOREIGN SCHEMA dbo
	FROM SERVER mssql_svr
	INTO public
	OPTIONS (import_default 'true');
```

## Using oracle_fdw for Oracle schema access (mapping)
[Original documentation](https://github.com/laurenz/oracle_fdw "oracle_fdw GitHub repository")  
  
Activate extension:
```SQL
CREATE EXTENSION oracle_fdw;
```
Connect server:
```SQL
CREATE SERVER oradb FOREIGN DATA WRAPPER oracle_fdw
          OPTIONS (dbserver '//dbserver.mydomain.com:1521/ORADB');
```
Grant user access:
```SQL
GRANT USAGE ON FOREIGN SERVER oradb TO postgres_username;
```
Create user mapping:
```SQL
CREATE USER MAPPING FOR postgres_username
          SERVER oradb
          OPTIONS (user 'orauser', password 'orapwd');
```
Import ORACLE schema:
```SQL
IMPORT FOREIGN SCHEMA ORA_SCHEMA
	FROM SERVER oradb
	INTO public;
```
