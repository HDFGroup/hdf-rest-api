**********************************************
PUT AttributeValue
**********************************************

Description
===========
Update the value of an attribute in a group, dataset, or committed datatype.

Requests
========

Syntax
------

To update the value of a group attribute:

.. code-block:: http

    PUT /groups/<id>/attributes/<name>/value HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /groups/<id>/attributes/<name>/value?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To update the value of a dataset attribute:

.. code-block:: http

    PUT /datasets/<id>/attributes/<name>/value HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /datasets/<id>/attributes/<name>/value?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To update the value of a committed datatype attribute:

.. code-block:: http

    PUT /datatypes/<id>/attributes/<name>/value HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /datatypes/<id>/attributes/<name>/value?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the dataset/group/committed datatype
* *<name>* is the url-encoded name of the requested attribute

Request Parameters
------------------

This implementation of the operation uses several request parameters, all optional:

domain
^^^^^
The domain containing the attribute's parent object. This 
parameter is optional if the domain is specified in the request headers.

Request Headers
---------------
This implementation of the operation uses only the request headers that are common
to most requests.  See :doc:`../CommonRequestHeaders`

Request Elements
----------------

The request body must include a JSON object with a 'value' key. The request body
may include an optional `encoding` parameter.

value
^^^^^

A JSON array (or number or string for scalar attributes with primitive types) that 
specifies the new values for the attribute.  The elements of the array must be 
compatible with the type of the attribute.
Not valid to provide if the shape is ``H5S_NULL``.


encoding
^^^^^
Specifies the encoding that the new attribute value is in. Defaults to `None`.

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

Sample Request - scalar attribute
----------------------------------

Create an integer scalar attribute in the group with UUID of "g-45f464d8-" named "attr4".  
The value of the attribute will be 42.

.. code-block:: http

    PUT /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr4/value HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 38
    Accept: */*
    Accept-Encoding: gzip, deflate


.. code-block:: json

    {
        "value": 42
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"type\": \"H5T_STD_I32LE\",\"value\": 42}" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr4/value

Sample Response - scalar attribute
-----------------------------------

.. code-block:: http

   HTTP/1.1 201 Created
   Date: Sun, 15 Jul 2018 15:07:03 GMT
   Content-Length: 13
   Content-Type: application/json
   Server: nginx/1.15.0

.. code-block:: json

    {"hrefs": []}

Sample Request - string attribute
----------------------------------

Create a two-element, fixed width string attribute in the group with UUID of 
"g-45f464d8-" named "attr6".  
The attributes values will be "Hello, ..." and "Goodbye!".

.. code-block:: http

    PUT /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr6/value HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 178
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "value": ["Hello, I'm a fixed-width string!", "Goodbye!"]
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"shape\": [2], \"type\": {\"class\": \"H5T_STRING\", \"charSet\": \"H5T_CSET_ASCII\", \"strPad\": \"H5T_STR_NULLPAD\", \"length\": 40},
      \"value\": [\"Hello, I'm a fixed-width string"'!'"\", \"Goodbye"'!'"\"]}" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr6/value

Sample Response - string attribute
-----------------------------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Sun, 15 Jul 2018 15:35:43 GMT
    Content-Length: 13
    Content-Type: application/json
    Server: nginx/1.15.0

..
    TODO

.. code-block:: json

    {"hrefs": []}

Sample Request - compound type
----------------------------------

Create a two-element, attribute in the group with UUID of 
"g-45f464d8-" named "attr_compound".   The attribute has a compound type with an integer
and a floating point element. 

.. code-block:: http

    PUT /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr_compound/value HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 187
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json


    {
        "value": [[55, 32.34], [59, 29.34]]
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"shape\": 2, \"type\": {\"class\": \"H5T_COMPOUND\", \"fields\": [{\"type\": \"H5T_STD_I32LE\", \"name\": \"temp\"},
      {\"type\": \"H5T_IEEE_F32LE\", \"name\": \"pressure\"}]}, \"value\": [[55, 32.34], [59, 29.34]]}" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr_compound/value

Sample Response - compound type 
-----------------------------------

.. code-block:: http

    HTTP/1.1 201 Created
    Date: Sun, 15 Jul 2018 15:43:00 GMT
    Content-Length: 13
    Content-Type: application/json
    Server: nginx/1.15.0
..
    TODO

.. code-block:: json

    {"hrefs": []}



Related Resources
=================

* :doc:`DELETE_Attribute`
* :doc:`GET_Attribute`
* :doc:`GET_Attributes`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
 

 
