```sql
requests
| where timestamp between (now(-14d) .. now())
| where cloud_RoleName contains "VH"
| where not(name in~ ("GET Health/Live", "GET Health/Ready", "GET /metrics"))
| extend LogEntry = todynamic(customDimensions)
| where bag_keys(LogEntry) has_any ("User", "Username")
| extend Username = tostring(LogEntry.Username), Tenant = tostring(LogEntry.Tenant)
| summarize Tenants = make_set(Tenant), CountPerTenant = dcount(Tenant), LogEntries = make_list(LogEntry) by Username
| where CountPerTenant > 1

```