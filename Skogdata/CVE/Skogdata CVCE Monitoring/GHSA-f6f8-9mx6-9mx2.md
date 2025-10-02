Link: [Django vulnerable to Denial of Service · CVE-2024-39614 · GitHub Advisory Database](https://github.com/advisories/GHSA-f6f8-9mx6-9mx2)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** Minimal
**Risiko ved utnyttelse:** Medium
### Omstendigheter
Bruk av python sin Django pakke og `get_supported_language_variant()`  skaper muligheten for DoS ved lange spesifikke stringer
### Faremomenter
DoS

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
oppgradering av python image og python pakke

#CVE #NVMFiks 

