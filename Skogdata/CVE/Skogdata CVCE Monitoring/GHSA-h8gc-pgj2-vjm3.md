Link: [Django Denial-of-service in django.utils.text.Truncator · CVE-2023-43665 · GitHub Advisory Database](https://github.com/advisories/GHSA-h8gc-pgj2-vjm3)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** Minimal
**Risiko ved utnyttelse:** Minimal
### Omstendigheter
Bruk av Python sin Django pakke med utils.text.Truncator og spesifikke lange html tekster kan forårsake DoS
### Faremomenter
DoS

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
Oppgradere python imaget og python pakka

#CVE #NVMFiks 

