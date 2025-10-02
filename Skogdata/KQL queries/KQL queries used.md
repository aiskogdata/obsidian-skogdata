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
