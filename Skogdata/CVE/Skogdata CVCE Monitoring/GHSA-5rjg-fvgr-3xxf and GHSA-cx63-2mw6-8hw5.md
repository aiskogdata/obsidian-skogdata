Link: [setuptools has a path traversal vulnerability in PackageIndex.download that leads to Arbitrary File Write · CVE-2025-47273 · GitHub Advisory Database](https://github.com/advisories/GHSA-5rjg-fvgr-3xxf)
[setuptools vulnerable to Command Injection via package URL · CVE-2024-6345 · GitHub Advisory Database](https://github.com/advisories/GHSA-cx63-2mw6-8hw5)
## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** Minimal
**Risiko ved utnyttelse:** høy
### Omstendigheter
Ved bruk av pakker i python kan malicious pakker utføre Arbritrary File Writes
### Faremomenter
Nedlastning av farlige filer og muligheter for Remote Code Execution

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
Oppgrader python imaget

#CVE #NVMFiks 