DELETE Attribute
================

Description
-----------

The implementation of the DELETE operation deletes the attribute named in the URI.

Requests
--------

### Syntax

``` sourceCode
DELETE /groups/<id>/<name> HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
DELETE /groups/<id>/<name>?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

``` sourceCode
DELETE /datasets/<id>/<name> HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
DELETE /datasets/<id>/<name>?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

``` sourceCode
DELETE /datatypess/<id>/<name> HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
DELETE /datatypes/<id>/<name>?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

-   *&lt;id&gt;* is the UUID of the dataset/group/committed datatype
-   *&lt;name&gt;* is the url-encoded name of the requested attribute

### Request Parameters

This implementation of the operation does not use request parameters.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

Responses
---------

### Response Headers

This implementation of the operation uses only response headers that are common to most responses. See ../CommonResponseHeaders.

### Response Elements

On success, a JSON response will be returned with the following elements:

#### hrefs

An array of links to related resources. See ../Hypermedia.

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

### Sample Request

``` sourceCode
DELETE /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr1 HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Accept: */*
Accept-Encoding: gzip, deflate
```

### Sample cURL command

``` sourceCode
$ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr1
```

### Sample Response

``` sourceCode
HTTP/1.1 200 OK
Date: Sun, 15 Jul 2018 16:06:54 GMT
Content-Length: 13
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{"hrefs": []}
```

Related Resources
-----------------

-   GET\_Attributes
-   GET\_Attribute
-   ../DatasetOps/GET\_Dataset
-   ../DatatypeOps/GET\_Datatype
-   ../GroupOps/GET\_Group
-   PUT\_Attribute

