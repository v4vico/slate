## OTP Submit

> Example OTP Confirmation

```json
{
    "ximpayid":"55EE8B0FD1204709B97E604491D74EE0",
    "codepin":"ZTY3RX",
    "ximpaytoken":"0891436da730d0ad6bac365dc67c0ce9",    
}
```

OTP Submit API is used only for XL payment method.

HTTP Method : HTTP/S POST

Mobile Network Operator | Production URL | Sandbox URL
--- | --- | ---
XL | https://secure.ximpay.com/api/08flex/Gopin.aspx | https://secure.ximpay.com/api/08flex/Gopin.aspx


Parameter | Type | Length | Description
--- | --- | --- | ---
ximpayid | Alphanumeric | 32 | Ximpay transaction ID
codepin | Alphanumeric | 6 | Ximpay OTP for payment confirmation
ximpaytoken | Alphanumeric | 35 | Token for OTP submit