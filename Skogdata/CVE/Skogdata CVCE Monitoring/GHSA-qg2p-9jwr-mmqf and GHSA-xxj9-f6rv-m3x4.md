Link: [GHSA-qg2p-9jwr-mmqf - OSV](https://osv.dev/vulnerability/GHSA-qg2p-9jwr-mmqf)
[CVE-2024-24680: Django denial-of-service attack in the intcomma template filter | Miggo](https://www.miggo.io/vulnerability-database/cve/CVE-2024-24680)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** minimal
**Risiko ved utnyttelse:** Høy
### Omstendigheter
Ved bruk av python Django pakka og irlze, urliztrnc kan DoS tilstander oppstå. Samme med intcomma
### Faremomenter
DoS

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
Oppgrader python imaget og python pakka

#CVE #NVMFiks 

