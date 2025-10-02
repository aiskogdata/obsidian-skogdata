Link: [sqlparse parsing heavily nested list leads to Denial of Service · CVE-2024-4340 · GitHub Advisory Database](https://github.com/advisories/GHSA-2m57-hf25-phgg)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** Minimal
**Risiko ved utnyttelse:** Medium
### Omstendigheter
Ved bruk av python pakka sqlparse, kan utførelse av sql queries med stor dybde (100 eller mer) skape RecursionError som kan utnyttes for og utføre DoS angrep om man parser sql queries fra bruker input. 
### Faremomenter
DoS

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
oppgradering av python image og python pakke

#CVE #NVMFiks

