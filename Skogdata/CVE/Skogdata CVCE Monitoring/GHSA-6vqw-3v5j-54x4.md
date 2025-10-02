Link: [cryptography NULL pointer dereference with pkcs12.serialize_key_and_certificates when called with a non-matching certificate and private key and an hmac_hash override · CVE-2024-26130 · GitHub Advisory Database](https://github.com/advisories/GHSA-6vqw-3v5j-54x4)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** Minimal
**Risiko ved utnyttelse:** Minimal
### Omstendigheter
Ved bruk av Python sin pkcs12 pakke for lesing av serfikater kan det oppstå situationser som krasjer python prosessen
### Faremomenter
DoS

### Finnes i
- `skogdata.azurecr.io/nvm.dronewoodvolume:0.1.1`

### Kan fikses ved
Oppgrader python imaget
#CVE #NVMFiks 

