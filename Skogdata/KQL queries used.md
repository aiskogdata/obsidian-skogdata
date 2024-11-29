```KQL
ContainerLogV2 | extend PrettyPodName = strcat_array((array_slice(split(PodName,"-"),0,2)),"-")
| summarize count() by PrettyPodName
| order by count_
```

```KQL
ContainerLogV2 | where PodNamespace == "vsys-stg"
| extend PrettyPodName = strcat_array((array_slice(split(PodName,"-"),0,2)),"-")
| summarize count() by PrettyPodName
| order by count_
```

```KQL
ContainerLogV2 | summarize Count = count() by PodNamespace
```

antall requests mot handels order per dag
```KQL
requests |where (cloud_RoleName  contains "VH") and name != "GET Health/Live" and name contains "POST" and (name matches regex "*Sale*" and not(name matches regex "*PGet*" or name matches regex "*Get*") or name matches regex "*Purchase*" and not (name matches regex "PGet" or name matches regex "*Get*"))
| summarize Count=count() by Day = bin(timestamp,1d)
| order  by  Day desc 
```

antall requests mot handels order per min
```KQL
requests |where (cloud_RoleName  contains "VH") and name != "GET Health/Live" and name contains "POST" and (name matches regex "*Sale*" and not(name matches regex "*PGet*" or name matches regex "*Get*") or name matches regex "*Purchase*" and not (name matches regex "PGet" or name matches regex "*Get*"))
| summarize Count=count() by Day = bin(timestamp,1m)
| order  by  Day desc 
```

antall requests mot handels order per day per minutt
```KQL
requests |where (cloud_RoleName  contains "VH") and name != "GET Health/Live" and name contains "POST" and (name matches regex "*Sale*" and not(name matches regex "*PGet*" or name matches regex "*Get*") or name matches regex "*Purchase*" and not (name matches regex "PGet" or name matches regex "*Get*"))
| extend Day = format_datetime(timestamp, "yyyy-MM-dd"), Minute = format_datetime(timestamp, "HH:mm")
| summarize Count = count() by Day, Minute
| order by Day asc, Minute asc 
```

max antall requests mot handels order innen et minutt per dag
```KQL
requests |where (cloud_RoleName  contains "VH") and name != "GET Health/Live" and name contains "POST" and (name matches regex "*Sale*" and not(name matches regex "*PGet*" or name matches regex "*Get*") or name matches regex "*Purchase*" and not (name matches regex "PGet" or name matches regex "*Get*"))
| extend Day = format_datetime(timestamp, "yyyy-MM-dd"), Minute = format_datetime(timestamp, "HH:mm")
| summarize MinuteCount = count() by Day, Minute | summarize MaxMinuteCount = max(MinuteCount) by Day | order by Day asc
```


queriet er for stort og timer ut 
```kql
//customEvents
//|  where  name  contains "NegativeMessageResponse" and customDimensions contains "DeliverySource finnes ikke for SourceReferenceId"
//| where timestamp  between (datetime(2024-03-01 12:00:00) .. datetime(2024-06-01 12:00:00))

requests
| extend RoundedTimeStamp = bin(timestamp, 1s)
| join kind=inner (exceptions
    | extend RoundedTimeStamp = bin(timestamp, 1s)
    )
    on RoundedTimeStamp, cloud_RoleName
| join kind=inner (customEvents | extend  RoundedTimeStamp = bin(timestamp, 1s))   on RoundedTimeStamp, cloud_RoleName 
| where timestamp between (datetime(2024-05-01 12:00:00) .. datetime(2024-10-01 12:00:00))
| where not(name contains "GET Health/Live") and (cloud_RoleName contains "VH.Retro.Api" or cloud_RoleName contains "VH.Virkeshandel.Api" or cloud_RoleName contains "VH.Service")
| where customDimensions2 contains "DeliverySource finnes ikke for SourceReferenceId"
| project
    RoundedTimeStamp,
    name,
    resultCode,
    customDimensions,
    cloud_RoleInstance,
    problemId,
    type,
    assembly,
    method,
    outerType,
    outerMessage,
    outerMethod,
    details,
    customDimensions1,
    operation_Name1,
    cloud_RoleName1,
    customDimensions2
```


kusto query som eksluderer interne ip addresser aka se kun eksterne kall til tjenester

```
requests
//|project  url, client_IP, cloud_RoleName, name
| project  name, client_IP, url, customDimensions, cloud_RoleName
//eksluderer alt som har med interne ting som er 'eksternt' fra API aka service bus, og frontend ting
| where not(name contains "ServiceBus") and not(name contains ".") and not (url contains "/vh") and not (url contains "/vhx")
//eksluderer alle private ip ranges
//| where not (client_IP matches regex @"\b(10\.(?:\d{1,3}\.){2}\d{1,3}|172\.(?:1[6-9]|2[0-9]|3[0-1])\.(?:\d{1,3}\.)\d{1,3}|192\.168\.(?:\d{1,3}\.)\d{1,3})\b")
//eksluderer 10.x.x.x privat ip range som er det vi bruker i K8s
| where not (client_IP matches regex @"10\.(?:\d{1,3}\.){2}\d{1,3}")
| summarize by cloud_RoleName, name, client_IP

```

```
requests
| project  name, client_IP, url, customDimensions, cloud_RoleName
//eksluderer ting som henter filer
| where not(name contains ".")
//eksluderer 10.x.x.x privat ip range som er det vi bruker i K8s
| where not (client_IP matches regex @"10\.(?:\d{1,3}\.){2}\d{1,3}")
| where cloud_RoleName contains "Register"
| summarize occurences = count () by  name

```


i praksis brukt for å grave gjennom register requests, finne alle so ikke kommer fra private ip, eller kaller på filer, deretter prøver og delevis greier å fjerne query data og erstatte paremeter med :param (men funker bare på numeriske verdier og noen spesial tilfeller. trenger mer arbeid)
```shell
requests
| project  name, client_IP, url, customDimensions, cloud_RoleName
//eksluderer ting som henter filer
| where not(name contains ".")
//eksluderer 10.x.x.x privat ip range som er det vi bruker i K8s
| where not (client_IP matches regex @"10\.(?:\d{1,3}\.){2}\d{1,3}")
| where cloud_RoleName contains "Register"
| extend LastSlashIndex = strlen(url) - 1 - indexof(reverse(url), "/")
| extend QueryIndex = indexof(url, "?")
//| extend LastSlashIndex = strlen(url) - 1 - indexof(reverse(url), "/")
| extend QueryTrimmedUrl = iif(QueryIndex > 0, substring(url, 0, QueryIndex), url)
| extend Endpoint = replace_regex(QueryTrimmedUrl,@"(^|[^a-zA-Z]\B)\d+", ":param")  // Replaces all groups of digits with "param"
|extend  Endpoint = replace_regex(Endpoint, @"((\:param)[a-zA-Z]+|[a-zA-Z]+(\:param)|(\-(\:param))|(\w+\-(\:param)))", ":param")
| summarize occurences = count () by  Endpoint
```


```shell
requests
| project  name, client_IP, url, customDimensions, cloud_RoleName
//eksluderer ting som henter filer
| where not(name contains ".")
//eksluderer 10.x.x.x privat ip range som er det vi bruker i K8s
//| where not (client_IP matches regex @"10\.(?:\d{1,3}\.){2}\d{1,3}")
| where cloud_RoleName contains "Register"
| extend LastSlashIndex = strlen(url) - 1 - indexof(reverse(url), "/")
| extend QueryIndex = indexof(url, "?")
//| extend LastSlashIndex = strlen(url) - 1 - indexof(reverse(url), "/")
| extend QueryTrimmedUrl = iif(QueryIndex > 0, substring(url, 0, QueryIndex), url)
| extend Endpoint = replace_regex(QueryTrimmedUrl,@"(^|[^a-zA-Z]\B)\d+", ":param")  // Replaces all groups of digits with "param"
|extend  Endpoint = replace_regex(Endpoint, @"([\w\s\b\-\(\)\@\.\+\|]+)?:param([\w\s\b\-\(\)\@\.\+\|]+)?", ":param") //finds all places with param but not only param
|extend  Endpoint = replace_regex(Endpoint, @"(:param)+", ":param") //replaces all combos of repeating params with param
| summarize occurences = count () by  Endpoint
```


```shell
exceptions
| where problemId  contains "GetCodebook"
|project timestamp, cloud_RoleName, cloud_RoleInstance, problemId, outerMessage, details, customDimensions
| where timestamp  between (now(-3d) .. now()) //siste tre dagene
//| where timestamp  between (datetime(2024-11-01 08:00) .. now())
//| where timestamp  between (datetime(2024-11-01 08:00) .. datetime(2024-11-29 08:00))
| limit 2000 //max 2147483647
```