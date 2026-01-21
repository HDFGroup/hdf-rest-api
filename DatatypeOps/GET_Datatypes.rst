**********************************************
GET Datatypes
**********************************************

Description
===========
Gets all the committed datatypes in a domain.

Requests
========

Syntax
------
.. code-block:: http

    GET /datatypes HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datatypes?domain=DOMAIN HTTP/1.1
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

datatypes
^^^^^^^^^
An array of UUIDs, one for each datatype in the domain.

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

    GET /datatypes HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datatypes

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Fri, 13 Jul 2018 18:23:58 GMT
    Content-Length: 746
    Etag: "e01f56869a9a919b1496c463f3569a2a7c319f11"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "datatypes": [
            "t-3e37ab7e-86b3-11e8-bce3-0242ac12000c",
            "t-af48f618-86c9-11e8-9cb8-0242ac120008",
            "t-b0510adc-86c9-11e8-9361-0242ac120007",
            "t-b0e855b8-86c9-11e8-ac22-0242ac12000d",
            "t-b17d24fe-86c9-11e8-b364-0242ac12000a",
            "t-b21dd3ea-86c9-11e8-9cb8-0242ac120008",
            "t-b2b2a380-86c9-11e8-9361-0242ac120007",
            "t-b34f3542-86c9-11e8-ac22-0242ac12000d",
            "t-b4659570-86c9-11e8-b364-0242ac12000a",
            "t-b4f32ffc-86c9-11e8-9cb8-0242ac120008",
            "t-b58f2b50-86c9-11e8-9361-0242ac120007",
            "t-b66113c2-86c9-11e8-ac22-0242ac12000d",
            "t-b6f8e454-86c9-11e8-b364-0242ac12000a"
        ],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/datatypes", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008", "rel": "root"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"}
        ]
    }

Sample Request with Marker and Limit
------------------------------------

This example uses the "Marker" request parameter to return only UUIDs after the given
Marker value.
Also, the "Limit" request parameter is used to limit the number of UUIDs in the response to 5.

.. code-block:: http

    GET /datatypes?Marker=t-b17d24fe-86c9-11e8-b364-0242ac12000a&Limit=5 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

*URL enclosed in quotes to prevent shell from seeing ampersand*

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" "hsdshdflab.hdfgroup.org/datatypes?Marker=t-b17d24fe-86c9-11e8-b364-0242ac12000a&Limit=5"

Sample Response with Marker and Limit
-------------------------------------

 .. code-block:: http

    HTTP/1.1 200 OK
    Date: Fri, 13 Jul 2018 18:30:29 GMT
    Content-Length: 410
    Etag: "a2e2d5a3ae63cd504d02b51d99f27b30d17b75b5"
    Content-Type: application/json
    Server: nginx/1.15.0

 .. code-block:: json

    {
        "datatypes": [
            "t-b21dd3ea-86c9-11e8-9cb8-0242ac120008",
            "t-b2b2a380-86c9-11e8-9361-0242ac120007",
            "t-b34f3542-86c9-11e8-ac22-0242ac12000d",
            "t-b4659570-86c9-11e8-b364-0242ac12000a",
            "t-b4f32ffc-86c9-11e8-9cb8-0242ac120008"
        ],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/datatypes", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008", "rel": "root"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"}
        ]
    }


Related Resources
=================

* :doc:`DELETE_Datatype`
* :doc:`GET_Datatype`
* :doc:`POST_Datatype`
* :doc:`../DatasetOps/POST_Dataset`
* :doc:`../AttrOps/PUT_Attribute`


 