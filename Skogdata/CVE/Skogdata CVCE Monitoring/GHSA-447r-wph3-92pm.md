Link: [Microsoft Security Advisory CVE-2024-38095 | .NET Denial of Service Vulnerability · CVE-2024-38095 · GitHub Advisory Database](https://github.com/advisories/ghsa-447r-wph3-92pm)

## Utnyttelse av sårbarhet

**Sannsynlighet for utnyttelse hos skogdata:** medium
**Risiko ved utnyttelse:** Medium
### Omstendigheter
Ved parsing av feilformaterte x.509 sertfikater kan det oppstå DoS omstendigheter
### Faremomenter
DoS by cpu consumption

### Finnes i
- `skogdata.azurecr.io/vsys.integration.viol2:1.0.0`

### Kan fikses ved
Oppgrader .NET 6.0 pakke og image til >6.0.31, oppgradere .NET 8.0 >8.0.6

#CVE #ForvaltningFiks 

