- Lagt til feltet EoriNum i Party (Eu spesifikt org nummer)
- Lagt til LatinClassification/Latinsk Navn på TreespeciesGrp2
-  Lagt til tabellen EUHarmonizedProductCode og lagt til et nytt felt i Assortment med FK kobling til tabellen 
-  Lagt til tabellen EuTracesAuthenticationData, med FK referanse til Party via OrgNum
- GUI for å fylle ut EoriNum og feltene WebServiceId og WebServiceKey i EuTracesAuthenticationData , Latinsk Navn, velge riktig EuHarmonizedProductCode i Assortment, og for å lage nye EuHarmonizedProductCode
-  Lagt til feltene EUHarmonizedSystemCodeNum og EUHarmonizedSystemCodeNum_Description for GetAssortmentWithAllProperties("/api/assortment/{assortmentNum}/WithAllProperties") og GetAssortmentWithAllPropertiesList ("/api/assortment/WithAllProperties")
- Lagt til endepunktet "/api/party/GetParty/ByOrgNum/{orgNum}" for å hente aktører via org nummer i intern api
- lagt til GET og POST for endepunktet "/api/party/{organizationNumber}/eutraces/apidata" for å hente og large til EuTracesAuthenticationData
-  Eksporterer EoriNum til datalake via Aktør.sql
-  Eksporterer EUHarmonizedSystemCodeNum og LatinClassification via sortiment.sql til Datalake.

Det har også kommet inn nytt endepunkt for Party under  "/api/party/GetParty/ByOrgNum/{orgNum}" for å hente aktører via org nummer

PBIer
1. [Som Skogdata-admin kan jeg vedlikeholde HS-kode for hvert sortiment](https://skogdata.visualstudio.com/SkogData_workitems/edit/22817)
2. [Som Skogdata-admin kan jeg vedlikeholde det latinske treslaget for hvert enkelt treslag2 i sortimentsregisteret](https://skogdata.visualstudio.com/SkogData/_backlogs/backlog/Prosjekt%20-%20EU%20Deforestation%20Regulation(EUDR)/Backlog%20items/?workitem=22818)
3. [Som kunde ønsker jeg HS-Kode, det latinske treslaget og EORI-nummer tilgjengelig i DataLake](https://skogdata.visualstudio.com/SkogData/_backlogs/backlog/Prosjekt%20-%20EU%20Deforestation%20Regulation(EUDR)/Backlog%20items/?workitem=22819)
4. [Som kunde ønsker jeg HS-Kode, det latinske treslaget og EORI-nummer tilgjengelig i DataLake](https://skogdata.visualstudio.com/SkogData/_backlogs/backlog/Prosjekt%20-%20EU%20Deforestation%20Regulation(EUDR)/Backlog%20items/?workitem=22819)
5. [Som kunde ønsker jeg å legge inn mitt Eori-nummer i Register](https://skogdata.visualstudio.com/SkogData/_backlogs/backlog/Prosjekt%20-%20EU%20Deforestation%20Regulation(EUDR)/Backlog%20items/?workitem=22820)
6. [Som kunde ønsker jeg å legge inn min API-autentiseringsnøkkel i Register](https://skogdata.visualstudio.com/SkogData/_backlogs/backlog/Prosjekt%20-%20EU%20Deforestation%20Regulation(EUDR)/Backlog%20items/?workitem=22821)


https://skogdata.visualstudio.com/SkogData/_workitems/edit/22817
https://skogdata.visualstudio.com/SkogData/_workitems/edit/22818
https://skogdata.visualstudio.com/SkogData/_workitems/edit/22819
https://skogdata.visualstudio.com/SkogData/_workitems/edit/22819
https://skogdata.visualstudio.com/SkogData/_workitems/edit/22820
https://skogdata.visualstudio.com/SkogData/_workitems/edit/22821