**********************************************
GET Shape
**********************************************

Description
===========
Gets the shape of a dataset.

Requests
========

Syntax
------
.. code-block:: http

    GET /datasets/<id>/shape HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datasets/<id>/shape?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

*<id>* is the UUID of the dataset that shape is requested for.

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

created
^^^^^^^
A timestamp giving the time the datashape (same as the dataset) was created in 
UTC (ISO-8601 format).

lastModified
^^^^^^^^^^^^
A timestamp giving the most recent time the dataspace has been modified (i.e. a  
dimension has been extended) in UTC (ISO-8601 format).

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

    GET /datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/shape HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/shape

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Wed, 18 Jul 2018 22:01:40 GMT
    Content-Length: 440
    Etag: "76ed777f151c70d0560d1414bffe1515a3df86b0"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "shape": {
            "class": "H5S_SIMPLE",
            "maxdims": [10, 10],
            "dims": [10, 10]
        },
        "lastModified": 1531174596,
        "created": 1531174596,
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/shape", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013", "rel": "owner"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"}
        ]
    }

Sample Request - Resizable
--------------------------

.. code-block:: http

    GET /datasets/d-20388136-8ad5-11e8-8126-0242ac12000d/shape HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datasets/d-20388136-8ad5-11e8-8126-0242ac12000d/shape

Sample Response - Resizable
----------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Wed, 18 Jul 2018 22:05:23 GMT
    Content-Length: 432
    Etag: "1082800980d6809a8008b22e225f1adde8afc73f"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "shape": {
            "dims": [10, 25],
            "class": "H5S_SIMPLE",
            "maxdims": [0, 0]
        },
        "lastModified": 1531950860,
        "created": 1531950860,
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-20388136-8ad5-11e8-8126-0242ac12000d/shape", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-20388136-8ad5-11e8-8126-0242ac12000d", "rel": "owner"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e", "rel": "root"}
        ]
    }

Related Resources
=================

* :doc:`GET_Dataset`
* :doc:`GET_DatasetType`
* :doc:`PUT_DatasetShape`


