**********************************************
DELETE Attribute
**********************************************

Description
===========
The implementation of the DELETE operation deletes the attribute named in the URI.

Requests
========

Syntax
------

To delete an attribute of a group:

.. code-block:: http

    DELETE /groups/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /groups/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To delete an attribute of a dataset:

.. code-block:: http

    DELETE /datasets/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /datasets/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To delete an attribute of a committed datatype:

.. code-block:: http

    DELETE /datatypes/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /datatypes/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the dataset/group/committed datatype
* *<name>* is the url-encoded name of the requested attribute

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

    DELETE /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept: */*
    Accept-Encoding: gzip, deflate

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr1

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Sun, 15 Jul 2018 16:06:54 GMT
    Content-Length: 13
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {"hrefs": []}

Related Resources
=================

* :doc:`GET_Attributes`
* :doc:`GET_Attribute`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
* :doc:`PUT_Attribute`


 