POST Group
==========

Description
-----------

Creates a new Group.

*Note:* By default the new Group will not be linked from any other group in the domain. A link element can be included in the request body to have an existing group link to the new group. Alternatively, use the *PUT link* operation to link the new group.

Requests
--------

### Syntax

``` sourceCode
POST /groups HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
POST /groups?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

### Request Parameters

This implementation of the operation does not use request parameters.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

### Request Elements

Optionally the request body can be a JSON object that has a link key with sub-keys:

#### id

The UUID of the group that the new group should be linked to. If the UUID is not valid, the request will fail and a new group will not be created.

#### name

The name of the new link.

Responses
---------

### Response Headers

This implementation of the operation uses only response headers that are common to most responses. See ../CommonResponseHeaders.

### Response Elements

On success, a JSON response will be returned with the following elements:

#### id

The UUID of the newly created group.

#### root

The root group of the domain which the group was created in.

#### attributeCount

The number of attributes belonging to the group.

#### linkCount

The number of links belonging to the group.

#### created

A timestamp giving the time the group was created in UTC (ISO-8601 format).

#### lastModified

A timestamp giving the most recent time the group has been modified (i.e. attributes or links updated) in UTC (ISO-8601 format).

### Special Errors

This implementation of the operation does not return special errors. For general information on standard error codes, see ../CommonErrorResponses.

Examples
--------

### Sample Request

Create a new, un-linked Group.

``` sourceCode
POST /groups HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Content-Length: 0
Accept: */*
Accept-Encoding: gzip, deflate
```

### Sample cURL command

``` sourceCode
$ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups
```

### Sample Response

``` sourceCode
HTTP/1.1 201 Created
Content-Length: 202
Server: nginx/1.15.0
Date: Thu, 12 Jul 2018 16:49:10 GMT
Content-Type: application/json
```

``` sourceCode
{
    "id": "g-7fbbf52a-85f3-11e8-9cc2-0242ac120008",
    "root": "g-b116b6f0-85e9-11e8-9cc2-0242ac120008",
    "created": 1531414150.1522243,
    "lastModified": 1531414150.1522243,
    "linkCount": 0,
    "attributeCount": 0
}
```

### Sample Request with Link

Create a new Group, link to root (which has uuid of "g-b116b6f0-...") as "linked\_group".

``` sourceCode
POST /groups HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Content-Length: 82
Accept: */*
Accept-Encoding: gzip, deflate
```

``` sourceCode
{
    "link": {
        "id": "g-b116b6f0-85e9-11e8-9cc2-0242ac120008",
        "name": "linked_group"
    }
}
```

### Sample cURL command

``` sourceCode
$ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5"
  -d "{\"link\": {\"id\": \"g-b116b6f0-85e9-11e8-9cc2-0242ac120008\", \"name\": \"linked_group\"}}" hsdshdflab.hdfgroup.org/groups
```

### Sample Response with Link

``` sourceCode
HTTP/1.1 201 Created
Content-Length: 200
Server: nginx/1.15.0
Date: Thu, 12 Jul 2018 16:57:57 GMT
Content-Type: application/json
```

``` sourceCode
{
    "id": "g-b9bd362a-85f4-11e8-a549-0242ac12000b",
    "root": "g-b116b6f0-85e9-11e8-9cc2-0242ac120008",
    "linkCount": 0,
    "attributeCount": 0,
    "lastModified": 1531414676.963812,
    "created": 1531414676.963812
}
```

Related Resources
-----------------

-   DELETE\_Group
-   GET\_Links
-   PUT\_Link
-   GET\_Group
-   GET\_Groups

