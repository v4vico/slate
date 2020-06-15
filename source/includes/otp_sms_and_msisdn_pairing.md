# OTP SMS and MSISDN Pairing

Security is top priority of our payment by pulsa service. New layer of payment security is needed in 
order to reduce the fraud possibility, and because of that Ximpay provide solution and make payment 
by pulsa security stronger. With this new layer of security, we hope mobile user will be more cautious 
and aware of the service by pulsa.

<aside class="notice">
<b>Version 1.0</b> <br/> 
Last update <em>January, 2018</em>
</aside>

## OTP SMS API

This section will explain technically how partners send SMS OPT to user.

### OTP Request

> OTP Request Body Example

```json
{
    "msisdn":"628xxxxx",
    "id":"user_id",
    "pwd":"user_password",
    "sms":"Code OTP anda adalah xxx",
    "service":"PartnerCodeService"
}
```

API URL | https://api.ximpay.com/send/otp.aspx 
---------|--------------------------------------
Method | HTTP/S POST Protocol
Content-Type | application/json

Request Body

No | Parameter | Type | Length | Description
---|-----------|------|--------|------------
1 | msisdn | Numeric | 15 | User phone number
2 | id | Alphanumeric | 50 | SMS OTP Access ID
3 | pwd | Alphanumeric | 50 | SMS OTP Access Password
4 | sms | Alphanumeric | 160 | OTP SMS Text
5 | service | Alphanumeric | 50 | Service Code for OTP

### OTP Response

> OTP Response Example

```json
{
"tid":"000D7C52EB4C42F0B53D5D7C64045AF4",
"status-id":"100"
}
```

After OTP request, response will be as below:

Status | Description
------|--------
100 | Success
106 | Empty MSISDN
111 | Empty Data
112 | Invalid JSON Format
113 | Invalid ID or PWD
114 | Service not found
115 | Operator not found
116 | Maximum SMS Character exceeded
117 | Internal Data Base error


## MSISDN Pairing API

This section will explain technically how to interact with Ximpay MSISDN pairing API for Payment Gateway.

### Pairing Token

MD5 ( Lower case (Partner ID + Username + MSISDN + secret key))


### Pairing Request

> Pairing Request Body Example

```json
{
    "partnerid": "UNIP",
    "username": "Vico",
    "msisdn": "628131789554",
    "token": "ec54adb27186924d0a27d48c238ad511"
}
```

API URL | https://api.ximpay.com/partner/pair.aspx
----| ----------
Method | HTTP/S POST Protocol
Content-Type | application/json

Request Body

No | Parameters | Type | Length | Description
---- |---- |---- |---- |----
1 | partnerId | Alphanumeric | 5 | Merchant partner ID. e.g., Triyakom partner id is TYKM
2 | username | Alphanumeric | 50 | User identity in merchant
3 | msisdn | Numeric | 15 | User Phone Number
5 | token | Alphanumeric | 32 | Token for pairing request

### Pairing Response

> Pairing Response Example

```json
{
    "response": "100",
    "responsemessage": "Pairing Success"
}
```

After pairing request, response will be as below:

Status | Description
----|-----
100 | Success / Updated
101 | Already Paired
102 | Phone already used
103 | Reach update Limit
104 | Empty Partner ID
105 | Empty User ID
106 | Empty MSISDN
107 | Empty Token
108 | Unauthorized Merchant
109 | Invalid MSISDN Format
110 | Invalid Token
111 | Empty Data
112 | General Error, Reason may vary
