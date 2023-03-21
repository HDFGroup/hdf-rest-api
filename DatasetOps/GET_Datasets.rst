**********************************************
GET Datasets
**********************************************

Description
===========
Returns UUIDs for all the datasets in a domain.

Requests
========

Syntax
------
.. code-block:: http

    GET /datasets HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datasets?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

Request Parameters
------------------
This implementation of the operation uses the following request parameters (both 
optional):

Limit
^^^^^
If provided, a positive integer value specifying the maximum number of UUIDs to return.

Marker
^^^^^^
If provided, a string value indicating that only UUIDs that occur after the
marker value will be returned.

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

datasets
^^^^^^^^
An array of UUIDs, one for each dataset in the domain.

hrefs
^^^^^
An array of links to related resources.  See :doc:`../Hypermedia`.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

.. code-block:: http

    GET /datasets HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datasets

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 16:29:14 GMT
    Content-Length: 405
    Etag: "977e96c7bc63a6e05d10d56565df2ab8d30e404d"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "datasets": [
            "d-be8bace4-83c5-11e8-90e7-0242ac120013",
            "d-be9c3582-83c5-11e8-947e-0242ac120014",
            "d-bf1cb98c-83c5-11e8-b9ee-0242ac12000a",
            "d-bf2af63c-83c5-11e8-87e1-0242ac12000c"
        ],
        "hrefs": [
            {"rel": "self", "href": "hsdshdflab.hdfgroup.org/datasets"},
            {"rel": "root", "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016"},
            {"rel": "home", "href": "hsdshdflab.hdfgroup.org/"}
        ]
    }

Sample Request with Marker and Limit
------------------------------------

This example uses the "Marker" request parameter to return only UUIDs after the given
Marker value.
The "Limit" request parameter is used to limit the number of UUIDs in the response to 5.

.. code-block:: http

    GET /datasets?Marker=d-85641798-8b73-11e8-bad6-0242ac120009&Limit=5 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/many_datasets.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

*URL enclosed in quotes to prevent shell from seeing ampersand*

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/many_datasets.h5" "hsdshdflab.hdfgroup.org/datasets?Marker=d-85641798-8b73-11e8-bad6-0242ac120009&Limit=5"

Sample Response with Marker and Limit
-------------------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 16:51:42 GMT
    Content-Length: 408
    Etag: "cb708d4839cc1e165fe6bb30718e49589ef140f4"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "datasets": [
            "d-8652ac0a-8b73-11e8-827b-0242ac120007",
            "d-875c8206-8b73-11e8-a0b6-0242ac12000e",
            "d-88567b8a-8b73-11e8-9d44-0242ac12000b",
            "d-8908c7cc-8b73-11e8-bad6-0242ac120009",
            "d-89a73f9c-8b73-11e8-827b-0242ac120007"
        ],
        "hrefs": [
            {"href": "hsds.local/datasets", "rel": "self"},
            {"href": "hsds.local/groups/g-0e8ddffa-8b73-11e8-a0b6-0242ac12000e", "rel": "root"},
            {"href": "hsds.local/", "rel": "home"}
        ]
    }

Related Resources
=================

* :doc:`DELETE_Dataset`
* :doc:`GET_Dataset`
* :doc:`POST_Dataset`


