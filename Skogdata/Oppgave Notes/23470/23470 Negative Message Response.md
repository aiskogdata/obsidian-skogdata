


### Clean slate
Basert på det harald viste så er det så at når to VH clients sender volumReadyToShip meldinger frem og tilbake, er det slik at avsender først sender en Blanket message som innholder en del oppsett men ikke faktisk data, også kommer det en ordre melding etterpå som utfører transaksjonene. 
Feilen per nå ser ut til å være forårsaket av at 

volumeReadyToShip.SourceReferenceId


### kusto queries
```kql
customEvents
|  where  name  contains "NegativeMessageResponse" and customDimensions contains "DeliverySource finnes ikke for SourceReferenceId"
| where timestamp  between (datetime(2024-01-01 00:01:00) .. now())
```

```kql
customEvents
|  where operation_ParentId == "{en eller annen parent id fra forrige gir hele meld kjeden}"
| where timestamp  between (datetime(2024-01-01 12:00:00) .. now())
| order by timestamp desc 
```

```kql
exceptions
| where timestamp  between (datetime(2024-10-08 12:00:00) .. datetime(2024-10-08 20:00:00))
| order by timestamp desc 
```
^^^finne tilsvarnde exception
```sql
SELECT TOP (1000) *
  FROM [VH_ATS_E].[dbo].[DeliverySource]
  WHERE DeliverySourceId = 'delivery source id fra custom dimension fra tidliger query'
```
^^^ finnes ikke i sql

```kql
exceptions
| where not(cloud_RoleName == "Vsys.Im.CustomerPortal.Messaging.Receiver") 
| where timestamp  between (datetime(2024-10-08 16:00:00) .. datetime(2024-10-08 19:00:00))
| order by timestamp desc 
```
^^^så etter exceptions, fant ingen
	

### Handlings forløp
Feilen er alltid relatert til at en melding sent via Azure Service Bus, og ser ut til å i hovedsak skje når en VH kunde sender melding til seg selv. 

Fra det harald har funnets er det ut som VH klienten sender en Blanket Message som gir 'kontrakt' setupen men at meldingen som kommer etterpå med innfyllingsdata ikke blir sendt riktig. Det er denne meldingen som vanligvis inneholder delivery source
Det er slik at DeliverySource i dette tilfellet kommer fra en annen VH klient sin database og må replikeres i mottagende database, men når referenase dataen ikke har delivery source så blir det da tomt i mottagende data base.

#### Handlingsforløp i kode fra feilmeldinger

`ServiceApi.Customizable.RecieveMessage` får `InventoryStatus` som switch filter => 
`ServiceApi.Customizable.RecieveInventoryStatus` gir feil `"InventStatusNotValid", "Kunne ikke lagre framkjørtvolum fra transportklartmelding."` => generet fra at `volumeReadyToShip = shipmentRepo.SaveVolumeReadyToShip(inventoryStatusDTO);` gir null => som er generet fra 
```cs
```        var deliverySource =
            _vhContext.DeliverySource
            .Include(x => x.Shipment.PuODeliverySchedule.PuOLine)
            .Include(x => x.Shipment.SODeliverySchedule.SOLine)
            .FirstOrDefault
            (ds2 => ds2.SourceReferenceId == volumeReadyToShip.SourceReferenceId
                && ds2.Shipment.PuODeliverySchedule.PuOLine.PuOHeadId == volumeReadyToShip.PurchaseOrderId
                && !ds2.Inactive);
                ```
som gir null og feilen `"DeliverySource", $"DeliverySource finnes ikke for SourceReferenceId {volumeReadyToShip.SourceReferenceId} PuOHeadId {volumeReadyToShip.PurchaseOrderId}"`


Ser ut til å kun skje ved intern kommunikasjon hos kunde, ~~men meldingen kommer fra TR og ~~~ ser ut at DeliverySourceID som kommer derfra er ikke å finne på tidspunktet.

Cato sier at TR ikke sender noe InventoryStatus meld til VH så feilen er ved VH til VH klient hos kunde

Generlt har flyten nå vært at jeg fant puohead og source reference id i `custom events `ved søk og brukte dtte til å finne mer meldinger i `traces` ved å søke i message etter source reference id. Dette ga meg en Business acknowledgement som var svar for InventoryStatus med dok nummer 339293 fra vsys vh

potensielt noe rundt VolumeDeliveredToRoadGroup da `GET ExternalVolumeDeliveredToRoad/GetVolumeDeliveredToRoadGroup [orderId/supplypointId]` ble kalt rundt dette tidspunket
samme med `GET ExternalVolumeDeliveredToRoad/ValidShipmentStatusExist [volumeDeliveredToRoadGroupId]`
evnt `POST ExternalVolumeDeliveredToRoad/SaveVolumeDeliveredToRoad [volumeDeliveredToRoadGroupId]`



finner mye av `Savepoints are disabled because Multiple Active Result Sets (MARS) is enabled. If 'SaveChanges' fails, then the transaction cannot be automatically rolled back to a known clean state. Instead, the transaction should be rolled back by the application before retrying 'SaveChanges'. See https://go.microsoft.com/fwlink/?linkid=2149338 for more information. To identify the code which triggers this warning, call 'ConfigureWarnings(w => w.Throw(SqlServerEventId.SavepointsDisabledBecauseOfMARS)` men usikker på om det er relatert

```
requests
| where timestamp  between (datetime(2024-10-08 18:40:00) .. datetime(2024-10-08 18:50:00))
| where cloud_RoleName contains "VH" and not(name contains "Health")
//| where message contains "fdb37646-3a38-4562-a004-6e5cfc2167ea"
| order by timestamp desc 
```

![[Pasted image 20241114155438.png]]
![[Pasted image 20241114155522.png]]
![[Pasted image 20241114155828.png]]
![[Pasted image 20241114155848.png]]
![[Pasted image 20241114155921.png]]
ATS relaterted azure bus processes
	![[Pasted image 20241114161702.png]]

Mtmish detaljene
![[Pasted image 20241119090103.png]]

serviceapi.customizable
skal denne fortsatt ikke bli behandlet? 
![[Pasted image 20241119092507.png]]

