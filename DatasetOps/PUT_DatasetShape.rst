**********************************************
PUT Shape
**********************************************

Description
===========
Modifies the dimensions of a dataset.  Dimensions can only be changed if the dataset
was initially created with that dimension as *extensible* - i.e. the maxdims value
for that dimension is larger than the initial dimension size (or maxdims set to 0).

*Note:* Dimensions can only be made larger, they can not be reduced.

Requests
========

Syntax
------
.. code-block:: http

    PUT /datasets/<id>/shape HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /datasets/<id>/shape?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the dataset whose shape will be modified.

Request Parameters
------------------
This implementation of the operation does not use request parameters.

Request Headers
---------------
This implementation of the operation uses only the request headers that are common
to most requests.  See :doc:`../CommonRequestHeaders`

Request Elements
----------------
The request body must include a JSON object with a "shape" key as described below:

shape
^^^^^
An integer array giving the new dimensions of the dataset.

Responses
=========

Response Headers
----------------

This implementation of the operation uses only response headers that are common to 
most responses.  See :doc:`../CommonResponseHeaders`.

Response Elements
-----------------

On success, a JSON response will be returned with the following elements:

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

    PUT /datasets/d-20388136-8ad5-11e8-8126-0242ac12000d/shape HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 19
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "shape": [10, 25]
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"shape\": [10, 25]}" hsdshdflab.hdfgroup.org/datasets/d-20388136-8ad5-11e8-8126-0242ac12000d/shape

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Wed, 18 Jul 2018 21:54:47 GMT
    Content-Length: 13
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {"hrefs": []}

Related Resources
=================

* :doc:`GET_Dataset`
* :doc:`GET_DatasetShape`
* :doc:`GET_Value`
* :doc:`POST_Value`
* :doc:`PUT_Value`


 