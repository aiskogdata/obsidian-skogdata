```xml
{<?xml version="1.0" encoding="utf-16"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <wsse:Security xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">
      <wsu:Timestamp>
        <wsu:Created>2024-11-11T09:28:36Z</wsu:Created>
        <wsu:Expires>2024-11-11T09:28:46Z</wsu:Expires>
      </wsu:Timestamp>
      <wsse:UsernameToken>
        <wsse:Username>n00grbyp</wsse:Username>
        <wsse:Password Type="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0#PasswordDigest">yS6DbmttOEC5MliYCnXYbGCYtWk=</wsse:Password>
        <wsse:Nonce EncodingType="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0#Base64Binary">XLYXFFh6OeW1hMOWmkWwqg==</wsse:Nonce>
        <wsu:Created>2024-11-11T09:28:36Z</wsu:Created>
      </wsse:UsernameToken>
    </wsse:Security>
    <v4:WebServiceClientId xmlns:v4="http://ec.europa.eu/sanco/tracesnt/base/v4">eudr-repository</v4:WebServiceClientId>
  </s:Header>
  <s:Body xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <EudrEchoRequest xmlns="http://ec.europa.eu/tracesnt/eudr/echo">
      <query>hello traces</query>
    </EudrEchoRequest>
  </s:Body>
</s:Envelope>}
```