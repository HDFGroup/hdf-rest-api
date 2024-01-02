**********************************************
PUT Attributes
**********************************************

Description
===========
Creates a collections of new attributes in a group, dataset, or committed datatype.

*Note*: The new attributes will replace any existing attributes with the same names if the `replace` parameter is provided.

Requests
========

Syntax
------

To create a collection of group attributes:

.. code-block:: http

    PUT /groups/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /groups/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To create a collection of dataset attributes:

.. code-block:: http

    PUT /datasets/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /datasets/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To create a collection of committed datatype attributes:

.. code-block:: http

    PUT /datatypes/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /datatypes/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the dataset/group/committed datatype
* *<name>* is the url-encoded name of the requested attribute

Request Parameters
------------------

This implementation of the operation uses several request parameters, all optional:

replace
^^^^^
Whether existing attributes on the same name on the target object should be replaced. If such an attribute already exists and `replace` is false,
the request will fail with code 409. This parameter is optional and defaults to false. 

domain
^^^^^
The domain containing the attributes' parent objects. This 
parameter is optional if the domain is specified in the request headers.

Request Headers
---------------
This implementation of the operation uses only the request headers that are common
to most requests.  See :doc:`../CommonRequestHeaders`

Request Elements
----------------

The request must provide at least one attribute to be written. This may be done in one of three ways: As 
a list of attribute items named `attributes`, a dictionary `obj_ids` mapping a set of 
target object ids to the attribute objects which are to be written to those
objects, or a single attribute item desribe by a `type` and optional `shape`/`value`.

obj_ids
^^^^^
A collection of objects to which the attributes are to be written. 

If the same set of attributes is to be written to each target object, `obj_ids` should
be a list. If each target object has a unique collection of attributes which should 
be written to it, `obj_ids` should map each target object id
to a set of attributes to write to that object.

If this is not provided, the attributes in `attr_names` will be written to the object
with the id specified in the query.

attributes
^^^^
A dictionary mapping the name of each new attribute to its information. Each individual
attribute is required to provide a `type`, and may optionally provide a `shape` and `value`.

type
^^^^

Specifies the desired type of a single attribute to create.  Either a string that is one of the 
predefined type values, a UUID of a committed type, or a JSON object describing the type.
See :doc:`../Types/index` for details of the type specification.

shape
^^^^^^

Specifies the desired shape of a single attribute to create.
Either a string with the value ``H5S_NULL`` or an
integer array describing the dimensions of the attribute. 
If shape is not provided, a scalar attribute will be created.
If a shape value of ``H5S_NULL`` is specified a null space attribute will be created.
(Null space attributes can not contain any data values.)

value
^^^^^

Specifies the desired value of a single attribute to create.
A JSON array (or number or string for scalar attributes with primitive types) that 
specifies the initial values for the attribute.  The elements of the array must be 
compatible with the type of the attribute.
Not valid to provide if the shape is ``H5S_NULL``.

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

    PUT /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr4 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 38
    Accept: */*
    Accept-Encoding: gzip, deflate


.. code-block:: json

    {
        "type": "H5T_STD_I32LE", 
        "value": 42
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"type\": \"H5T_STD_I32LE\",\"value\": 42}" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr4

Sample Response - scalar attribute
-----------------------------------

.. code-block:: http

   HTTP/1.1 201 Created
   Date: Sun, 15 Jul 2018 15:07:03 GMT
   Content-Length: 13
   Content-Type: application/json
   Server: nginx/1.15.0

..
    TODO

.. code-block:: json

    {"hrefs": []}

Sample Request - string attribute
----------------------------------

Create a two-element, fixed width string attribute in the group with UUID of 
"g-45f464d8-" named "attr6".  
The attributes values will be "Hello, ..." and "Goodbye!".

.. code-block:: http

    PUT /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr6 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 178
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json

    {
        "shape": [2], 
        "type": {
            "class": "H5T_STRING",
            "cset": "H5T_CSET_ASCII",  
            "strpad": "H5T_STR_NULLPAD", 
            "strsize": 40
        }, 
        "value": ["Hello, I'm a fixed-width string!", "Goodbye!"]
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"shape\": [2], \"type\": {\"class\": \"H5T_STRING\", \"charSet\": \"H5T_CSET_ASCII\", \"strPad\": \"H5T_STR_NULLPAD\", \"length\": 40},
      \"value\": [\"Hello, I'm a fixed-width string"'!'"\", \"Goodbye"'!'"\"]}" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr6

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

    PUT /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr_compound HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 187
    Accept: */*
    Accept-Encoding: gzip, deflate

.. code-block:: json


    {
        "shape": 2, 
        "type": {
            "class": "H5T_COMPOUND",
            "fields": [
                {"type": "H5T_STD_I32LE", "name": "temp"}, 
                {"type": "H5T_IEEE_F32LE", "name": "pressure"}
            ] 
        }, 
        "value": [[55, 32.34], [59, 29.34]]
    }

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X PUT -u username:password --header "X-Hdf-domain: /shared/tall.h5" --header "Content-Type: application/json"
      -d "{\"shape\": 2, \"type\": {\"class\": \"H5T_COMPOUND\", \"fields\": [{\"type\": \"H5T_STD_I32LE\", \"name\": \"temp\"},
      {\"type\": \"H5T_IEEE_F32LE\", \"name\": \"pressure\"}]}, \"value\": [[55, 32.34], [59, 29.34]]}" hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr_compound

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
 

 
