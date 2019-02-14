GET Attributes
==============

Description
-----------

Gets all the attributes of a dataset, group, or committed datatype. For each attribute the request returns the attribute's name, type, and shape. To get the attribute data use GET\_Attribute.

Requests
--------

### Syntax

To get the attributes of a group:

``` sourceCode
GET /groups/<id>/attributes HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
GET /groups/<id>/attributes?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

To get the attributes of a dataset:

``` sourceCode
GET /datasets/<id>/attributes HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
GET /datasets/<id>/attributes?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

To get the attributes of a datatype:

``` sourceCode
GET /datatypes/<id>/attributes HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
GET /datatypes/<id>/attributes?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

where:

-   *&lt;id&gt;* is the UUID of the dataset/group/committed datatype

### Request Parameters

This implementation of the operation uses the following request parameters (both optional):

#### Limit

If provided, a positive integer value specifying the maximum number of attributes to return.

#### Marker

If provided, a string value indicating that only attributes that occur after the marker value will be returned. *Note:* the marker expression should be url-encoded.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

Responses
---------

### Response Headers

This implementation of the operation uses only response headers that are common to most responses. See ../CommonResponseHeaders.

### Response Elements

On success, a JSON response will be returned with the following elements:

#### attributes

An array of JSON objects with an element for each returned attribute. Each element will have keys: name, type, shape, created, and lastModified. See GET\_Attribute for a description of these keys.

#### hrefs

An array of links to related resources. See ../Hypermedia.

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

### Sample Request

Get attributes of a group with UUID: "1a956e54-...".

``` sourceCode
GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Accept-Encoding: gzip, deflate
Accept: */*
```

### Sample cURL command

``` sourceCode
$ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes
```

### Sample Response

``` sourceCode
HTTP/1.1 200 OK
Date: Sun, 15 Jul 2018 16:23:43 GMT
Content-Length: 797
Etag: "7cbeefcf8d9997a8865bdea3bf2d541a14e9bf71"
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{
    "attributes": [
        {
            "type": {
                "base": "H5T_STD_I8LE",
                "class": "H5T_INTEGER"
            },
            "name": "attr1",
            "shape": {
                "dims": [10],
                "class": "H5S_SIMPLE"
            },
            "created": 1531174596.117736,
            "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes/attr1"
        },
        {
            "type": {
                "base": "H5T_STD_I32BE",
                "class": "H5T_INTEGER"
            },
            "name": "attr2",
            "shape": {
                "dims": [2, 2],
                "class": "H5S_SIMPLE"
            },
            "created": 1531174596.141592,
            "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes/attr2"
        }
    ],
    "hrefs": [
        {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes", "rel": "self"},
        {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
        {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "owner"}
    ]
}
```

### Sample Request - get Batch

Get the five attributes that occur after attribute "attr2" from a group with UUID: "g-45f464d8-...".

``` sourceCode
GET /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes?Marker=attr2&Limit=5 HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Accept-Encoding: gzip, deflate
Accept: */*
```

### Sample cURL command

``` sourceCode
$ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes?Marker=attr2&Limit=5"
```

### Sample Response - get Batch

``` sourceCode
HTTP/1.1 200 OK
Date: Sun, 15 Jul 2018 16:38:16 GMT
Content-Length: 1767
Etag: "9483f4356e08d12b719aa64ece09e659b05adaf2"
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{
    "attributes": [
        {
            "name": "attr3",
            "type": {
                "base": "H5T_STD_U32BE",
                "class": "H5T_INTEGER"
            },
            "shape": {
                "class": "H5S_SCALAR"
            },
            "created": 1531672545.5978162,
            "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr3"
        },
        {
            "name": "attr4",
            "type": {
                "base": "H5T_STD_I32LE",
                "class": "H5T_INTEGER"
            },
            "shape": {
                "class": "H5S_SCALAR"
            },
            "created": 1531667223.0914037,
            "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr4"
        },
        {
            "name": "attr5",
            "type": {
                "base": "H5T_STD_U64LE",
                "class": "H5T_INTEGER"
            },
            "shape": {
                "class": "H5S_SCALAR"
            },
            "created": 1531672562.6758137,
            "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr5"
        },
        {
            "name": "attr6",
            "type": {
                "strPad": "H5T_STR_NULLPAD",
                "charSet": "H5T_CSET_ASCII",
                "class": "H5T_STRING",
                "length": 40
            },
            "shape": {
                "class": "H5S_SIMPLE",
                "dims": [2]
            },
            "created": 1531668943.5116098,
            "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr6"
        },
        {
            "name": "attr7",
            "type": {
                "base": "H5T_STD_U64LE",
                "class": "H5T_INTEGER"
            },
            "shape": {
                "class": "H5S_SCALAR"
            },
            "created": 1531672573.915442,
            "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr7"
        }
    ],
    "hrefs": [
        {"href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes", "rel": "self"},
        {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
        {"href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e", "rel": "owner"}
    ]
}
```

Related Resources
-----------------

-   DELETE\_Attribute
-   GET\_Attributes
-   ../DatasetOps/GET\_Dataset
-   ../DatatypeOps/GET\_Datatype
-   ../GroupOps/GET\_Group
-   PUT\_Attribute

