**********************************************
PUT Link
**********************************************

Description
===========
Creates a new link to a group, dataset, or committed datatype. 
Attempting to create a link to an object that does not exist will fail for a hard link, but succeed for soft and external links.

*Note*: The link creation will fail if a link with the same name in the same location already exists.

Requests
========

Syntax
------

To create a group link:

.. code-block:: http

    PUT /groups/<id>/links/<title> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /groups/<id>/links/<title>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the group that will contain the new link
* *<title>* is the url-encoded name of the requested link

Request Parameters
------------------

domain
^^^^^
The domain containing the link's parent object. This 
parameter is optional if the domain is specified in the request headers.

Request Headers
---------------
This implementation of the operation uses only the request headers that are common
to most requests.  See :doc:`../CommonRequestHeaders`

Request Elements
----------------

id
^^^^
The UUID of the object that the new link will point to. This should only be provided for hard links. 

h5path
^^^^
The path to the object the new link will point to. This should be provided for soft and external links.

h5domain
^^^^
The domain containing the object the new link will point to. This should be provided only for external links.

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
An empty array.  See :doc:`../Hypermedia`.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request - Hard Link
----------------------------------

This request creates a hard link called "hardlnk" to the root group. The created link itself resides within the root group.

.. code-block:: http

    PUT /groups/g-51db5cdf-08b64144-d953-d45780-3ec9cc/links/hardlnk HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 38
    Accept: */*
    Accept-Encoding: gzip, deflate


.. code-block:: json

    {
        "id": "g-51db5cdf-08b64144-d953-d45780-3ec9cc"
    }

Sample Response - Hard Link
-----------------------------------

.. code-block:: http

   HTTP/1.1 201 Created
   Date: Sun, 15 Jul 2018 15:07:03 GMT
   Content-Length: 13
   Content-Type: application/json
   Server: nginx/1.15.0

.. code-block:: json

    {"hrefs": []}

Related Resources
=================

* :doc:`DELETE_Link`
* :doc:`GET_Link`
* :doc:`GET_Links`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
 

 
