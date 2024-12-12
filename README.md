# MalyBotApi

# REST API

The REST API to the example app is described below.

## Application Login
 
### Request

`Post /applogin`

    curl -i -H 'Accept: application/json 'http://109.199.100.162/applogin
    
    body :
     {
      "secretKey": "string",
      "appId": "string"
     }
get secretKey and appId from web site 
### Response

    {
      "code": 0,
      "message": "string",
      "content": {
            "accessToken": "string"
            "expiresIn": 22,
            "tokenType": "Bearer"
          }
    }

## Get Requset From Bot

### Request

`Get /api/Bot/Requset`

    curl -i -H 'Accept: application/json'  http://localhost:7000/api/Bot/Requse

### Response

       {
          "code": 0,
          "message": "Success",
          "content": [
                    {
                      "clientId": int,
                      "requset_no": int,
                      "from": "string",
                      "doc_type": "string",
                      "containt_type": "string",
                      "menu_no": null,
                      "processed": 0,
                      "action": null,
                      "param": null
                    }
           ]
    }
`clientId` :what is client save requset
`requset_no` :Requset no
`from` : Mobile Number thet sen requset
`doc_type, menu_no,`:null
`processed` :return 0 if not processed else return 1,
`action` : return action from menu item selected

## Send Message

### Request

`P /api/Client/senMessage`

    
    curl -i -H 'Accept: application/json' http://localhost:7000/thing/1
    body:
     {
      "clientId": 0,
      "requset_no": 0,
          "message": {
            "to": "string",
            "contentType": "string",
            "mimType": "string",
            "fileName": "string",
            "menuNo": "string",
            "content": "string",
            "capture": "string"
        }
    }
`clientId` : null
`requset_no`: requset number that response by get requset or null
`to` : mobile number
`mimType` : Text ,Media 
`fileName` : file name if `mimType` Media
`menuNo` :null
`content` :plan text or base64 if `mimType` is Media
`capture` :if `mimType` is Media
### Response

    {
      "code": int,
      "message": "String",
      "content": "string"
    }
`code`:200 if  Success
`message` : Success or error message if not Success
`content` : return Id message if code 200 or error message details
## Create Menu or Update

### Request

`POST /api/Menu/Sync`

    curl -i -H 'Accept: application/json' http://localhost:7000/api/Menu/Sync
     body :
       {
          "id": 0,
          "menuName": "string",
          "body": "string",
          "buttonText": "string",
          "title": "string",
          "footer": "string",
          "structureType": "string",
          "itemTitle": "string",
          "type": "string",
          "botId": 0,
          "items": [
            {
              "id": 0,
              "title": "string",
              "description": "string",
              "action": "string",
              "menuModelId": 0,
              "isDeleted": true
            }
          ]
        }
`id`: if 0 or null well create new menu  else update menu by id <br />
`menuName` :Name from Api not Use by bot 
`body` : 
![image description](./menu.jpg)
### Response

    HTTP/1.1 404 Not Found
    Date: Thu, 24 Feb 2011 12:36:30 GMT
    Status: 404 Not Found
    Connection: close
    Content-Type: application/json
    Content-Length: 35

    {"status":404,"reason":"Not found"}

## Create another new Thing

### Request

`POST /thing/`

    curl -i -H 'Accept: application/json' -d 'name=Bar&junk=rubbish' http://localhost:7000/thing

### Response

    HTTP/1.1 201 Created
    Date: Thu, 24 Feb 2011 12:36:31 GMT
    Status: 201 Created
    Connection: close
    Content-Type: application/json
    Location: /thing/2
    Content-Length: 35

    {"id":2,"name":"Bar","status":null}

## Get list of Things again

### Request

`GET /thing/`

    curl -i -H 'Accept: application/json' http://localhost:7000/thing/

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:31 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 74

    [{"id":1,"name":"Foo","status":"new"},{"id":2,"name":"Bar","status":null}]

## Change a Thing's state

### Request

`PUT /thing/:id/status/changed`

    curl -i -H 'Accept: application/json' -X PUT http://localhost:7000/thing/1/status/changed

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:31 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 40

    {"id":1,"name":"Foo","status":"changed"}

## Create Menu 

### Request

`POST /api/Menu/Sync`

    curl -i -H 'Accept: application/json' http://localhost:7000/thing/1
    body :
     {
  "id": 0,
  "menuName": "string",
  "body": "string",
  "buttonText": "string",
  "title": "string",
  "footer": "string",
  "structureType": "string",
  "itemTitle": "string",
  "type": "string",
  "botId": 0,
  "items": [
    {
      "id": 0,
      "title": "string",
      "description": "string",
      "action": "string",
      "menuModelId": 0,
      "isDeleted": true
    }
  ]
}
### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:31 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 40

    {"id":1,"name":"Foo","status":"changed"}

## Change a Thing

### Request

`PUT /thing/:id`

    curl -i -H 'Accept: application/json' -X PUT -d 'name=Foo&status=changed2' http://localhost:7000/thing/1

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:31 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 41

    {"id":1,"name":"Foo","status":"changed2"}

## Attempt to change a Thing using partial params

### Request

`PUT /thing/:id`

    curl -i -H 'Accept: application/json' -X PUT -d 'status=changed3' http://localhost:7000/thing/1

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:32 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 41

    {"id":1,"name":"Foo","status":"changed3"}

## Attempt to change a Thing using invalid params

### Request

`PUT /thing/:id`

    curl -i -H 'Accept: application/json' -X PUT -d 'id=99&status=changed4' http://localhost:7000/thing/1

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:32 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 41

    {"id":1,"name":"Foo","status":"changed4"}

## Change a Thing using the _method hack

### Request

`POST /thing/:id?_method=POST`

    curl -i -H 'Accept: application/json' -X POST -d 'name=Baz&_method=PUT' http://localhost:7000/thing/1

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:32 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 41

    {"id":1,"name":"Baz","status":"changed4"}

## Change a Thing using the _method hack in the url

### Request

`POST /thing/:id?_method=POST`

    curl -i -H 'Accept: application/json' -X POST -d 'name=Qux' http://localhost:7000/thing/1?_method=PUT

### Response

    HTTP/1.1 404 Not Found
    Date: Thu, 24 Feb 2011 12:36:32 GMT
    Status: 404 Not Found
    Connection: close
    Content-Type: text/html;charset=utf-8
    Content-Length: 35

    {"status":404,"reason":"Not found"}

## Delete a Thing

### Request

`DELETE /thing/id`

    curl -i -H 'Accept: application/json' -X DELETE http://localhost:7000/thing/1/

### Response

    HTTP/1.1 204 No Content
    Date: Thu, 24 Feb 2011 12:36:32 GMT
    Status: 204 No Content
    Connection: close


## Try to delete same Thing again

### Request

`DELETE /thing/id`

    curl -i -H 'Accept: application/json' -X DELETE http://localhost:7000/thing/1/

### Response

    HTTP/1.1 404 Not Found
    Date: Thu, 24 Feb 2011 12:36:32 GMT
    Status: 404 Not Found
    Connection: close
    Content-Type: application/json
    Content-Length: 35

    {"status":404,"reason":"Not found"}

## Get deleted Thing

### Request

`GET /thing/1`

    curl -i -H 'Accept: application/json' http://localhost:7000/thing/1

### Response

    HTTP/1.1 404 Not Found
    Date: Thu, 24 Feb 2011 12:36:33 GMT
    Status: 404 Not Found
    Connection: close
    Content-Type: application/json
    Content-Length: 35

    {"status":404,"reason":"Not found"}

## Delete a Thing using the _method hack

### Request

`DELETE /thing/id`

    curl -i -H 'Accept: application/json' -X POST -d'_method=DELETE' http://localhost:7000/thing/2/

### Response

    HTTP/1.1 204 No Content
    Date: Thu, 24 Feb 2011 12:36:33 GMT
    Status: 204 No Content
    Connection: close


