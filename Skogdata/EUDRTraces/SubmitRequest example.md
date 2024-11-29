```xml
{<?xml version="1.0" encoding="utf-16"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <wsse:Security xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">
      <wsu:Timestamp>
        <wsu:Created>2024-11-11T07:57:17Z</wsu:Created>
        <wsu:Expires>2024-11-11T07:57:27Z</wsu:Expires>
      </wsu:Timestamp>
      <wsse:UsernameToken>
        <wsse:Username>n00grbyp</wsse:Username>
        <wsse:Password Type="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0#PasswordDigest">HqDEj0tK0ArvzpWVEXW2WXWPXNI=</wsse:Password>
        <wsse:Nonce EncodingType="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0#Base64Binary">oTyoH6/kpM6SzgEJ6x2HKA==</wsse:Nonce>
        <wsu:Created>2024-11-11T07:57:17Z</wsu:Created>
      </wsse:UsernameToken>
    </wsse:Security>
    <v4:WebServiceClientId xmlns:v4="http://ec.europa.eu/sanco/tracesnt/base/v4">eudr-repository</v4:WebServiceClientId>
  </s:Header>
  <s:Body xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <SubmitStatementRequest xmlns="http://ec.europa.eu/tracesnt/certificate/eudr/submission/v1">
      <operatorType>OPERATOR</operatorType>
      <statement>
        <internalReferenceNumber xmlns="http://ec.europa.eu/tracesnt/certificate/eudr/model/v1">ident</internalReferenceNumber>
        <activityType xmlns="http://ec.europa.eu/tracesnt/certificate/eudr/model/v1">IMPORT</activityType>
        <comment xmlns="http://ec.europa.eu/tracesnt/certificate/eudr/model/v1">Opprettet av VSYS Virkeshandel 2024-11-11T07:57:16Z</comment>
        <commodities xmlns="http://ec.europa.eu/tracesnt/certificate/eudr/model/v1">
          <descriptors>
            <descriptionOfGoods>Raw forest material</descriptionOfGoods>
            <goodsMeasure>
              <netWeight>2</netWeight>
            </goodsMeasure>
          </descriptors>
          <hsHeading>4403</hsHeading>
          <speciesInfo>
            <scientificName>A</scientificName>
            <commonName>a</commonName>
          </speciesInfo>
          <producers>
            <country>NO</country>
            <name>consignorName</name>
            <geometryGeojson>eyJ0eXBlIjoiRmVhdHVyZUNvbGxlY3Rpb24iLCJmZWF0dXJlcyI6W3sidHlwZSI6IkZlYXR1cmUiLCJwcm9wZXJ0aWVzIjp7fSwiZ2VvbWV0cnkiOnsiY29vcmRpbmF0ZXMiOiBbW1s3LjI2MTQ1ODY1Njc3OTYzOCwyNy4zNzgwODMzOTgxMjQyMDRdLFsxMi4zODI3MzI2NjA1OTU4MiwxMS4zOTkwMTI1NDMyMDg3NDFdLFszMS4xMTIyNDQzNDA2OTA1OCw3LjU5MDUyNTI4MzAwODYwM10sWzMwLjM3MTEwMzM5NTY0NTY5LDIzLjMzMjAwNjg4ODI4MDU2XSxbNy4yNjE0NTg2NTY3Nzk2MzgsMjcuMzc4MDgzMzk4MTI0MjA0XV1dLCJ0eXBlIjoiUG9seWdvbiJ9fV19</geometryGeojson>
          </producers>
        </commodities>
        <geoLocationConfidential xmlns="http://ec.europa.eu/tracesnt/certificate/eudr/model/v1">false</geoLocationConfidential>
      </statement>
    </SubmitStatementRequest>
  </s:Body>
</s:Envelope>}
```