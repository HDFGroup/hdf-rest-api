**********************************************
GET ACL
**********************************************

Description
===========
Returns access information for the given user for the object with the UUID provided in the URI.

Requests
========

Syntax
------

To get a user's default access for a domain:

.. code-block:: http

    GET /acls/<userid> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /acls/<userid>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get a user's access information for a group:

.. code-block:: http

    GET /groups/<id>/acls/<userid> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>   

.. code-block:: http

    GET /groups/<id>/acls/<userid>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get a user's access information for a dataset:

.. code-block:: http

    GET /datasets/<id>/acls/<userid> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datasets/<id>/acls/<userid>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get a user's access information for a committed datatype:

.. code-block:: http

    GET /datatypes/<id>/acls/<userid> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datatypes/<id>/acls/<userid>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

where:

* <id> is the UUID of the requested dataset/group/committed datatype
* <userid> is the userid for the requested user.  Use the special userid "default" to get the default access permisions for the object

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


acl
^^^
A JSON object that describes a user's access permissions.  Subkeys of acl are:

userName: the userid of the requested user

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

    GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/acls/default HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/acls/default

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Wed, 18 Jul 2018 16:21:21 GMT
    Content-Length: 408
    Etag: "2c410d1c469786f25ed0075571a8e7a3f313cec1"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "acl": {
            "userName": "default",
            "create": false,
            "update": false,
            "updateACL": false,
            "read": true,
            "readACL": false,
            "delete": false
        },
        "hrefs": [
            {"rel": "self", "href": "hsdshdflab.hdfgroup.org/acls"},
            {"rel": "root", "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016"},
            {"rel": "home", "href": "hsdshdflab.hdfgroup.org/"},
            {"rel": "owner", "href": "hsdshdflab.hdfgroup.org/"}
        ]
    }

Related Resources
=================

* :doc:`PUT_ACL`
* :doc:`GET_ACLs`



 