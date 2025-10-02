Link: [Django SQL injection in HasKey(lhs, rhs) on Oracle · CVE-2024-53908 · GitHub Advisory Database](https://github.com/advisories/GHSA-m9g8-fxxm-xg86)
[Django SQL injection vulnerability · CVE-2024-42005 · GitHub Advisory Database](https://github.com/advisories/GHSA-pv4p-cwwg-4rph)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** Minimal
**Risiko ved utnyttelse:** Minimal
### Omstendigheter
bruk av Django pakke i python kan gi mulighere for SQL injection
### Faremomenter
Datalekasje og redigereing

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
Oppgrader python imaget og pakka

#CVE #NVMFiks 

