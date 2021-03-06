**********************************************
DELETE Group
**********************************************

Description
===========
The implementation of the DELETE operation deletes the group with the UUID given in the
URI.  All attributes and links of the group will also be deleted.  In addition, any 
links from other groups **TO** the deleted group will be removed.

*Note:* Groups, datatypes, and datasets that are referenced by the group's links will 
**not** be deleted.  Use the DELETE operation for those objects to remove them.

Requests
========

Syntax
------

.. code-block:: http

    DELETE /groups/<id> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    DELETE /groups/<id>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

*<id>* is the UUID of the group to be deleted.

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

    DELETE /groups/g-76de767c-8606-11e8-9cc2-0242ac120008 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Authorization: authorization_string

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X DELETE -u username:password --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-76de767c-8606-11e8-9cc2-0242ac120008

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 12 Jul 2018 19:05:43 GMT
    Content-Type: application/json
    Server: nginx/1.15.0

Related Resources
=================

* :doc:`POST_Group`
* :doc:`GET_Group`


 