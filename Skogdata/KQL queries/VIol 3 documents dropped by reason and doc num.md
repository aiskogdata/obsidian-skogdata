```sql
AppEvents 
| where TimeGenerated  between (datetime(2024-08-01 01:00) .. now())
| where _ResourceId contains "sdinsightapp-prod"
| where Name startswith "IncomingMessageDropped"
| extend messageid = tostring(Properties.MessageId), ReplyTo=tostring(Properties.ReplyTo), To=tostring(Properties.To)
| where To startswith "biometria.viol3" and ReplyTo startswith "vsys.progress"
| distinct DocumentNumber=tostring(Properties.DocumentNumber), Reason=tostring(Properties.Reason), RepliedToOrg=tostring(Properties.ReplyToOrganisationNumber)

```