**********************************************
DELETE Attributes
**********************************************

Description
===========
Delete a collection of attributes specified by name as a parameter. 

========

Syntax
------
.. code-block:: http

    DELETE /groups/<id>/attributes?attr_names=<name>/<name>/.../<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /groups/<id>/attributes?separator=:&attr_names=<name>:<name>:...:<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /datasets/<id>/attributes?attr_names=<name>/<name>/.../<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /datasets/<id>/attributes?separator=:&attr_names=<name>:<name>:...:<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /datatypes/<id>/attributes?attr_names=<name>/<name>/.../<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /datatypes/<id>/attributes?separator=:&attr_names=<name>:<name>:...:<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>


* *<id>* is the UUID of the dataset/group/committed datatype
* *<name>* is the url-encoded name of a requested attribute

Request Parameters
------------------

This operation requires the parameter `attr_names` to specify the attributes to delete. Other parameters are optional.

attr_names
^^^^^
The names of the attributes to delete, as a single query parameter. Each individual attribute name is separated by a separator string. This parameter is required.

separator
^^^^^
The string that separates individual attribute names in the `attr_names` parameter. This parameter is optional, with a default value of `/`.

domain
^^^^^
The domain containing the attributes' parent object. This parameter is optional if the domain is specified in the request headers.

encoding
^^^^^
The type of encoding used for the attribute names. This parameter is optional.

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

    DELETE /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/?attr_names=attr1/attr2/attr3 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept: */*
    Accept-Encoding: gzip, deflate

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/?attr_names=attr1/attr2/attr3

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

* :doc:`DELETE_Attribute`
* :doc:`GET_Attributes`
* :doc:`GET_Attribute`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
* :doc:`PUT_Attribute`


 