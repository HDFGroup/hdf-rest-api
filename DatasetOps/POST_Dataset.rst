**********************************************
POST Dataset
**********************************************

Description
===========
Creates a new Dataset.

Requests
========

Syntax
------
.. code-block:: http

    POST /datasets HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    POST /datasets?domain=DOMAIN HTTP/1.1
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
The request body must include a JSON object with a "type" key.  Optionally "shape", 
"maxdims", "creationProperties" and "link" keys can be provided.

type
^^^^
Either a string that is one of the predefined type values, a UUID of a committed type,
or a JSON object describing the type.  See :doc:`../Types/index` for details of the
type specification.

shape
^^^^^^
Either a string with the value ``H5S_NULL`` or an
integer array describing the initial dimensions of the dataset.  If shape is not
provided, a scalar dataset will be created.
If the shape value of ``H5S_NULL`` is specified a dataset with a null dataspace will be 
created.  A null
dataset has attributes and a type, but will not be able to store any values.

maxdims
^^^^^^^
An integer array describing the maximum extent of each dimension (or 0 for unlimited
dimensions).  If maxdims is not provided the resulting dataset will be non-extensible.
Not valid to include if ``H5S_NULL`` is specified for the shape.

creationProperties
^^^^^^^^^^^^^^^^^^
A JSON object that can specify chunk layout, filters, fill value, and other aspects of the dataset.
See: http://hdf5-json.readthedocs.org/en/latest/bnf/dataset.html#grammar-token-dcpl for a complete 
description of fields that can be used.

If creationProperties is not provided, default values will be used.

link["id"]
^^^^^^^^^^
The UUID of the group that the new dataset should be linked to.  If the UUID is not valid,
the request will fail and a new dataset will not be created.

link["name"]
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
The UUID of the newly created dataset.

root
^^^^
The UUID of the root group for the domain which the new dataset is within.

type
^^^^
A JSON object representing the type definition for the dataset. See :doc:`../Types/index`
for information on how different types are represented.

shape
^^^^^

A JSON object with the following keys:

class: A string with one of the following values:

 * H5S_NULL: A null dataspace, which has no elements
 * H5S_SCALAR: A dataspace with a single element (although possibly of a complex datatype)
 * H5S_SIMPLE: A dataspace that consists of a regular array of elements
 
dims: An integer array whose length is equal to the number of dimensions (rank) of the 
dataspace.  The value of each element gives the current size of each dimension.  Dims
is not returned for H5S_NULL or H5S_SCALAR dataspaces.

maxdims: An integer array whose length is equal to the number of dimensions of the 
dataspace.  The value of each element gives the maximum size of each dimension. A value
of 0 indicates that the dimension has *unlimited* extent.  maxdims is not returned for
H5S_SIMPLE dataspaces which are not extensible or for H5S_NULL or H5S_SCALAR dataspaces.

attributeCount
^^^^^^^^^^^^^^
The number of attributes belonging to the dataset.

created
^^^^^^^
A timestamp giving the time the dataset was created in UTC (ISO-8601 format).

lastModified
^^^^^^^^^^^^
A timestamp giving the most recent time the dataset has been modified (i.e. attributes or 
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

Create a one-dimensional dataset with 10 floating point elements.

.. code-block:: http

    POST /datasets HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 39
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "shape": 10, 
        "type": "H5T_IEEE_F32LE"
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"shape\": 10, \"type\": \"H5T_IEEE_F32LE\"}" hsdshdflab.hdfgroup.org/datasets

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Wed, 18 Jul 2018 19:46:30 GMT
    Content-Length: 651
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "id": "d-438f976c-8ac3-11e8-9ac3-0242ac12000c",
        "type": {
            "class": "H5T_FLOAT",
            "base": "H5T_IEEE_F32LE"
        },
        "shape": {
            "class": "H5S_SIMPLE",
            "dims": [10]
        },
        "lastModified": 1531943189,
        "created": 1531943189,
        "attributeCount": 0,
        "root": "g-45f464d8-883e-11e8-a9dc-0242ac12000e"
    }

Sample Request with Link
------------------------

Create a dataset with 10 variable length string elements.  Create link in group: 
"g-45f464d8-..." with name: "linked_dset".

.. code-block:: http

    POST /datasets HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 239
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "type": {
            "class": "H5T_STRING",
            "length": "H5T_VARIABLE", 
            "charSet": "H5T_CSET_ASCII", 
            "order": "H5T_ORDER_NONE", 
            "strPad": "H5T_STR_NULLTERM"
        },
        "shape": 10, 
        "link": {
            "id": "g-45f464d8-883e-11e8-a9dc-0242ac12000e", 
            "name": "linked_dset"
        }
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"type\": {\"class\": \"H5T_STRING\", \"length\": \"H5T_VARIABLE\", \"charSet\": \"H5T_CSET_ASCII\", \"order\": \"H5T_ORDER_NONE\", \"strPad\": \"H5T_STR_NULLTERM\"},
      \"shape\": 10, \"link\": {\"id\": \"g-45f464d8-883e-11e8-a9dc-0242ac12000e\", \"name\": \"linked_dset\"}}" hsdshdflab.hdfgroup.org/datasets

Sample Response with Link
-------------------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Wed, 18 Jul 2018 19:54:02 GMT
    Content-Length: 363
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "id": "d-5154aed6-8ac4-11e8-9db9-0242ac120007",
        "shape": {
            "class": "H5S_SIMPLE",
            "dims": [10]
        },
        "type": {
            "charSet": "H5T_CSET_ASCII",
            "order": "H5T_ORDER_NONE",
            "strPad": "H5T_STR_NULLTERM",
            "class": "H5T_STRING",
            "length": "H5T_VARIABLE"
        },
        "created": 1531943641,
        "attributeCount": 0,
        "lastModified": 1531943641,
        "root": "g-45f464d8-883e-11e8-a9dc-0242ac12000e"
    }

Sample Request - Resizable Dataset
----------------------------------

  Create a one-dimensional dataset with 10 elements, but extendable to an unlimited
  dimension.

.. code-block:: http

    POST /datasets HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 53
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "type": "H5T_IEEE_F32LE",
        "shape": 10,
        "maxdims": 0
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"type\": \"H5T_IEEE_F32LE\", \"shape\": 10, \"maxdims\": 0}" hsdshdflab.hdfgroup.org/datasets

Sample Response - Resizable Dataset
-----------------------------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Wed, 18 Jul 2018 20:25:45 GMT
    Content-Length: 292
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "id": "d-bf2a5b64-8ac8-11e8-8126-0242ac12000d",
        "type": {
            "class": "H5T_FLOAT",
            "base": "H5T_IEEE_F32LE"
        },
        "shape": {
            "maxdims": [0],
            "class": "H5S_SIMPLE",
            "dims": [10]
        },
        "root": "g-45f464d8-883e-11e8-a9dc-0242ac12000e",
        "attributeCount": 0,
        "lastModified": 1531945544,
        "created": 1531945544
    }

Sample Request - Committed Type
----------------------------------

  Create a two-dimensional dataset which uses a committed type with UUID: 

.. code-block:: http

    POST /datasets HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 69
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "type": "accd0b1e-a792-11e4-bada-3c15c2da029e",
        "shape": [10, 10]
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"type\": \"t-9bd41cc6-8ac9-11e8-b72d-0242ac12000a\", \"shape\": [10, 10]}" hsdshdflab.hdfgroup.org/datasets

Sample Response - Committed Type
-----------------------------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Wed, 18 Jul 2018 20:33:23 GMT
    Content-Length: 328
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "id": "d-d04c4d2a-8ac9-11e8-9db9-0242ac120007",
        "shape": {
            "class": "H5S_SIMPLE",
            "dims": [10, 10]
        },
        "type": {
            "base": "H5T_IEEE_F32LE",
            "id": "t-9bd41cc6-8ac9-11e8-b72d-0242ac12000a", 
            "class": "H5T_FLOAT"
        },
        "created": 1531946002,
        "attributeCount": 0,
        "lastModified": 1531946002,
        "root": "g-45f464d8-883e-11e8-a9dc-0242ac12000e"
    }

Sample Request - SZIP Compression with chunking
-----------------------------------------------

.. code-block:: http

    POST /datasets HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 67
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "creationProperties": {
            "filters": [
                {
                    "bitsPerPixel": 8,
                    "coding": "H5_SZIP_EC_OPTION_MASK",
                    "id": 4,
                    "pixelsPerBlock": 32,
                    "pixelsPerScanline": 100
                }
            ],
            "layout": {
                "class": "H5D_CHUNKED",
                "dims": [
                    100,
                    100
                ]
            }
        },
        "shape": [
            1000,
            1000
        ],
        "type": "H5T_IEEE_F32LE"
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"creationProperties\": {\"filters\": [{\"bitsPerPixel\": 8, \"coding\": \"H5_SZIP_EC_OPTION_MASK\", \"id\": 4, \"pixelsPerBlock\": 32, \"pixelsPerScanline\": 100}],
      \"layout\": {\"class\": \"H5D_CHUNKED\", \"dims\": [100, 100]}}, \"shape\": [1000, 1000], \"type\": \"H5T_IEEE_F32LE\"}" hsdshdflab.hdfgroup.org/datasets

Sample Response - SZIP Compression with chunking
------------------------------------------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Wed, 18 Jul 2018 21:03:51 GMT
    Content-Length: 284
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "id": "d-11dac970-8ace-11e8-8126-0242ac12000d",
        "attributeCount": 0,
        "type": {
            "class": "H5T_FLOAT",
            "base": "H5T_IEEE_F32LE"
        },
        "shape": {
            "class": "H5S_SIMPLE",
            "dims": [1000, 1000]
        },
        "lastModified": 1531947830,
        "created": 1531947830,
        "root": "g-45f464d8-883e-11e8-a9dc-0242ac12000e"
    }

Related Resources
=================

* :doc:`GET_Dataset`
* :doc:`GET_Datasets`
* :doc:`GET_Value`
* :doc:`POST_Value`
* :doc:`PUT_Value`


