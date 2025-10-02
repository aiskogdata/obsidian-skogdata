```sql
exceptions
| where timestamp >= make_datetime(2025,09,02)
| where type == "TypeError" and method == "vertexInput" 
| project timestamp, operation_Id, client_Type, client_Browser, client_OS, client_IP, application_Version
//henter ut assosiert request og henter ut Username om det eksisterer
| join kind=inner (
    requests
    | project operation_Id, url, Username = tostring(customDimensions["Username"])
) on operation_Id
| summarize errors = make_list(pack(
    "User", Username,
    "timestamp", timestamp,
    "client_type", client_Type,
    "client_OS", client_OS,
    "client_IP", client_IP,
    "application_Version", application_Version,
    "client_Browser", client_Browser,
    "url", url
))
by operation_Id
| mv-expand split_error = array_split(errors, 1)
| extend element = split_error[0] 
| project-away errors, split_error
| evaluate bag_unpack(element)
```