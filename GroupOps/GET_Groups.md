GET Groups
==========

Description
-----------

Returns UUIDs for all the groups in a domain (other than the root group).

Requests
--------

### Syntax

``` sourceCode
GET /groups HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
GET /groups?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

### Request Parameters

This implementation of the operation uses the following request parameters (both optional):

#### Limit

If provided, a positive integer value specifying the maximum number of UUIDs to return.

#### Marker

If provided, a string value indicating that only UUIDs that occur after the marker value will be returned.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

Responses
---------

### Response Headers

This implementation of the operation uses only response headers that are common to most responses. See ../CommonResponseHeaders.

### Response Elements

On success, a JSON response will be returned with the following elements:

#### groups

An array of UUIDs - one for each group (excluding the root group) in the domain. If the "Marker" and/or "Limit" request parameters are used, a subset of the UUIDs may be returned.

#### hrefs

An array of hypertext links to related resources. See ../Hypermedia.

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

### Sample Request

``` sourceCode
GET /groups HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Accept-Encoding: gzip, deflate
Accept: */*
```

### Sample cURL command

``` sourceCode
$ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups
```

### Sample Response

``` sourceCode
HTTP/1.1 200 OK
Date: Thu, 12 Jul 2018 18:40:30 GMT
Content-Length: 443
Etag: "83575a7865761b6d4eaf5d285ab1de062c49250b"
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{
    "groups": [
        "g-be6eb652-83c5-11e8-b9ee-0242ac12000a",
        "g-be836c0a-83c5-11e8-947e-0242ac120014",
        "g-beaaa824-83c5-11e8-a8e6-0242ac120016",
        "g-beb56bba-83c5-11e8-87e1-0242ac12000c",
        "g-bf15f8b8-83c5-11e8-8ad9-0242ac120009"
    ],
    "hrefs": [
        {"href": "hsdshdflab.hdfgroup.org/groups", "rel": "self"},
        {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"},
        {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"}
    ]
}
```

### Sample Request with Marker and Limit

This example uses the "Marker" request parameter to return only UUIDs after the given Marker value. The "Limit" request parameter is used to limit the number of UUIDs in the response to 2.

``` sourceCode
GET /groups?Marker=g-be836c0a-83c5-11e8-947e-0242ac120014&Limit=2 HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Accept-Encoding: gzip, deflate
Accept: */*
```

### Sample cURL command

*URL enclosed in quotes to prevent shell from seeing ampersand*

``` sourceCode
$ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" "hsdshdflab.hdfgroup.org/groups?Marker=g-be836c0a-83c5-11e8-947e-0242ac120014&Limit=2"
```

### Sample Response with Marker and Limit

``` sourceCode
HTTP/1.1 200 OK
Date: Thu, 12 Jul 2018 18:52:35 GMT
Content-Length: 317
Etag: "49221af3436fdaca7e26c74b491ccf8698555f08"
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{
    "groups": [
        "g-beaaa824-83c5-11e8-a8e6-0242ac120016",
        "g-beb56bba-83c5-11e8-87e1-0242ac12000c"
    ],
    "hrefs": [
        {"href": "hsdshdflab.hdfgroup.org/groups", "rel": "self"},
        {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"},
        {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"}
    ]
}
```

Related Resources
-----------------

-   DELETE\_Group
-   GET\_Links
-   GET\_Group
-   POST\_Group

