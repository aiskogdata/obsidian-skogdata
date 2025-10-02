### steg 0: 
appen kaller etter Discovery document som gir all url og sånn info ved  [authentication.utv.vsys.no/.well-known/openid-configuration](https://authentication.utv.vsys.no/.well-known/openid-configuration)

### Steg 1
en login GET request går til authorization_endpoint fra dokumentet (f.eks. https://authentication.utv.vsys.no/connect/authorize)
Den skal inneholde:
- `client_id (hvor finner jeg ut hvilken å bruke)
- `state` (hva er formatet til state, hvor kan det evnt generers)
- `redirect_uri` hvor appen skal motta login suksess/info
- `response_type` hva skal dette være?
- `response_mode` hva skal dette være?
- `scope` space split string over alle scope som har blitt godtatt ved innlogging
- `code_challenge` trenger gjennomgang av dette
- `code_challenge` ---""---
returnerer et 302 kall til redirect uri endepunktet
### steg 2
Ved `redirect_uri` mottar appen en ticket med:
 - `code`som brukes for token senere
 - `scope`som er tillatelse omfanger
 - `state`som er infoen generet av appen første steg, må sjekkes mot eget `state` for å bekrefte at det er riktig bruker
### steg 3

Appen må så gjøre et kall til https://authentication.utv.vsys.no/connect/token for å generere den faktiske token som skal brukes for tilgang. Endpointet trenger:
- `client_id` (hvor får jeg det?)
- `code` (fra steg 2)
- `grant_type` = authorization_code
- `redirect_uri` (hvor skal appen motta token)
- `code_verifier` trenger gjennomgang av dette
appen får tilbake et json object (i body?) med access_token og id_token som kan brukes varierende steder. 

### Steg 4
brukerdata kan så hentes fra GET  https://authentication.utv.vsys.no/connect/userinfo som gir tilbake claims for brukern
	GET med bare bearer token av access_token

appen kan så bruke alt dette i videre api call hvor nødvendig

for å logge ut kall /connect/endsession som gir en addresse for utloggingsflyt og 302 kode

#### spørsmål

Appen skal for øyeblikket kun har superbrukere fra internt i SD
	- hvilket scope burde jeg bruke for dette?
Hva er formatet på state og code challenge greiene?
hva er passord for testbrukerne?
hvordan skjekke om bruker er logget inn med riktig scope?
- validering av session mot  "https://authentication.utv.vsys.no/connect/checksession" men hva sendes med?


