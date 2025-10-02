Link: [Python Cryptography package vulnerable to Bleichenbacher timing oracle attack · CVE-2023-50782 · GitHub Advisory Database](https://github.com/advisories/GHSA-3ww4-gg4f-jr7f)
[cryptography mishandles SSH certificates · CVE-2023-38325 · GitHub Advisory Database](https://github.com/advisories/GHSA-cf7p-gm2m-833m)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** minimal
**Risiko ved utnyttelse:** høy
### Omstendigheter
Ved bruk av python-cryptography pakka kan en remote attacker potensielt få dekryptert meldinger pw TLS servere ved bruk av RSA nøkkelutveksling. 
### Faremomenter
Eksponerer sensitive data og potensielt mer. 

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
Oppgrader python image og python pakka

#CVE #NVMFiks 

