**********************************************
GET Dataset
**********************************************

Description
===========
Returns information about the dataset with the UUID given in the URI.

Requests
========

Syntax
------
.. code-block:: http

    GET /datasets/<id> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datasets/<id>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the requested dataset.

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

id
^^
The UUID of the dataset object.

root
^^^^
The UUID of the root group for the domain which the dataset is within.

type
^^^^
A JSON object representing the type of the dataset.  See :doc:`../Types/index` for 
details of the type representation.

shape
^^^^^
A JSON object representing the shape of the dataset.  See :doc:`GET_DatasetShape` for
details of the shape representation.

creationProperties
^^^^^^^^^^^^^^^^^^
A JSON object that describes chunk layout, filters, fill value, and other aspects of the dataset.
See: http://hdf5-json.readthedocs.org/en/latest/bnf/dataset.html#grammar-token-dcpl for a complete 
description of fields that can be used.

attributeCount
^^^^^^^^^^^^^^
The number of attributes belonging to the dataset.

created
^^^^^^^
A timestamp giving the time the dataset was created in UTC (ISO-8601 format).

lastModified
^^^^^^^^^^^^
A timestamp giving the most recent time the group has been modified (i.e. attributes or 
links updated) in UTC (ISO-8601 format).

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

    GET /datasets/d-bf1cb98c-83c5-11e8-b9ee-0242ac12000a HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datasets/d-bf1cb98c-83c5-11e8-b9ee-0242ac12000a

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 16:14:29 GMT
    Content-Length: 966
    Etag: "ecbd7e52654b0a8f4ccbebac06175ce5df5f8c79"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "id": "d-bf1cb98c-83c5-11e8-b9ee-0242ac12000a",
        "root": "g-be5996fa-83c5-11e8-a8e6-0242ac120016",
        "layout": {
            "class": "H5D_CHUNKED",
            "dims": [10]
        },
        "creationProperties": {
            "fillTime": "H5D_FILL_TIME_ALLOC",
            "layout": {
                "class": "H5D_CHUNKED",
                "dims": [10]
            }
        },
        "shape": {
            "class": "H5S_SIMPLE",
            "dims": [10],
            "maxdims": [10]
        },
        "type": {
            "class": "H5T_FLOAT",
            "base": "H5T_IEEE_F32BE"
        },
        "attributeCount": 0,
        "domain": "/shared/tall.h5",
        "created": 1531174597,
        "lastModified": 1531174597,
        "hrefs": [
            {"rel": "self", "href": "hsdshdflab.hdfgroup.org/datasets/d-bf1cb98c-83c5-11e8-b9ee-0242ac12000a"},
            {"rel": "root", "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016"},
            {"rel": "home", "href": "hsdshdflab.hdfgroup.org/"},
            {"rel": "attributes", "href": "hsdshdflab.hdfgroup.org/datasets/d-bf1cb98c-83c5-11e8-b9ee-0242ac12000a/attributes"},
            {"rel": "data", "href": "hsdshdflab.hdfgroup.org/datasets/d-bf1cb98c-83c5-11e8-b9ee-0242ac12000a/value"}
        ]
    }

Related Resources
=================

* :doc:`DELETE_Dataset`
* :doc:`../AttrOps/GET_Attributes`
* :doc:`GET_DatasetShape`
* :doc:`GET_DatasetType`
* :doc:`GET_Datasets`
* :doc:`GET_Value`
* :doc:`POST_Value`
* :doc:`PUT_Value`
 

 