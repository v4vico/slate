## Payment Request 

> Here is the common and best practice for sending data to Ximpay API with HTTP POST. 
Token generated on 1/31/2015.

```html
<form action="https://secure.ximpay.com/01/?act=reqpay" id="Payment" name="Payment" 
method="POST" >
</table><br /><br />
<input type="hidden" name="PARTNERID" value="tykm">
<input type="hidden" name="ITEMID" value="point1">
<input type="hidden" name="REDIRECTURL" value="http://www.triyakom.com/payhistory">
<input type="hidden" name="CBPARAM" value="123456">
<input type="hidden" name="TOKEN" value="3a5c28816aba03a21d323f3558db0561">
<input type="hidden" name="OP" value="SF">
<input type="hidden" name="MSISDN" value="088112653254">
<input type="hidden" name="USERNAME" value="username">
<input name="submit" type="submit" class="btn_submit" id="submit" value="SUBMIT" />
</form>
```

Ximpay URL | Method
------|-------
https://secure.ximpay.com/09SDP/?act=reqpay | HTTP/S POST Protocol

Parameter explanation:

No | Parameters | Type | Length | Description
---|----|----|---|----
1 | PARTNERID | Alphanumeric | 5 | Merchant partner ID. e.g., Triyakom partner id is TYKM
2 | ITEMID | Alphanumeric | 8 | Merchant item collection ID. e.g., "point extra" item id is POINT1. You should register your item to Ximpay to get this item ID.
3 | REDIRECTURL | Alphanumeric | 2^31-3(max) | Redirect URL when consumer click "back" button at the end of transaction. This url can be set to wherever page you desired consumer will o to. In above example consumer will be redirected to payhistory page that maybe contain information of Page 10 of 14 all consumer purchase history. Noted, This URL is NOT RELATED to payment status.
4 | CBPARAM | Alphanumeric | 512 | Your callback parameter. e.g., You want to send your own transaction identifier(ID) "123456", when Ximpay send payment status to your callback URL.
5 | TOKEN | Alphanumeric | 35 | Token for request. Will be explained in chapter 3.1.1.
6 | OP | Alphanumeric | 20 | Telecommunication operator code. SF (Smartfren)
7 | MSISDN | Numeric | 25 | User cellphone number. Format example “0881xx” or “62881xx”. (Optional)
8 | USERNAME | Alphanumeric | 100 | User identity, can be in form of email also. (Optional)

If all parameter valid, then Ximpay page will appeared. Else error message will be displayed on browser.

**Generate Token**

Here will be explained how to create ximpay token. Actually token consist of all parameter that 
merchant use when requesting payment to Ximpay. The difference are date and secret key 
parameter also operator code.

Parameters | Description
-------|-------
Partner Id | Same partner id from request
Item Id | Same item id from request
Redirect URL | Same Redirect URL (without encode)
Callback Parameter | Same callback parameter (without encode)
Date | Format ("M/d/yyyy") . e.g., 1/31/2015 (No leading zero)
Secret Key | Secret key will be generated and given to you after registered as Ximpay Partner. 


Below is example of ximpay parameter that will be encrypted. We need to join and set to lowercase before encrypt to md5.

> Example in Java:

```java 
String partnerId = "TYKM";
String itemId = "POINT1";
String url = "http://www.triyakom.com/payhistory";
String callbackParam = "123456";
String date = "1/31/2015";
String secretKey = "ABCD";

String lower = (partnerId + itemId + url + callbackParam + date + secretKey).toLowerCase();
String token = getMD5(lower);
```

<aside class="success">
MD5 ( LowerCase ( Partner Id + Item Id + Redirect URL + Callback Parameter + Date + Secret Key ) )
</aside>