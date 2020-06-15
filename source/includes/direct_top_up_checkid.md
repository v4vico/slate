## Check ID

To ensure the user validity, user must enter its user id for Ximpay to check to game partner. 
Therefore game partner must have ID Checking URL. 


##### Request

> Request Example

```json
{
    "game":"TicTacToe",
    "userid":"123456",
    "name":"Dadang",
    "server":"Server-Asia",
    "os":"Android",
    "token":"token"
}
```


Method | Content-Type | URL
-------|---------|------
HTTP/S POST | application/json | Merchant URL

Parameter Name | Optional |Type | Description 
----------------|----------|------|-------
game | Mandatory | String | Game name 
userid | Optional |String | ID User 
name | Optional |String | User Name 
server | Optional |String |Server Name 
os | Optional |String | ID User 
token | mandatory | String | md5((game + user_id + name + os + secret_key).toLowerCase()).toLowerCase()

##### Response

> Response

```json
{
    "username":"Popo",
    "userid":"123456",
    "avatarurl":"http://img.com/avatar.png",
    "charname":"Popo",
    "result":{
        "code":10,
        "msg":"OK"
    }
}
```

Parameter | Type | Optional
-------|-------|---------
username | String | Optional
userid | String | Optional
avatarurl | String | Optional
charname | String | Optional
result | Object | Mandatory

<br/>
**Result Object**

 Code | Msg
----|-------
10 | OK
20 | User Not Found
