## Payment Request

> Here is the common and best practice for sending data to Ximpay Web API with HTTP POST. 

```text
Token generated on 1/31/2015.
```

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

HTTP Method : HTTP/S POST

Mobile Network Operator | Ximpay URL
--- | ---
Telkomsel | https://secure.ximpay.com/api/04/Gopayment.aspx
Indosat | https://secure.ximpay.com/api/07/Gopayment.aspx

