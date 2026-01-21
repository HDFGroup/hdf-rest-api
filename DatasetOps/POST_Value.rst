**********************************************
POST Value
**********************************************

Description
===========
Gets values of a dataset for a given point selection (provided in the body of the 
request).

Requests
========

Syntax
------
.. code-block:: http

    POST /datasets/<id>/value HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    POST /datasets/<id>/value?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the requested dataset

Request Parameters
------------------
This implementation of the operation does not use request parameters.

Request Headers
---------------
This implementation of the operation supports the common headers in addition to the "Accept" header value
of "application/octet-stream".  Use this accept value if a binary response is desired.  Binary data will be
more efficient for large data requests.  If a binary response can be returned, the "Content-Type" response
header will be "application/octet-stream".  Otherwise the response header will be "application/json".

Note: Binary responses are only supported for datasets that have a fixed-length type
(i.e. either a fixed length primitive type or compound type that in turn consists of fixed-length types).  Namely
variable length strings and variable length data types will always be returned as JSON.

Note: if a binary response is returned, it will consist of the equivalent binary data of the "data" item in the JSON
response.  No data representing "hrefs" is returned.

For other request headers, see :doc:`../CommonRequestHeaders`

Request Body
------------

JSON and Binary response requests
:::::::::::::::::::::::::::::::::

The request body should be a JSON object with the following key:

points
^^^^^^

An array of points defining the selection.  Each point can either be an integer
(if the dataset has just one dimension), or an array where the length of the 
array is equal to the number of dimensions of the dataset.

Responses
=========

Response Headers
----------------

This implementation of the operation uses only response headers that are common to 
most responses.  See :doc:`../CommonResponseHeaders`.

Response Elements
-----------------

JSON Response
:::::::::::::

On success, a JSON response will be returned with the following elements:

value
^^^^^
An array of values where the length of the array is equal to the number of points 
in the request.  Each value will be a string, integer, or JSON object consistent
with the dataset type (e.g. a compound type).

Binary Response
:::::::::::::::

On success, a binary response will be returned with the binary data values of the
requested points for the dataset.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

.. code-block:: http

    POST /datasets/d-be9c3582-83c5-11e8-947e-0242ac120014/value HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 40
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "points": [19, 17, 13, 11, 7, 5, 3, 2]
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"points\": [19, 17, 13, 11, 7, 5, 3, 2]}" hsdshdflab.hdfgroup.org/datasets/d-be9c3582-83c5-11e8-947e-0242ac120014/value

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Wed, 18 Jul 2018 21:23:45 GMT
    Content-Length: 39
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "value": [0, 1, 4, 9, 16, 25, 36, 49]
    }

Sample Request - Binary
-----------------------

.. code-block:: http

    POST /datasets/d-be9c3582-83c5-11e8-947e-0242ac120014/value HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 40
    Accept: application/octet-stream
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "points": [19, 17, 13, 11, 7, 5, 3, 2]
    }

Sample cURL command
-------------------

*Note the use of "--output -" to redirect output to the terminal. This is not advised,
as it can mess up the terminal, and "--output <FILE>" should be used instead.*

.. code-block:: bash

    $ curl --output - -X POST --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json" --header "Accept: application/octet-stream"
      -d "{\"points\": [19, 17, 13, 11, 7, 5, 3, 2]}" hsdshdflab.hdfgroup.org/datasets/d-be9c3582-83c5-11e8-947e-0242ac120014/value

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Fri, 20 Jul 2018 17:54:40 GMT
    Content-Length: 32
    Content-Type: application/json
    Server: nginx/1.15.0

Hexdump of output as captured to file

::

    00000000  00 00 00 13 00 00 00 11  00 00 00 0d 00 00 00 0b  |................|
    00000010  00 00 00 07 00 00 00 05  00 00 00 03 00 00 00 02  |................|
    00000020


Related Resources
=================

* :doc:`GET_Dataset`
* :doc:`GET_Value`
* :doc:`PUT_Value`


