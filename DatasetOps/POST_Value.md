POST Value
==========

Description
-----------

Gets values of a dataset for a given point selection (provided in the body of the request).

Requests
--------

### Syntax

``` sourceCode
POST /datasets/<id>/value HTTP/1.1
Host: DOMAIN
Authorization: <authorization_string>
```

*&lt;id&gt;* is the UUID of the requested dataset t

### Request Parameters

This implementation of the operation does not use request parameters.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

### Request Body

The request body should be a JSON object with the following key:

#### points

An array of points defining the selection. Each point can either be an integer (if the dataset has just one dimension), or an array where the length of the array is equal to the number of dimensions of the dataset.

Responses
---------

### Response Headers

This implementation of the operation uses only response headers that are common to most responses. See ../CommonResponseHeaders.

### Response Elements

On success, a JSON response will be returned with the following elements:

#### value

An array of values where the length of the array is equal to the number of points in the request. Each value will be a string, integer, or JSON object consistent with the dataset type (e.g. a compound type).

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

### Sample Request

``` sourceCode
POST /datasets/4e83ad1c-ab6e-11e4-babb-3c15c2da029e/value HTTP/1.1
Content-Length: 92
User-Agent: python-requests/2.3.0 CPython/2.7.8 Darwin/14.0.0
host: tall.test.hdfgroup.org
Accept: */*
Accept-Encoding: gzip, deflate
```

``` sourceCode
{
"points": [19, 17, 13, 11, 7, 5, 3, 2]
}
```

### Sample Response

``` sourceCode
HTTP/1.1 200 OK
Date: Tue, 03 Feb 2015 06:31:38 GMT
Content-Length: 47
Content-Type: application/json
Server: TornadoServer/3.2.2
```

``` sourceCode
{
"value": [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
}
```

Related Resources
-----------------

-   GET\_Dataset
-   GET\_Value
-   PUT\_Value

