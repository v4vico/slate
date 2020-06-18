## OTP Submit Response

> Example successful OTP submit

```json
{
  "ximpaytransaction": [
    {
      "responsecode": 1,
      "ximpayid": "55EE8B0FD1204709B97E604491D74EE0",
      "pin": "ZTY3RX"
    }
  ]
}
```

> Example failed OTP submit

```json
{
  "ximpaytransaction": [
		{"responsecode": -2}
  ]
}
```

Submitted OTP will be verified and responded to merchant/partner. Verified OTP will be proceed to charge user mobile credit.

Parameter | Type | Description
--- | --- | ---
responsecode | Numeric | Ximpay response code
ximpayid | Alphanumeric | Ximpay transaction ID
pin | Alphanumeric | OTP for payment
