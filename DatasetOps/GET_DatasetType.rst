**********************************************
GET Type
**********************************************

Description
===========
Gets Type Information for a dataset.

Requests
========

Syntax
------
.. code-block:: http

    GET /datasets/<id>/type HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datasets/<id>/type?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

*<id>* is the UUID of the dataset the type information is requested for.

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

type
^^^^
A JSON object representing the type definition for the dataset. See :doc:`../Types/index`
for information on how different types are represented.

hrefs
^^^^^
An array of links to related resources.  See :doc:`../Hypermedia`.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request - Predefined Type
--------------------------------

.. code-block:: http

    GET /datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/type HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/type

Sample Response - Predefined Type
---------------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 16:04:48 GMT
    Content-Length: 374
    Etag: "802b160bf786596a9cb9f6d5cd6faa4fe1127e8c"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "type": {
            "base": "H5T_STD_I32BE",
            "class": "H5T_INTEGER"
        },
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/type", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013", "rel": "owner"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"}
        ]
    }

Sample Request - Compound Type
--------------------------------

.. code-block:: http

    GET /datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/type HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /home/test_user1/h5pyd_test/3.4/query_compound_dset.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /home/test_user1/h5pyd_test/3.4/query_compound_dset.h5"
      hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/type

Sample Response - Compound Type
--------------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 16:08:38 GMT
    Content-Length: 763
    Etag: "1f97eac24aa18d3c462a2f2797c4782a1f2a0aa2"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "type": {
            "class": "H5T_COMPOUND",
            "fields": [
                {
                    "type": {
                        "strPad": "H5T_STR_NULLPAD",
                        "length": 4,
                        "class": "H5T_STRING",
                        "charSet": "H5T_CSET_ASCII"
                    },
                    "name": "symbol"
                },
                {
                    "type": {
                        "strPad": "H5T_STR_NULLPAD",
                        "length": 8,
                        "class": "H5T_STRING",
                        "charSet": "H5T_CSET_ASCII"
                    },
                    "name": "date"
                },
                {
                    "type": {
                        "class": "H5T_INTEGER",
                        "base": "H5T_STD_I32LE"
                    },
                    "name": "open"
                },
                {
                    "type": {
                        "class": "H5T_INTEGER",
                        "base": "H5T_STD_I32LE"
                    },
                    "name": "close"
                }
            ]
        },
        "hrefs": [
            {"rel": "self", "href": "hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/type"},
            {"rel": "owner", "href": "hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014"},
            {"rel": "root", "href": "hsdshdflab.hdfgroup.org/groups/g-a6b9f118-807b-11e8-a81a-0242ac12000b"}
        ]
    }

Related Resources
=================

* :doc:`GET_Dataset`
* :doc:`GET_DatasetShape`
* :doc:`POST_Dataset`


