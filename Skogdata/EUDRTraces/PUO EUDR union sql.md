```sql
USE [VH_MS_E]
SELECT TOP (1000) 
Head.PuOHeadId,
Head.Ordernum,
Head.PuOHeadStatusNum,
Head.OrderDate, 
Head.SellerName,
Head.SalesCodeDescription,
Head.BuyerName,
Head.ReportToEudrTraces,
EU.DdsStatus,
EU.EudrDueDiligenceStatementId,
EU.EUDRDueDiligenceStatementReferenceNumber,
EU.EUDRDueDiligenceStatementVerificationNumber,
EU.PuOHeadId
FROM Purchase.PuOHead AS Head
INNER JOIN  
EUDR AS EU
ON 
Head.PuOHeadId = EU.PuOHeadId
ORDER BY [Created] DESC
```