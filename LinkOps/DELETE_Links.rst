**********************************************
DELETE Links
**********************************************

Description
===========
Delete a collection of links specified by name as a parameter. 

========

Syntax
------
.. code-block:: http

    DELETE /groups/<id>/links?titles=<title>/<title>/.../<title> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /groups/<id>/links?separator=:&titles=<title>:<title>:...:<title> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

* *<id>* is the UUID of the group containing the links to delete
* *<title>* is the url-encoded name of a requested link

Request Parameters
------------------

This operation requires the parameter `titles` to specify the links to delete. Other parameters are optional.

titles
^^^^^
The names of the links to delete, as a single query parameter. Each individual link name is separated by a separator string. This parameter is required.

separator
^^^^^
The string that separates individual link names in the `titles` parameter. This parameter is optional, with a default value of `/`.

domain
^^^^^
The domain containing the links' parent object. This parameter is optional if the domain is specified in the request headers.

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

On success, an empty JSON response will be returned.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

.. code-block:: http

    DELETE /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/links/?titles=lnk1/lnk2/lnk3 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept: */*
    Accept-Encoding: gzip, deflate

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/links/?titles=lnk1/lnk2/lnk3

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Sun, 15 Jul 2018 16:06:54 GMT
    Content-Length: 13
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {}

Related Resources
=================

* :doc:`DELETE_Link`
* :doc:`GET_Links`
* :doc:`GET_Link`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
* :doc:`PUT_Link`


 