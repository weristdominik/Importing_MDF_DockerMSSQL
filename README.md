# Importing_MDF_DockerMSSQL

Importing MDF Files into MSSQL Server hosted on  Linux/MacOS Docker. 


1. Get MSSQL Docker Image and RUN:
```
docker run -e "ACCEPT_EULA=1" -e "MSSQL_SA_PASSWORD={password}" -e "MSSQL_PID=Developer" -e "MSSQL_USER=SA" -p 1433:1433 -d --name=sql mcr.microsoft.com/azure-sql-edge
```


2. Download Azure Data Studion & Install

https://azure.microsoft.com/de-de/products/data-studio

How to connect:

<img width="495" alt="Screenshot 2024-01-21 at 14 47 40" src="https://github.com/weristdominik/Importing_MDF_DockerMSSQL/assets/47948163/8c89f6bf-3250-4ae2-9b9e-b63494353102">







3. Copy .mdf & .ldf File into Docker Container
```
docker cp XXX/database1.mdf CONTAINERID:/var/opt/mssql/data
docker cp XXX/database1_log.ldf CONTAINERID:/var/opt/mssql/data
```

4. Change Permission of .mdf & .ldf File(s)
```
docker exec -it -u root sql bash
chown mssql:root /var/opt/mssql/data/database1.mdf
chown mssql:root /var/opt/mssql/data/database1_log.ldf
```
   
5. Import via Azure Data Studio - SQL Query
```
CREATE DATABASE myNewDatabase
    ON (FILENAME = '/var/opt/mssql/data/database1.mdf'),   
    (FILENAME = '/var/opt/mssql/data/database1_log.ldf')   
    FOR ATTACH;
```

6. Now You can enter Azure Data Studio and refresh your Server -> You will than see your 'myNewDatabase' Database.

