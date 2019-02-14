PUT Link
========

Description
-----------

Creates a new link in a given group.

Either hard, soft, or external links can be created based on the request elements. See examples below.

*Note:* any existing link with the same name will be replaced with the new link.

Requests
--------

### Syntax

``` sourceCode
PUT /groups/<id>/links/<name> HTTP/1.1
X-Hdf-domain: DOMAIN
Authorization: <authorization_string>
```

``` sourceCode
PUT /groups/<id>/links/<name>?domain=DOMAIN HTTP/1.1
Authorization: <authorization_string>
```

-   *&lt;id&gt;* is the UUID of the group that the link will be created in.
-   *&lt;name&gt;* is the URL-encoded name of the link.

### Request Parameters

This implementation of the operation does not use request parameters.

### Request Headers

This implementation of the operation uses only the request headers that are common to most requests. See ../CommonRequestHeaders

### Request Elements

The request body must include a JSON object that has the following keys:

#### id

For Hard links - The UUID of the object to link to. If the UUID is not valid, the request will fail and a new link will not be created. If this key is present, the h5path and h5domain keys will be ignored.

#### h5path

For Soft and External links - A string describing a path to an external resource. If this key is present a soft or external link will be created.

#### h5domain

For External links - A string giving the external domain where the resource is present. If this key is present, the h5path key must be provided as well.

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

### Sample Request - Create Hard Link

In group "g-b116b6f0-...", create a hard link named "g3" that points to the object with UUID "g-b9bd362a-...".

``` sourceCode
PUT /groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008/links/g3 HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Content-Length: 48
Content-Type: application/json
Accept: */*
Accept-Encoding: gzip, deflate
```

``` sourceCode
{"id": "g-b9bd362a-85f4-11e8-a549-0242ac12000b"}
```

### Sample cURL command

``` sourceCode
$ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
  -d "{\"id\": \"g-b9bd362a-85f4-11e8-a549-0242ac12000b\"}" hsdshdflab.hdfgroup.org/groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008/links/g3
```

### Sample Response - Create Hard Link

``` sourceCode
HTTP/1.1 201 Created
Date: Thu, 12 Jul 2018 19:25:16 GMT
Content-Length: 13
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{"hrefs": []}
```

### Sample Request - Create Soft Link

In group "g-b116b6f0-...", create a soft link named "softlink" that contains the path "/somewhere".

``` sourceCode
PUT /groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008/links/softlink HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Content-Length: 24
Accept: */*
Accept-Encoding: gzip, deflate
```

``` sourceCode
{"h5path": "/somewhere"}
```

### Sample cURL command

``` sourceCode
$ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
  -d "{\"h5path\": \"/somewhere\"}" hsdshdflab.hdfgroup.org/groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008/links/softlink
```

### Sample Response - Create Soft Link

``` sourceCode
HTTP/1.1 201 Created
Date: Thu, 12 Jul 2018 19:30:28 GMT
Content-Length: 13
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{"hrefs": []}
```

### Sample Request - Create External Link

In group "g-b116b6f0-...", create an external link named "extlink" that references the object at path: "/dset1" in domain: "/shared/ext\_file.h5".

``` sourceCode
PUT /groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008/links/extlink HTTP/1.1
Host: hsdshdflab.hdfgroup.org
X-Hdf-domain: /shared/tall.h5
Content-Length: 51
Accept: */*
Accept-Encoding: gzip, deflate
```

``` sourceCode
{"h5domain": "/shared/ext_file.h5", "h5path": "/dset1"}
```

### Sample cURL command

``` sourceCode
$ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
  -d "{\"h5domain\": \"/shared/ext_file.h5\", \"h5path\": \"/dset1\"}" hsdshdflab.hdfgroup.org/groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008/links/extlink
```

### Sample Response - Create External Link

``` sourceCode
HTTP/1.1 201 Created
Date: Thu, 12 Jul 2018 19:34:57 GMT
Content-Length: 13
Content-Type: application/json
Server: nginx/1.15.0
```

``` sourceCode
{"hrefs": []}
```

Related Resources
-----------------

-   DELETE\_Link
-   GET\_Link
-   GET\_Links
-   GET\_Group

