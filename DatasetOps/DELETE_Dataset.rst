**********************************************
DELETE Dataset
**********************************************

Description
===========
The implementation of the DELETE operation deletes the dataset named in the URI.  All 
attributes and links of the dataset will also be deleted.  In addition any 
links from other groups to the deleted group will be removed.

Requests
========

Syntax
------
.. code-block:: http

    DELETE /datasets/<id> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /datasets/<id>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

*<id>* is the UUID of the requested dataset to be deleted.

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

    DELETE /datasets/d-11dac970-8ace-11e8-8126-0242ac12000d HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 0
    Accept: */*
    Accept-Encoding: gzip, deflate

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X DELETE -u username:pasword --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datasets/d-11dac970-8ace-11e8-8126-0242ac12000d

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Wed, 18 Jul 2018 21:15:42 GMT
    Content-Length: 0
    Content-Type: application/json
    Server: nginx/1.15.0

Related Resources
=================

* :doc:`GET_Datasets`
* :doc:`GET_Dataset`
* :doc:`POST_Dataset`
 

 