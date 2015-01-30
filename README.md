# Zabbix template for Microsoft SQL Server monitoring via perfmon, services and ODBC.

This template monitors SQL Server databases status, jobs status, performance,  etc.

Before install template, create in Zabbix Value Mapping:

Name of mapping:
MS SQL Server database state

|Value    |Mapped to
|---------|----------
|0        |ONLINE
|1        |RESTORING
|2        |RECOVERING
|3        |RECOVERY PENDING
|4        |SUSPECT
|5        |EMERGENCY
|6        |OFFLINE
|7        |Database Does Not Exist on Server

To use ODBC, install unixODBC and freeTDS .
Sample config ODBC:
/etc/odbcinst.ini
```shell
[FreeTDS]
Description = FreeTDS Driver v0.91
Driver = /usr/lib/i386-linux-gnu/odbc/libtdsodbc.so
Setup = /usr/lib/i386-linux-gnu/odbc/libtdsS.so
UsageCount = 1
```

/etc/odbc.ini
```shell
[1c_base]
Driver = FreeTDS
ServerName = 1c_base
```

And configure SQL Server to access data for ODBC user [zab]:
```sql
CREATE LOGIN [zab] WITH PASSWORD=N'****', DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[us_english], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF

GRANT VIEW SERVER STATE to [zab]
GRANT VIEW ANY DEFINITION TO [zab]

USE master;
GRANT SELECT TO [zab];

USE msdb;
GRANT SELECT TO [zab];
```
