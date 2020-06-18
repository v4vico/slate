## Payment Notification

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

> Example of Payment Status

```text
https://merchant_callback_URL?ximpayid=1F12BB46435A46738ABBA4AF23BCFB9D&ximpaystatus=1&
cbparam=123456&ximpaytoken=86d4191bfc30afefb7c89a1a17ddfb61&failcode=0
```

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