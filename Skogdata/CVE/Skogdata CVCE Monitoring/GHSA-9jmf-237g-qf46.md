Link: [Django Path Traversal vulnerability · CVE-2024-39330 · GitHub Advisory Database](https://github.com/advisories/GHSA-9jmf-237g-qf46)
## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** Minimal
**Risiko ved utnyttelse:** Minimal
### Omstendigheter
Bruk ac PYthon sin Djano pakke kan tillate fillagring uten validering av pathen 
### Faremomenter
Farlige filer blir lagret og kan brukes via andre sårbarheter

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
oppgradering av python image og python pakke
#CVE #NVMFiks 

