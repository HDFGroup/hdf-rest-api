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
This implementation of the operation uses only the request headers that are common
to most requests.  See :doc:`../CommonRequestHeaders`

Request Body
------------

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

On success, a JSON response will be returned with the following elements:

value
^^^^^
An array of values where the length of the array is equal to the number of points 
in the request.  Each value will be a string, integer, or JSON object consistent
with the dataset type (e.g. a compound type).

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
    Content-Length: 92
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
    Content-Length: 40
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "value": [0, 1, 4, 9, 16, 25, 36, 49]
    }

Related Resources
=================

* :doc:`GET_Dataset`
* :doc:`GET_Value`
* :doc:`PUT_Value`


