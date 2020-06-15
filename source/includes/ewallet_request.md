## Payment Request

> Minimum Request

```json
{
	"partner_id " : "test-partner",
	"order_id" : "test-order-1",
	"amount" : 100,
	"item":[{
		"name": "test-item",
		"price": 100,
		"quantity": 1
	}],	
	"method":"gopay",
	"redirect_url": "https://pay-history.partner.com",
	"token":"test-token"
}
```

> Complete Request

```json
{
	"partner_id " : "test-partner",
	"order_id" : "test-order-1",
	"amount" : 100,
	"item":[{
		"id": "test-item-id",
		"name": "test-item",
		"price": 100,
		"quantity": 1
	}],
	"customer": {
		"phone": "08123456789",
		"email": "vico@triyakom.com"
	},
	"method":"gopay",
	"redirect_url": "https://pay-history.partner.com",
	"token":"test-token"
}
```

> Successfull response

```json
{
	"status_code": 0,
	"status": "Transaction Request Accepted",
	"url":"https://gopay-api.ximpay.com/snap?token=8f63db20-ee0e-4031-b8f9-d8ad37d91856&trans_id=9DC5C97183CE4814AF7ECD9837E4CD76",
	"order_id": "test-order-1",
	"ximpay_id": "tes-ximpay-1"
}
```

> Failed response

```json
{
	"status_code": 4000,
	"status": "Bad Request"
}
```

### Production Endpoint

Payment Method | Environment | URL | HTTP Method
------ | ------ | ------ | ------
Gopay | Production | https://gopay-api.ximpay.com | POST
Ovo | Production | https://ovo-api.ximpay.com | POST

### Development Endpoint

Payment Method | Environment | URL | HTTP Method
------ | ------ | ------ | ------
Gopay | Development | https://gopay-api-sandbox.ximpay.com | POST
Ovo | Development | https://ovo-api-sandbox.ximpay.com | POST

### Request Header
Key | Value
--- | ---
Content-Type | application/json

### Request Body

Parameter | Description | Mandatory
--- | --- | ---
partner_id | Partner ID provided by Ximpay | Yes
order_id | Unique transaction ID | Yes
amount | Total Charge | Yes
item | Array of item that will be purchased | Yes
customer | Customer information | No
method | Payment method | Yes
redirect_url | Merchant/Partner URL that will be redirected at the end of payment | Yes
token | Authentication token | Yes

#### Request Body - item

Parameter | Description | Mandatory
--- | --- | --- 
id | Item ID | No
name | Name of item | Yes
price | Price of item | Yes
quantity | Quantity of item | Yes

#### Request Body - customer

Parameter | Description | Mandatory
--- | --- | ---
phone | Customer mobile phone number | Yes
email | Customer email address | Yes

#### Request Body - token

Additional information for token:

- date

    format with “M/d/yyyy”, day and month have no leading zero (4/24/2020)

- secret_key

    provided by Ximpay

>
```text
SHA256 (LOWERCASE (partner_id + amount + order_id + date + secret_key))
```

### Response

Parameter | Description
--- | ---
status_code | Request status code 
status | Request status description
url | Payment page URL
order_id | Merchant/Partner Order ID
ximpay_id | Ximpay Transaction ID

#### Response - status_code

status_code | status | HTTP Code
--- | --- | ---
0 | Transaction request accepted | 200
4000 | Bad Request, Not Readable | 400
4000 | Method Not Allowed | 405
4000 | URL Not Found | 404
4001 | Invalid Merchant/Partner | 403
4002 | Invalid Order ID | 400
4003 | Invalid Amount | 400
4004 | Invalid Item | 400
4005 | Invalid URL | 400
4006 | Invalid Customer Email | 400
4007 | Invalid Customer Phone | 400
4008 | Invalid Token | 400
4009 | Invalid Payment Method | 400
4010 | Invalid Item Price | 400
4011 | Invalid Item Quantity | 400
4012 | Invalid Item Name | 400
5000 | Internal Server Error | 500

#### Response - url 

Use  this URL to open payment page for Gopay. Modal popup/new browser tab/new browser window is favorable since the UI is mobile friendly.

Payment page have time limit of 5 minutes in the confirmation page and 5 minute in QR code scan page.

After time limit exceeded unpaid transaction can’t be proceed and need to create new payment request.