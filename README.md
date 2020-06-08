# datacore-apis-user-docs
Documentation on how to use Datacore Apis

## How to send data to Datacore

Datacore supports 3 different approaches of posting documents to a given API.

1. Regular: POST to /api-name/api-version/user-token
2. Blob: POST to /blob/api-name/api-version/user-token
3. AMP: POST to /amp/api-name/api-version/user-token

### Regular
Using the Regular POST type you will have to follow the most common approach.

Sending a single document
```http
POST /api-name/api-version/user-token
Authorization: YourAuthorizationHeaderContent
Content-Type: application/json

{"your":"single doc","b":true,"n":1}
```

Response: 
```http
HTTP/1.1 202 Accepted
x-request-id: o594w62548df
x-response-id: o594w62548df
access-control-expose-headers: *
content-type: application/json; charset=utf-8
content-length: 72
Date: Mon, 08 Jun 2020 18:51:39 GMT
Connection: close

{
  "count": 1,
  "success": 1,
  "results": [
    {"_id": "15916422992871331.4640aaa"}
  ]
}
```


Sending multiple documents
```http
POST /api-name/api-version/user-token
Authorization: YourAuthorizationHeaderContent
Content-Type: application/json

[
    {"your":"doc A","b":true,"n":1},
    {"your":"doc B","b":false,"n":2}
]
```

Response:
```http
HTTP/1.1 202 Accepted
x-request-id: ofj5k20q8rh5
x-response-id: ofj5k20q8rh5
access-control-expose-headers: *
content-type: application/json; charset=utf-8
content-length: 121
Date: Mon, 08 Jun 2020 18:50:03 GMT
Connection: close

{
  "count": 2,
  "success": 2,
  "results": [
    {"_id": "15916422031847881.1917aaaa-00001"},
    {"_id": "15916422031847881.1917aaaa-00002"}
  ]
}
```

### Blob
The Blob approach is mostly used by web applications that intend to avoid the OPTIONS requests when sending data to datacore.

This can be acomplished by sending the ```Content-Type``` header as ```text/plain``` and
no setting an ```Authorization``` header.

In this case the ```Authorization``` header's content is part of the body of the POST
as its first line and the real body, the document or documents, come right after the first ```\n``` break line character.

```http
POST /blob/api-name/api-version/user-token
Content-Type: text/plain

YourAuthorizationHeaderContent
[
    {"your":"doc A","b":true,"n":1},
    {"your":"doc B","b":false,"n":2}
]
```
OR
```http
POST /blob/api-name/api-version/user-token
Content-Type: text/plain

YourAuthorizationHeaderContent\n[{"your":"doc A","b":true,"n":1},{"your":"doc B","b":false,"n":2}]
```

The Response is the same as if it were sent with the Regular approach:
```http
HTTP/1.1 202 Accepted
x-request-id: 8fu5i4o39fi
x-response-id: 8fu5i4o39fi
access-control-expose-headers: *
content-type: application/json; charset=utf-8
content-length: 121
Date: Mon, 08 Jun 2020 18:50:03 GMT
Connection: close

{
  "count": 2,
  "success": 2,
  "results": [
    {"_id": "15916441267172131.2293aaaa-00001"},
    {"_id": "15916441267172131.2293aaaa-00002"}
  ]
}
```

### AMP
The AMP approach is meant to be used by AMP web pages that have several limitations when it comes to javascript.

The usage here is by setting the ```Content-Type``` header as ```application/json```, which occurs naturally in AMP pages and setting no ```Authorization``` header.

In this case the ```Authorization``` header's content is part of the body of the body of 
the POST request which **must be an Array with exactly 2 elements**

1. The authorization header content
1. The document or documents intended to be saved

The AMP approach goes as follows
```http
POST /amp/api-name/api-version/user-token
Content-Type: application/json

[
    "YourAuthorizationHeaderContent",
    [
        {"your":"doc A","b":true,"n":1},
        {"your":"doc B","b":false,"n":2}
    ]
]
```

The Response is the same as if it were sent with the Regular approach:
```http
HTTP/1.1 202 Accepted
x-request-id: 8fu5i4o39fi
x-response-id: 8fu5i4o39fi
access-control-expose-headers: *
content-type: application/json; charset=utf-8
content-length: 121
Date: Mon, 08 Jun 2020 19:29:55 GMT
Connection: close

{
  "count": 2,
  "success": 2,
  "results": [
    {"_id": "15916445959516161.3157aaaa-00001"},
    {"_id": "15916445959516161.3157aaaa-00002"}
  ]
}
```
