Link: [Microsoft Security Advisory CVE-2025-30399 | .NET Remote Code Vulnerability · CVE-2025-30399 · GitHub Advisory Database](https://github.com/advisories/GHSA-266m-wp2v-x7mq)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** --
**Risiko ved utnyttelse:** --
### Omstendigheter
Om en angriper greier og plassere filer i spesifikke lokasjoner kan du ved bruk av annen sårbarhet kjøre kode fra disse filene uten tillatelse.
### Faremomenter
Store muligheter for DoS og datalekasje

### Finnes i
- `skogdata.azurecr.io/vsys.progress.messaging:1.4.0`
-  `skogdata.azurecr.io/vsys.integration.viol3.workers.toviol3responsemonitoring:1.3.8`
- `skogdata.azurecr.io/vsys.integration.viol3.workers.toviol3dlq:1.3.8` 
- `skogdata.azurecr.io/vsys.integration.viol3.workers.toviol3:1.3.8`
- `skogdata.azurecr.io/vsys.integration.viol3.workers.fromviol3:1.3.8`
- `skogdata.azurecr.io/vsys.integration.viol2:1.0.0`
- `skogdata.azurecr.io/vsys.authentication.transportuserreport:2.24.1`

### Kan fikses ved
Oppgrader .NET 8 pakker/images til >8.0.16 og .NET 9 til  >9.0.5 eller .NET 10

#CVE #ForvaltningFiks 

