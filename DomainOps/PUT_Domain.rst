**********************************************
PUT Domain
**********************************************

Description
===========
This operation creates a new domain.

*Note*: Initially the only object contained in the domain is the root group.  Use other
PUT and POST operations to create new objects in the domain.

*Note*: The operation will fail if the domain already exists (a 409 code will be returned).

Requests
========

Syntax
------
.. code-block:: http

    PUT / HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

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

root
^^^^
The UUID of the root group of this domain.

created
^^^^^^^
A timestamp giving the time the domain was created in UTC (ISO-8601 format).

owner
^^^^^
The user which owns the domain.

lastModified
^^^^^^^^^^^^
A timestamp giving the most recent time that any content in the domain has been
modified in UTC (ISO-8601 format).

acls
^^^^
A JSON object representing the :doc:`../AclOps/index` values for the domain.

Special Errors
--------------

This implementation of the operation does not return any special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

An http status code of 409 (Conflict) will be returned if the domain already exists.

Examples
========

Sample Request
--------------

.. code-block:: http

    PUT / HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/newfile.h5
    Content-Length: 0
    Accept: */*
    Accept-Encoding: gzip, deflate

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/newfile.h5" hsdshdflab.hdfgroup.org/

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Thu, 12 Jul 2018 16:16:34 GMT
    Content-Length: 380
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "root": "g-a6915d1a-85ef-11e8-8659-0242ac12000c",
        "created": 1531412497.967022,
        "owner": "test_user1",
        "lastModified": 1531412497.967022,
        "acls": {
            "default": {"updateACL": false, "read": true, "delete": false, "update": false, "create": false, "readACL": false},
            "test_user1": {"updateACL": true, "read": true, "delete": true, "update": true, "create": true, "readACL": true}
        }
    }

Related Resources
=================

* :doc:`DELETE_Domain`
* :doc:`../GroupOps/GET_Group`
* :doc:`GET_Domain`
 

 