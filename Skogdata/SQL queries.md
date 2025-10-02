### skjekke mengde GB i databasen 

#### "Vanlig"
```sql
USE [SD_Register_E]
SELECT 
    SUM(size * 8.0 / 1024 / 1024) AS TotalSizeGB
FROM sys.master_files;
```
#### File share eller mounted (aka skogdata)
```sql
USE [SD_Register_E]
SELECT 
    name AS FileName,
    type_desc AS FileType,
    size * 8.0 / 1024 / 1024 AS SizeGB
FROM sys.database_files;
```


# footer