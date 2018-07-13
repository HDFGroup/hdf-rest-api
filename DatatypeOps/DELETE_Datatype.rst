**********************************************
DELETE Datatype
**********************************************

Description
===========
The implementation of the DELETE operation deletes the committed datatype
 named in the URI.  All attributes of the datatype will also be deleted.

Requests
========

Syntax
------
.. code-block:: http

    DELETE /datatypes/<id> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /datatypes/<id>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

*<id>* is the UUID of the datatype to be deleted.

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

This implementation of the operation does not return any response elements.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

.. code-block:: http

    DELETE /datatypes/t-6b0bdf9a-86b2-11e8-89f2-0242ac120009 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 0
    Accept: */*
    Accept-Encoding: gzip, deflate

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datatypes/t-6b0bdf9a-86b2-11e8-89f2-0242ac120009

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Fri, 13 Jul 2018 15:49:44 GMT
    Content-Type: application/json
    Server: nginx/1.15.0

Related Resources
=================

* :doc:`../AttrOps/GET_Attributes`
* :doc:`GET_Datatype`
* :doc:`GET_Datatypes`
* :doc:`POST_Datatype`
* :doc:`../DatasetOps/POST_Dataset`
* :doc:`../AttrOps/PUT_Attribute`


 