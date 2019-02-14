DELETE Domain
=============

Description
-----------

The DELETE operation deletes the given domain and all its resources (groups, datasets, attributes, etc.).

Requests
--------

### Syntax

``` sourceCode
DELETE / HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
DELETE /?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

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
DELETE / HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/deleteme.h5
Content-Length: 0
Accept: */*
```

### Sample cURL command

``` sourceCode
$ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/deleteme.h5" hsdshdflab.hdfgroup.org/
```

### Sample Response

``` sourceCode
HTTP/1.1 200 OK
Date: Thu, 12 Jul 2018 16:33:54 GMT
Content-Length: 0
Content-Type: application/json
Server: nginx/1.15.0
```

Related Resources
-----------------

-   GET\_Domain
-   PUT\_Domain

