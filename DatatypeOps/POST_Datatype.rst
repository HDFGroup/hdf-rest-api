**********************************************
POST Datatype
**********************************************

Description
===========
Creates a new committed datatype.

Requests
========

Syntax
------
.. code-block:: http

    POST /datatypes  HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    POST /datatypes?domain=DOMAIN  HTTP/1.1
    Authorization: <authorization_string>

Request Parameters
------------------
This implementation of the operation does not use request parameters.

Request Headers
---------------
This implementation of the operation uses only the request headers that are common
to most requests.  See :doc:`../CommonRequestHeaders`

Request Elements
----------------
The request body must be a JSON object with a 'type' link key as described below.
Optionally, the request body can include a 'link' key that describes how the new
committed datatype will be linked.

type
^^^^
The value of the type key can either be one of the predefined type strings 
(see predefined types), or a JSON representation of a type. (see :doc:`../Types/index`).

link
^^^^
If present, the link value must include the following subkeys:

link['id']
^^^^^^^^^^
The UUID of the group the new datatype should be linked from.  If the UUID is not valid,
the request will fail and a new datatype will not be created.

link['name']
^^^^^^^^^^^^
The name of the new link.

Responses
=========

Response Headers
----------------

This implementation of the operation uses only response headers that are common to 
most responses.  See :doc:`../CommonResponseHeaders`.

Response Elements
-----------------

On success, a JSON response will be returned with the following elements:

id
^^
The UUID of the newly created datatype object.

root
^^^^
The root group of the domain which the datatype is within.

attributeCount
^^^^^^^^^^^^^^
The number of attributes belonging to the datatype.

created
^^^^^^^
A timestamp giving the time the datatype was created in UTC (ISO-8601 format).

lastModified
^^^^^^^^^^^^
A timestamp giving the most recent time the datatype has been modified (i.e. attributes or 
links updated) in UTC (ISO-8601 format).

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

Create a new committed datatype using the "H5T_IEEE_F32LE" (32-bit float) predefined type.

.. code-block:: http

    POST /datatypes HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 26
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "type": "H5T_IEEE_F32LE"
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"type\": \"H5T_IEEE_F32LE\"}" hsdshdflab.hdfgroup.org/datatypes

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Fri, 13 Jul 2018 15:35:49 GMT
    Content-Length: 186
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "id": "t-6b0bdf9a-86b2-11e8-89f2-0242ac120009",
        "created": 1531496149.3141127,
        "root": "g-b116b6f0-85e9-11e8-9cc2-0242ac120008",
        "lastModified": 1531496149.3141127,
        "attributeCount": 0
    }

Sample Request with Link
------------------------

Create a new committed datatype and link to root (id "g-b116b6f0-...") as "linked_dtype".

.. code-block:: http

    POST /datatypes HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 108
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "type": "H5T_IEEE_F64LE",
        "link": {
            "id": "g-b116b6f0-85e9-11e8-9cc2-0242ac120008", 
            "name": "linked_dtype"
        }
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"type\": \"H5T_IEEE_F64LE\", \"link\": {\"id\": \"g-b116b6f0-85e9-11e8-9cc2-0242ac120008\", \"name\": \"linked_dtype\"}}" hsdshdflab.hdfgroup.org/datatypes

Sample Response with Link
-------------------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Fri, 13 Jul 2018 15:41:44 GMT
    Content-Length: 186
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "id": "t-3e37ab7e-86b3-11e8-bce3-0242ac12000c",
        "root": "g-b116b6f0-85e9-11e8-9cc2-0242ac120008",
        "attributeCount": 0,
        "created": 1531496503.6064572,
        "lastModified": 1531496503.6064572
    }

Related Resources
=================

* :doc:`DELETE_Datatype`
* :doc:`GET_Datatype`
* :doc:`GET_Datatypes`
* :doc:`../DatasetOps/POST_Dataset`
* :doc:`../AttrOps/PUT_Attribute`


 