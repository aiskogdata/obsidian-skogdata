```sql
exceptions
| where problemId  contains "GetCodebook"
|project timestamp, cloud_RoleName, cloud_RoleInstance, problemId, outerMessage, details, customDimensions
| where timestamp  between (now(-3d) .. now()) //siste tre dagene
//| where timestamp  between (datetime(2024-11-01 08:00) .. now())
//| where timestamp  between (datetime(2024-11-01 08:00) .. datetime(2024-11-29 08:00))
| limit 2000 //max 2147483647
```

```sql
ContainerLog
| join kind=inner(ContainerInventory) on ContainerID
| where TimeGenerated between (datetime(2024-11-20 06:03) .. datetime(2024-11-29 22:04:12))
| where _ResourceId contains "sd-k8s-prod"
| project TimeGenerated, TimeOfCommand, LogEntry, Image, ContainerID, Name, Ports
| where not (Image contains "vsys.authentication.web" or Image  contains "im" or Image  contains "tr" or Image contains "progress")
| where not (LogEntry contains "api/health") and LogEntry contains "RegisterSrv" and LogEntry contains "400" and not(LogEntry contains "200") and LogEntry  contains "codebook"

//| limit 1000
```