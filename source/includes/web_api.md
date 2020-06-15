# SMS DCB Web API

Ximpay Web API, give merchants freedom to use their own GUI for payment.

## Payment Request

Mobile Network Operator | Ximpay URL | Method
-----|------|-------
Telkomsel | https://secure.ximpay.com/api/04/Gopayment.aspx | HTTP/S POST Protocol
Indosat | https://secure.ximpay.com/api/07/Gopayment.aspx | HTTP/S POST Protocol

> Here is the common and best practice for sending data to Ximpay API with HTTP POST. 
Token generated on 1/31/2015.

```json
{
    "partnerid":"tykm",
    "itemid":"point1",
    "cbparam":"123456",
    "token":"a7926959c49420a7d9a01a89dbfd1286",
    "op":"TSEL",
    "msisdn":"0812xxxxxxxx",    	
    "username": "user@triyakom.com"
}
```
