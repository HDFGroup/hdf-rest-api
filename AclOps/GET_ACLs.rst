**********************************************
GET ACLs
**********************************************

Description
===========
Returns access information for all users defined in the ACL (Access Control List) 
for the object with the UUID provided in the URI.

Requests
========

Syntax
------

To get the ACL for a domain:

.. code-block:: http

    GET /acls HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /acls?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get the ACL for a group:

.. code-block:: http

    GET /groups/<id>/acls HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /groups/<id>/acls?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get the ACL for a dataset:

.. code-block:: http

    GET /datasets/<id>/acls HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datasets/<id>/acls?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get the ACL for a committed datatype:

.. code-block:: http

    GET /datatypes/<id>/acls HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datatypes/<id>/acls?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

where:

* <id> is the UUID of the requested dataset/group/committed datatype

Request Parameters
------------------
This implementation of the operation does not use request parameters.

Request Headers
---------------
This implementation of the operation uses only the request headers that are common
to most requests.  See :doc:`../CommonRequestHeaders`

Responses
=========

Response Headers
----------------

This implementation of the operation uses only response headers that are common to 
most responses.  See :doc:`../CommonResponseHeaders`.

Response Elements
-----------------

On success, a JSON response will be returned with the following elements:


acls
^^^^
A JSON list that contains one element for each user specified in the ACL.
The elements will be JSON objects that describe each user's access permissions.  
The subkeys of each element are:

userName: the userid of the user ('default' for the default access)

create: A boolean flag that indicates if the user is authorized to create new resources

delete: A boolean flag that indicates if the user is authorized to delete resources

read: A boolean flag that indicates if the user is authorized to read (GET) resources

update: A boolean flag that indicates if the user is authorized to update resources

readACL: A boolean flag that indicates if the user is authorized to read the object's ACL

updateACL: A boolean flag that indicates if the user is authorized to update the object's ACL

 
hrefs
^^^^^
An array of hypertext links to related resources.  See :doc:`../Hypermedia`.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

.. code-block:: http

    GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/acls  HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/acls

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Wed, 18 Jul 2018 16:30:29 GMT
    Content-Length: 701
    Etag: "2c410d1c469786f25ed0075571a8e7a3f313cec1"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "acls": [
            {
                "create": false,
                "read": true,
                "readACL": false,
                "userName": "default",
                "delete": false,
                "update": false,
                "updateACL": false
            },
            {
                "create": true,
                "domain": "/shared/tall.h5",
                "read": true,
                "readACL": true,
                "userName": "test_user1",
                "update": true,
                "delete": true,
                "updateACL": true
            },
            {
                "create": false,
                "domain": "/shared/tall.h5",
                "read": true,
                "readACL": false,
                "userName": "test_user2", 
                "update": false,
                "delete": false,
                "updateACL": false
            }
        ],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/acls", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "owner"}
        ]
    }

Related Resources
=================

* :doc:`PUT_ACL`
* :doc:`GET_ACL`



 