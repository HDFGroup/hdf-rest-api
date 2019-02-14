PUT ACL
=======

Description
-----------

Update the access information for the given user for the object with the UUID provided in the URI.

Requests
--------

### Syntax

To update a user's access information for a domain:

``` sourceCode
PUT /acls/<userid> HTTP/1.1
Host: DOMAIN
Authorization: <authorization_string>
```

To update a user's access information for a group:

``` sourceCode
PUT /groups/<id>/acls/<userid> HTTP/1.1
Host: DOMAIN
Authorization: <authorization_string>
```

To get a user's access information for a dataset:

``` sourceCode
PUT /datasets/<id>/acls/<userid> HTTP/1.1
Host: DOMAIN
Authorization: <authorization_string>
```

To get a user's access information for a committed datatype:

``` sourceCode
PUT /datatypes/<id>/acls/<userid> HTTP/1.1
Host: DOMAIN
Authorization: <authorization_string>
```

where:

-   &lt;id&gt; is the UUID of the requested dataset/group/committed datatype
-   &lt;userid&gt; is the userid for the requested user. Use the special userid "default" to get the default access permisions for the object

### Request Parameters

This implementation of the operation does not use request parameters.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

### Request Elements

The request body most include a JSON object that has the following keys and boolean values:

> { 'read': &lt;True or False&gt;,
>
> 'create': &lt;True or False&gt;,
>
> 'update': &lt;True or False&gt;,
>
> 'delete': &lt;True or False&gt;,
>
> 'readACL': &lt;True or False&gt;,
>
> 'updateACL': &lt;True or False&gt;
>
> }

Responses
---------

### Response Headers

This implementation of the operation uses only response headers that are common to most responses. See ../CommonResponseHeaders.

### Response Elements

On success, a JSON response will be returned with the following elements:

#### hrefs

An array of hypertext links to related resources. See ../Hypermedia.

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

### Sample Request

``` sourceCode
PUT /groups/052dcbbd-9d33-11e4-86ce-3c15c2da029e/acls/test_user1 HTTP/1.1
host: tall.test.hdfgroup.org
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.3.0 CPython/2.7.8 Darwin/14.0.0

{ 'read': True, 'create': False, 'update': False, 
         'delete': False, 'readACL': False, 'updateACL': False }
```

### Sample Response

``` sourceCode
HTTP/1.1 201 Created
Date: Fri, 16 Jan 2015 20:06:08 GMT
Content-Length: 660
Etag: "2c410d1c469786f25ed0075571a8e7a3f313cec1"
Content-Type: application/json
Server: TornadoServer/3.2.2
```

``` sourceCode
"hrefs": [
    {
        "href": "http://tall_acl.test.hdfgroup.org/groups/eb8f6959-8775-11e5-96b6-3c15c2da029e/acls/test_user1",
        "rel": "self"
    },
    {
        "href": "http://tall_acl.test.hdfgroup.org/groups/eb8f6959-8775-11e5-96b6-3c15c2da029e",
        "rel": "root"
    },
    {
        "href": "http://tall_acl.test.hdfgroup.org/",
        "rel": "home"
    },
    {
        "href": "http://tall_acl.test.hdfgroup.org/groups/eb8f6959-8775-11e5-96b6-3c15c2da029e",
        "rel": "owner"
    }
]
```

Related Resources
-----------------

-   GET\_ACL
-   GET\_ACLs

