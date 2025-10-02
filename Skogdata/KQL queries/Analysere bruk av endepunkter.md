
### Final
```sql
requests  
| project  name, client_IP, url, customDimensions, cloud_RoleName, timestamp
| where timestamp  between (datetime(2024-11-01 08:00) .. now())
//| where timestamp  between (datetime(2024-11-01 08:00) .. datetime(2024-11-29 08:00))
//eksluderer ting som henter filer  
| where not(name contains ".")  
//eksluderer 10.x.x.x privat ip range som er det vi bruker i K8s  
| where not (client_IP matches regex @"10\.(?:\d{1,3}\.){2}\d{1,3}")  
| where cloud_RoleName contains "Register"  
| extend LastSlashIndex = strlen(url) - 1 - indexof(reverse(url), "/")  
| extend QueryIndex = indexof(url, "?")  
| extend LastSlashIndex = strlen(url) - 1 - indexof(reverse(url), "/")  
| extend QueryTrimmedUrl = iif(QueryIndex > 0, substring(url, 0, QueryIndex), url)  
| extend Endpoint = replace_regex(QueryTrimmedUrl,@"(^|[^a-zA-Z]\B)\d+", ":param")  // Replaces all groups of digits with "param"  
| extend  Endpoint = replace_regex(Endpoint, @"([\w\s\b\-\(\)\@\.\+\|]+)?:param([\w\s\b\-\(\)\@\.\+\|]+)?", ":param") //finds all places with param but not only param  
| extend  Endpoint = replace_regex(Endpoint, @"(undefined)+", ":param") //replaces all places where parameter is not defined with param  
| extend  Endpoint = replace_regex(Endpoint, "(/[^/]+/Get[^/]+/)[^/]+$", "\\1:param") //where 'Get' is part of the second to last part of the path, replace last part with param  
| extend  Endpoint = replace_regex(Endpoint, @"(:param)+", ":PARAM") //replaces all combos of repeating params with param  
| summarize occurences = count () by  Endpoint
| limit 1000 //max 2147483647
```


### Attempts

#### 1

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

#### 2
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

#### 3
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


