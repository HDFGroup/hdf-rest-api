GET Datatype
============

Description
-----------

Returns information about the committed datatype with the UUID given in the URI.

Requests
--------

### Syntax

``` sourceCode
GET /datatypes/<id> HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
GET /datatypes/<id>?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

*&lt;id&gt;* is the UUID of the requested datatype.

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

#### id

The UUID of the datatype object.

#### root

The root group of the domain which the datatype is within.

#### type

A JSON object representing the type of the datatype object.

#### attributeCount

The number of attributes belonging to the datatype.

#### created

A timestamp giving the time the datatype was created in UTC (ISO-8601 format).

#### lastModified

A timestamp giving the most recent time the datatype has been modified (i.e. attributes updated) in UTC (ISO-8601 format).

#### hrefs

An array of links to related resources. See ../Hypermedia.

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

Get the committed datatype with UUID: "t-3e37ab7e-...".

### Sample Request

``` sourceCode
GET /datatypes/t-3e37ab7e-86b3-11e8-bce3-0242ac12000c HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Accept-Encoding: gzip, deflate
Accept: */*
```

### Sample cURL command

``` sourceCode
$ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datatypes/t-3e37ab7e-86b3-11e8-bce3-0242ac12000c
```

### Sample Response

``` sourceCode
HTTP/1.1 200 OK
Date: Fri, 13 Jul 2018 15:57:37 GMT
Content-Length: 602
Etag: "c53bc5b2d3c3b5059b71ef92ca7d144a2df54456"
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{
    "id": "t-3e37ab7e-86b3-11e8-bce3-0242ac12000c",
    "root": "g-b116b6f0-85e9-11e8-9cc2-0242ac120008",
    "domain": "/shared/tall.h5",
    "type": {
        "class": "H5T_FLOAT",
        "base": "H5T_IEEE_F64LE"
    },
    "attributeCount": 0,
    "created": 1531496503.6064572,
    "lastModified": 1531496503.6064572,
    "hrefs": [
        {"rel": "self", "href": "hsdshdflab.hdfgroup.org/datatypes/t-3e37ab7e-86b3-11e8-bce3-0242ac12000c"},
        {"rel": "root", "href": "hsdshdflab.hdfgroup.org/groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008"},
        {"rel": "home", "href": "hsdshdflab.hdfgroup.org/"},
        {"rel": "attributes", "href": "hsdshdflab.hdfgroup.org/datatypes/t-3e37ab7e-86b3-11e8-bce3-0242ac12000c/attributes"}
    ]
}
```

Related Resources
-----------------

-   DELETE\_Datatype
-   GET\_Datatypes
-   POST\_Datatype
-   ../DatasetOps/POST\_Dataset
-   ../AttrOps/PUT\_Attribute

