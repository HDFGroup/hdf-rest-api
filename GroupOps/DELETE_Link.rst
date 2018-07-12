**********************************************
DELETE Link
**********************************************

Description
===========
The implementation of the DELETE operation deletes the link named in the URI.   

Groups, datatypes, and datasets that are referenced by the link will **not** be
deleted.   To delete groups, datatypes or datasets, use the appropriate DELETE operation
for those objects.

Requests
========

Syntax
------
.. code-block:: http

    DELETE /groups/<id>/links/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /groups/<id>/links/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the group the link is a member of.
* *<name>* is the URL-encoded name of the link.


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

An attempt to delete the root group will return 403 - Forbidden.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

.. code-block:: http

    DELETE /groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008/links/deleteme HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 0
    Accept: */*
    Accept-Encoding: gzip, deflate

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-b116b6f0-85e9-11e8-9cc2-0242ac120008/links/deleteme

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 12 Jul 2018 19:45:54 GMT
    Content-Length: 12
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {"href": []}

Related Resources
=================

* :doc:`../DatasetOps/DELETE_Dataset`
* :doc:`../DatatypeOps/DELETE_Datatype`
* :doc:`DELETE_Group`
* :doc:`GET_Link`
* :doc:`GET_Groups`
* :doc:`POST_Group`
 

 