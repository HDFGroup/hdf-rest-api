GET Group
=========

Description
-----------

Returns information about the group with the UUID given in the URI.

Requests
--------

### Syntax

``` sourceCode
GET /groups/<id> HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
GET /groups/<id>?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

*&lt;id&gt;* is the UUID of the requested group.

### Request Parameters

#### include\_links

If this request parameter is provided, the links of the group are included in the response.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

Responses
---------

### Response Headers

This implementation of the operation uses only response headers that are common to most responses. See ../CommonResponseHeaders.

### Response Elements

On success, a JSON response will be returned with the following elements:

#### id

The UUID of the requested group.

#### root

The root group of the domain that the group is within.

#### attributeCount

The number of attributes belonging to the group.

#### linkCount

The number of links belonging to the group.

#### created

A timestamp giving the time the group was created in UTC (ISO-8601 format).

#### lastModified

A timestamp giving the most recent time the group has been modified (i.e. attributes or links updated) in UTC (ISO-8601 format).

#### hrefs

An array of hypertext links to related resources. See ../Hypermedia.

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

### Sample Request

``` sourceCode
GET /groups/g-be6eb652-83c5-11e8-b9ee-0242ac12000a HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Accept-Encoding: gzip, deflate
Accept: */*
```

### Sample cURL command

``` sourceCode
$ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be6eb652-83c5-11e8-b9ee-0242ac12000a
```

### Sample Response

``` sourceCode
HTTP/1.1 200 OK
Date: Thu, 12 Jul 2018 18:27:24 GMT
Content-Length: 711
Etag: "2c410d1c469786f25ed0075571a8e7a3f313cec1"
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{
    "id": "g-be6eb652-83c5-11e8-b9ee-0242ac12000a",
    "root": "g-be5996fa-83c5-11e8-a8e6-0242ac120016",
    "linkCount": 2,
    "attributeCount": 0,
    "lastModified": 1531174596.5785563,
    "created": 1531174596.2463753,
    "domain": "/shared/tall.h5",
    "hrefs": [
        {"href": "hsdshdflab.hdfgroup.org/groups/g-be6eb652-83c5-11e8-b9ee-0242ac12000a", "rel": "self"},
        {"href": "hsdshdflab.hdfgroup.org/groups/g-be6eb652-83c5-11e8-b9ee-0242ac12000a/links", "rel": "links"},
        {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"},
        {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
        {"href": "hsdshdflab.hdfgroup.org/groups/g-be6eb652-83c5-11e8-b9ee-0242ac12000a/attributes", "rel": "attributes"}
    ]
}
```

Related Resources
-----------------

-   DELETE\_Group
-   GET\_Links
-   GET\_Groups
-   POST\_Group
-   ../AttrOps/GET\_Attribute

