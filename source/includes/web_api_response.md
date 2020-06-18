## Payment Response

> Example successful payment request

```json
{
  "ximpaytransaction": [
    {
      "responsecode": 1,
      "ximpayid": "55EE8B0FD1204709B97E604491D74EE0",
      "ximpayitem": [
        {
        "item_name": "10 Credit",
        "item_desc": "Sugar Rush Online",
        "amount": 5000
        }
      ]
    }
  ]
}
```

> Example failed payment request

```json
{
  "ximpaytransaction": [
		{"responsecode": -2}
  ]
}
```

After request initiated, ximpay will give response with information that can be used by merchant/partner for user.

Parameter | Type | Description
--- | --- | ---
responsecode | Numeric | Ximpay response code
ximpayid | Alphanumeric | Ximpay transaction ID
item_name | Alphanumeric | Item name
item_desc | Alphanumeric | Item description
amount | Numeric | Charge amount

