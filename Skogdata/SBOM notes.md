#### Gode grunner til å ha det:
- innsikt i pakkebruk kan gir større sikkerhet ved at vi kan fange opp feil pakke bruk for eks ved ha en automatisert prosess mot å skjekke name spotting
- Gi større innsikt i hvordan prosjektene våre henger sammen internt da den samler på project dependencies
- Gi oss bedre oversikt over docker images i bruk og evnt hvor vi kan optimalisere eller burde bytte ut


#### Rapport standard
- CycloneDX: OWASP utviklet standard godkjent av ECMA standardiserings org (aka en av de store standardisering komiteene)
	- [OWASP CycloneDX Software Bill of Materials (SBOM) Standard](https://cyclonedx.org/)
	- [CycloneDX v1.6 JSON Reference](https://cyclonedx.org/docs/1.6/json/)

- SPDX er ena annen standard [SPDX – Linux Foundation Projects Site](https://spdx.dev/)
	- Anerkjent av ISO 

#### Potensielle produkter
- [anchore/syft: CLI tool and library for generating a Software Bill of Materials from container images and filesystems](https://github.com/anchore/syft)
	- Best brukt med vulnrebility detectors som [anchore/grype: A vulnerability scanner for container images and filesystems](https://github.com/anchore/grype)
	- Can utgi i begge formater
-  En hel del repos for variende bruk [CycloneDX SBOM Standard](https://github.com/CycloneDX) med apache lisens
	- egne github actions 
		- [CycloneDX .NET Generate SBOM · Actions · GitHub Marketplace](https://github.com/marketplace/actions/cyclonedx-net-generate-sbom)
- [CycloneDX/cdxgen: Creates CycloneDX Bill of Materials (BOM) for your projects from source and container images. Supports many languages and package managers. Integrate in your CI/CD pipeline with automatic submission to Dependency Track server.](https://github.com/CycloneDX/cdxgen)
	- apche lisens cli, mye brukt
- [SPDX Tools – SPDX](https://spdx.dev/use/spdx-tools/) har lit men ikke like dekkende


[The Top 11 Open-Source SBOM Tools | Wiz](https://www.wiz.io/academy/top-open-source-sbom-tools)