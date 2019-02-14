DELETE Datatype
===============

Description
-----------

The implementation of the DELETE operation deletes the committed datatype  
named in the URI. All attributes of the datatype will also be deleted.

Requests
--------

### Syntax

``` sourceCode
DELETE /datatypes/<id> HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
DELETE /datatypes/<id>?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

*&lt;id&gt;* is the UUID of the datatype to be deleted.

### Request Parameters

This implementation of the operation does not use request parameters.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

Responses
---------

### Response Headers

This implementation of the operation uses only response headers that are common to most responses. See ../CommonResponseHeaders.

### Response Elements

This implementation of the operation does not return any response elements.

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

### Sample Request

``` sourceCode
DELETE /datatypes/t-6b0bdf9a-86b2-11e8-89f2-0242ac120009 HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Content-Length: 0
Accept: */*
Accept-Encoding: gzip, deflate
```

### Sample cURL command

``` sourceCode
$ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datatypes/t-6b0bdf9a-86b2-11e8-89f2-0242ac120009
```

### Sample Response

``` sourceCode
HTTP/1.1 200 OK
Date: Fri, 13 Jul 2018 15:49:44 GMT
Content-Type: application/json
Server: nginx/1.15.0
```

Related Resources
-----------------

-   ../AttrOps/GET\_Attributes
-   GET\_Datatype
-   GET\_Datatypes
-   POST\_Datatype
-   ../DatasetOps/POST\_Dataset
-   ../AttrOps/PUT\_Attribute

