Link: [jwt-go allows excessive memory allocation during header parsing · CVE-2025-30204 · GitHub Advisory Database](https://github.com/advisories/GHSA-mh63-6h87-95cp)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** Middels
**Risiko ved utnyttelse:** Høy
### Omstendigheter
Angriper kan gi modulen en JWT Bearer token med mange punktum som kan forårsake stort minnebruk
### Faremomenter
DoS

### Finnes i
	- `mcr.microsoft.com/oss/kubernetes/azure-cloud-node-manager:v1.29.15`

### Kan fikses ved
oppgradere imaget

#CVE 
#DevOpsFiks 

