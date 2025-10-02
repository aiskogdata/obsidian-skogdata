![[Pasted image 20241101085533.png]]


max antall requests mot handels order innen et minutt per dag
```KQL
requests |where (cloud_RoleName  contains "VH") and name != "GET Health/Live" and name contains "POST" and (name matches regex "*Sale*" and not(name matches regex "*PGet*" or name matches regex "*Get*") or name matches regex "*Purchase*" and not (name matches regex "PGet" or name matches regex "*Get*"))
| extend Day = format_datetime(timestamp, "yyyy-MM-dd"), Minute = format_datetime(timestamp, "HH:mm")
| summarize MinuteCount = count() by Day, Minute | summarize MaxMinuteCount = max(MinuteCount) by Day | order by Day asc
```