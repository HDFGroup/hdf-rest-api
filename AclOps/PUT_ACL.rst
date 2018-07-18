**********************************************
PUT ACL
**********************************************

Description
===========
Update the access information for the given user for the object with the UUID provided in the URI.

Requests
========

Syntax
------

To update a user's access information for a domain:

.. code-block:: http

    PUT /acls/<userid> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /acls/<userid>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To update a user's access information for a group:

.. code-block:: http

    PUT /groups/<id>/acls/<userid> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /groups/<id>/acls/<userid>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To update a user's access information for a dataset:

.. code-block:: http

    PUT /datasets/<id>/acls/<userid> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /datasets/<id>/acls/<userid>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To update a user's access information for a committed datatype:

.. code-block:: http

    PUT /datatypes/<id>/acls/<userid> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /datatypes/<id>/acls/<userid>?domain=DOMAIN HTTP/1.1
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

Request Elements
----------------

The request body most include a JSON object that has the following keys and boolean values:

.. code-block:: json

    { 
        "read": <true or false>, 
        "create": <true or false>, 
        "update": <true or false>, 
        "delete": <true or false>, 
        "readACL": <true or false>, 
        "updateACL": <true or false> 
    }

Responses
=========

Response Headers
----------------

This implementation of the operation uses only response headers that are common to 
most responses.  See :doc:`../CommonResponseHeaders`.

Response Elements
-----------------

On success, a JSON response will be returned with the following elements:

 
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

    PUT /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/acls/test_user1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

.. code-block:: json

    {
        "read": true,
        "create": false,
        "update": false, 
        "delete": false,
        "readACL": false,
        "updateACL": false
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5"
      -d "{\"read\": true, \"create\": false, \"update\": false, \"delete\": false, \"readACL\": false, \"updateACL\": false}"
      hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/acls/test_user1

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Wed, 18 Jul 2018 16:06:13 GMT
    Content-Length: 660
    Etag: "2c410d1c469786f25ed0075571a8e7a3f313cec1"
    Content-Type: application/json
    Server: nginx/1.15.0

Related Resources
=================

* :doc:`GET_ACL`
* :doc:`GET_ACLs`



 