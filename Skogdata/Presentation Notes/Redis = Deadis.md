### Slide 2

20 mars byttet Redis lisens for alle versjoner 7.4+ fra open source [BSD](https://redis.io/blog/what-redis-license-change-means-for-our-managed-service-providers/) [til dual-license Source available.](https://redis.io/blog/what-redis-license-change-means-for-our-managed-service-providers/) Sort på hvitt sier lisensene at alle som tilbyr Redis som en tjeneste må betale, altså alle de store ​skytjenestene andre som direkte konkurerer med Redis Stack.    
- sosiale medier presenterte ting som noe verre enn det egenetlig er som om "alle" skulle betale, pga noe vage ordlyder i lisensen. 
- For skogdata ser ikke lisensen ut til å bety mye (vi trenger ikke betale), men det kan fort ha noe å si for plugins og støtte verktøy vi bruker, slik som Redis Commander, om ikke ennå.  ​


    
 Mange Open Source contributers sluttet i protest over de neste dagene, blant annet mange av de størte contributerne, bla. "alle" som jobber hos AWS, Alibaba, Tencent​
- Viktigste som trakk seg ​  
- [https://github.com/madolson](https://github.com/madolson) (aws)​
- [https://github.com/enjoy-binbin](https://github.com/enjoy-binbin) (tencent)​
- [https://github.com/soloestoy](https://github.com/soloestoy) (alibaba)​
- [https://github.com/hwware](https://github.com/hwware) (huwei)​
- [https://github.com/mattsta](https://github.com/mattsta)


- 28 mars annonserte Linux Foundation [ValKey prosjektet](https://www.linuxfoundation.org/press/linux-foundation-launches-open-source-valkey-community) som er en direkte fork av Redis 7.2.4 (siste versjon under BSD) ​
	- har  støtte fra  Amazon Web Services (AWS), Google Cloud, Oracle, Ericsson, and Snap Inc

    
Madelyn Olson fra AWS var med på å starte linux alternativet.​
### Slide 3
Ingenting i terms of service påvirker Skogdata direkte `#ikkeAdvokat`, men mange av de store aktørene som støttet opp rundt redis har flyttet til ValKey prosjektet innkludert AWS, Google, SnapChat i en offisiell kapasitet samt Tencent, Alibaba og Huwei i en uoffisiel kapasitet. Microsoft støtter fortsatt offisielt Redis.

Google, AWS, og Oracle har offisielt lagt sin vekt bak en open source redis versjon, og Alibaba sine utviklere har i praksis trulkket seg fra Redis prosjektet. selv om det kan være PR stunt så er det i hvert fall ganske sannsynlig tror jeg at de alle ønsker å  løpe fra regninga og avvikle bruk av Redis. ElasticCache og MemoryStore kommer nok til å implementere ValKey isteden for Redis i fremtiden. Og Microsoft kommer nok ikke til å la seg falle etter i det racet. 


### Slide 4
Som sagt er ValKey en ny redis fork fra Linux med stor støtte og personlig tror jeg den er det beste alternativet om vi ønsker å bytte. Akuratt nå er det drop-in replacement for redis, og alt som har fungert for oss på redis burde fungere der.

De to andre som kan være verdt å vurdere er Garnet og KeyDB.

Garnet er Microsoft eid, og lagd i C#. Gitt vårt økosystem sw er det ikke nødvendigvis et dumt bytte. Skal også påstålig var drop-in replacement, men er skrevet i C# og driver av Microsoft Research, så ikke høyeste støtte faktoren. 

KeyDB er definitivt den forken av Redis som er mest moden og har vært gående siden 2019, og ble acquired av Snap i 2021. Dens hoved mål var 2 delt. multithreading mulighet, og gjøre ting enklere for utviklere via mer funksjonalitet. Den er også påstålig drop-in replacement, men er nok litt mer vrien da den har vært forka lengre. 

### Slide 5
ValKey er den nyeste forken som er fortsatt open source og som sagt har masse støtte. Den er for øyeblikket så en-til-en med Redis at den ikke har en egen Nuget pakke ennå og man må bruke StackExchange versjonen enn så lenge. 







​

​

​

​​