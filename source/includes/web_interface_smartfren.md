## Web Interface Smartfren

<aside class="notice">
<b>Version 1.2</b> <br/> 
Last update <em>January, 2018</em>
</aside>

### Service Mechanism

Interaction between Merchant as Triyakom partner and Ximpay payment gateway service pictured below. User Mobile Credit will be called as **"pulsa"**  in this documentation.

1. Consumer visit Merchant Website and select Ximpay as payment method.

    ![Image](Picture1.png) ![Image](Picture2.png)

2. Browser will open a pop up or new tab or new page to Ximpay. On first page of ximpay, user will be asked to 
input their cellphone number.

    ![Image](Picture3.png)

3. After user input their cellphone number in the page, ximpay page will inform user to check incoming SMS and 
follow instruction in SMS that has been sent by ximpay system.

    ![Image](Picture4.png)

4. To proceed transaction, consumer have to reply SMS from their mobile phone with the keyword stated in SMS. 
This is necessary as a sign of consumer awareness of buying product and agreed to pay with their Pulsa.

    ![Image](Picture5.png)

5. If consumer have enough pulsa then they will be charged and notified via SMS. Ximpay page also automatically
change to show payment status message (Success, Failed, or Insufficient Balance).

    ![Image](Picture6.png) ![Image](Picture7.png)


### Web Technical Specification

This section will explain technically how to interact with Ximpay Web Interface for Payment Gateway.

#### Payment Request 

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

Ximpay URL | https://secure.ximpay.com/09SDP/?act=reqpay
------|-------
Method | HTTP/S POST Protocol

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


Below is example of ximpay parameter that will be encrypted. We need to join and set to **LOWER CASE** before encrypt to md5

PAYMENT REQUEST TOKEN

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


### Payment Status

At the end of transaction, Ximpay will send payment status to Merchant. With this status, merchant will know 
whether the payment is success or failed in order to proceed to consumer. In this case, Merchant must provide 
and register API URL that will be used as callback URL for Ximpay which receive callback parameter from 
Ximpay.

Method | HTTP/S GET Protocol
-----|-------
Partner URL | Merchant_Callback_URL

**Callback Parameter:**

Following are some parameters that will be send to merchant callback URL.

1. **ximpayid**<br/>
Ximpay transaction ID. Ximpay have unique transaction ID for each transaction requested with length of 35 
character.
2. **ximpaystatus**<br/>
Status of transaction. Defined by number 1,2 and 3

Status | Description
----|-----
1 | Success
2 | Not Enough Credit/Insufficient balance
3 | Failed/Undeliverable

You should only add user's item if the ximpaystatus is 1 (success), otherwise don't give user anything.

3. **cbparam**<br/>
Your transaction ID that you have sent at the beginning, when requesting payment.
4. **ximpaytoken**<br/>
New generated token that will be send along with payment status. This new token is useful for 
authenticating valid payment response. When Ximpay send payment response, merchant have to create 
token and match with the incoming token. If doesn't match, then just ignore the response because it is not 
a valid payment response. <br/>
Below is the example of generating new token. Same like request token, the creation need to join some 
parameters and set to LOWER CASE before encrypt to md5. 

> Example in Java:

```java 
String ximpayId = "1F12BB46435A46738ABBA4AF23BCFB9D";
String ximpayStatus = "1";
String callbackParam = "123456";
String secretKey = "ABCD";

String lower = (ximpayId + ximpayStatus + callbackParam + secretKey).toLowerCase();
String token = getMD5(lower);
```

<aside class="success">
MD5 ( LowerCase ( ximpay id + ximpay status + Callback Parameter + Secret Key ) )
</aside>

5. **failcode**<br/>
Specify fail transaction to be more detail failcode will be explaining some fail condition.

Code| Description | Categorized On CMS
---|----|-----
301|  User not input phone number | Canceled
302|  User not send sms | Canceled
303|  User not input PIN | Canceled
304|  Failed send PIN to user | Failed
305|  Over limit balance | Canceled
306|  Failed Charge | Failed
307|  Failed send to operator | Failed

For  Success (status=1) and Not Enough Pulsa (status=2) case, the failcode will be given as 0 (failcode=0).

Now that all parameters have been explained. Here is the example of how payment response will be sent to 
merchant callback URL from Ximpay server:

<aside class="warning">
&lt;merchant_callback_URL&gt;?ximpayid=1F12BB46435A46738ABBA4AF23BCFB9D&ximpaystatus=1&amp;
cbparam=123456&amp;ximpaytoken=86d4191bfc30afefb7c89a1a17ddfb61&amp;failcode=0
</aside>

Merchant Callback URL must response with HTTP 200 and show "Success" message when they receive and 
successfully process payment status (1, 2 and 3). 

In case Merchant URL is getting error or exception, Merchant can show any error message as response except 
"Success". Please do not blindly give "Success" as a message when ximpay send invalid, empty or incomplete 
parameters

If Merchant response not meet the response qualification above, resend attempts will be made according to 
table shown below. If your URL is not available for the entire time, payment status will be lost.

RETRY NUMBER | INTERVAL | CUMULATIVE
-----|----|----
0 | 1 minute|  1 hour
1 | 2 minute|  3 hour
2 | 5 minute|  8 hour
3 | 10 minute | 18 hour

For special case, we need merchant callback URL to be possible to receive payment status update from 
unsuccessful status (ximpaystatus = 2 and ximpaystatus = 3) to become successfull status (ximpaystatus = 1). 
This case will be maintained manually through further communication, investigation and analysis by both parties 
(Ximpay and Merchant PIC).

