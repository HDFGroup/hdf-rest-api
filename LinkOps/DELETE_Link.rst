**********************************************
DELETE Link
**********************************************

Description
===========
The implementation of the DELETE operation deletes the link named in the URI. For links with 
names containing '/' or '#', :docs:`DELETE_Links` must be used.

Requests
========

Syntax
------
.. code-block:: http

    DELETE /groups/<id>/<links> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /groups/<id>/<links>/<title> HTTP/1.1
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /groups/<id>/<links>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the group containing the link to delete
* *<title>* is the url-encoded name of the requested link

Request Parameters
------------------

domain
^^^^^
The domain containing the link's parent object. This parameter is optional if the domain is specified in the request headers.

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

    DELETE /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/links/lnk1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept: */*
    Accept-Encoding: gzip, deflate

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/links/lnk1

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

* :docs:`DELETE_Links`
* :doc:`GET_Links`
* :doc:`GET_Link`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
* :doc:`PUT_Link`


 