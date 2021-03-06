PUT Shape
=========

Description
-----------

Modifies the dimensions of a dataset. Dimensions can only be changed if the dataset was initially created with that dimension as *extensible* - i.e. the maxdims value for that dimension is larger than the initial dimension size (or maxdims set to 0).

*Note:* Dimensions can only be made larger, they can not be reduced.

Requests
--------

### Syntax

``` sourceCode
PUT /datasets/<id>/shape HTTP/1.1
Host: DOMAIN
Authorization: <authorization_string>
```

*&lt;id&gt;* is the UUID of the dataset whose shape will be modified.

### Request Parameters

This implementation of the operation does not use request parameters.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

### Request Elements

The request body must include a JSON object with a "shape" key as described below:

#### shape

An integer array giving the new dimensions of the dataset.

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
PUT /datasets/b9b6acc0-a839-11e4-aa86-3c15c2da029e/shape HTTP/1.1
Content-Length: 19
User-Agent: python-requests/2.3.0 CPython/2.7.8 Darwin/14.0.0
host: resized.test.hdfgroup.org
Accept: */*
Accept-Encoding: gzip, deflate
```

``` sourceCode
{
"shape": [10, 25]
}
```

### Sample Response

``` sourceCode
HTTP/1.1 201 Created
Date: Fri, 30 Jan 2015 04:47:47 GMT
Content-Length: 331
Content-Type: application/json
Server: TornadoServer/3.2.2   
```

``` sourceCode
{
"hrefs": [
    {"href": "http://resized.test.hdfgroup.org/datasets/22e1b235-a83b-11e4-97f4-3c15c2da029e", "rel": "self"}, 
    {"href": "http://resized.test.hdfgroup.org/datasets/22e1b235-a83b-11e4-97f4-3c15c2da029e", "rel": "owner"}, 
    {"href": "http://resized.test.hdfgroup.org/groups/22dfff8f-a83b-11e4-883d-3c15c2da029e", "rel": "root"}
  ]
}
```

Related Resources
-----------------

-   GET\_Dataset
-   GET\_DatasetShape
-   GET\_Value
-   POST\_Value
-   PUT\_Value

