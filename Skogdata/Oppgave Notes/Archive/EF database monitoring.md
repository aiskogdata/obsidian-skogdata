
- Enkleste er nok å bruke Query Performance Insight i Azure [Query Performance Insight - Azure SQL Database | Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-sql/database/query-performance-insight-use?view=azuresql)
- Eventuelt kan Sql Server Auditing skrues på og man kan analysere via Application Insight [Set up Auditing - Azure SQL Database & Azure Synapse Analytics | Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-sql/database/auditing-setup?view=azuresql)

- Obs: dette generere en del data og burde bli puttet i en egen storage account med betydelig kortere levetid en det vi har for vanlige logger også kobler vi storage accounten til log analytics

- Eventuelt, om vi bare bryr oss om tid kan vi bruke [DbCommand Interceptor](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.dbcommandinterceptor?view=efcore-9.0) og logge resultatet så det blir med til Application Insight

[Performance Diagnosis - EF Core | Microsoft Learn](https://learn.microsoft.com/en-us/ef/core/performance/performance-diagnosis?tabs=simple-logging%2Cload-entities)
^^^^ denne er nok best, lage et eget lite test prosjekt som lagger queries og analyserer outputet