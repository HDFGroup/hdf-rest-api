**********************************************
POST Attributes
**********************************************

Description
===========
Gets a collection of attributes from a dataset, group, or committed datatype.
For each attribute the request returns the attribute's name, type, and shape.  To get 
the attribute data use the parameter `IncludeData`.

Requests
========

Syntax
------

To get the attributes of a group:

.. code-block:: http

    POST /groups/<id>/attributes HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    POST /groups/<id>/attributes?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get the attributes of a dataset:

.. code-block:: http

    POST /datasets/<id>/attributes HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    POST /datasets/<id>/attributes?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get the attributes of a datatype:

.. code-block:: http

    POST /datatypes/<id>/attributes HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    POST /datatypes/<id>/attributes?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

where:    

* *<id>* is the UUID of the dataset/group/committed datatype

Request Parameters
------------------
A collection of attributes to be read must be provided in one of two ways.

If the same attributes are to be read from each target object, the attributes'
names should be provided under `attr_names`.

If each target object has a unique set of attributes which you wish to read, then
the target attribute names should be provided as the value of each key within the
dictionary `obj_ids`.

attr_names
^^^^^
The names of the attributes which should be read from the target object(s).
This parameter is used when the same attributes are to be read from all
target objects.

obj_ids
^^^^^
The collection of objects to read attributes from.

If the same attribute names are to be read from all objects, `obj_ids` is a list.

If each target object has a different set of attributes which should be read from it, `obj_ids` is
a dictionary mapping each target object id to the names of the attributes to read from it.

domain
^^^^^
The domain containing the attributes' parent object. This 
parameter is optional if the domain is specified in the request headers.

ignore_nan
^^^^^
This parameter specifies whether to replace NaN values in retrieved data 
with None instances. This parameter is optional and defaults to false.

IncludeData
^^^^^
This parameter specifies whether to return the data of the attributes in 
addition to the metadata. This parameter is optional and defaults to true.

encoding
^^^^^
What encoding the attributes are stored in. This parameter is optional, 
and defaults to no encoding.

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


attributes
^^^^^^^^^^

An array of JSON objects with an element for each returned attribute.
Each element will have keys: name, type, shape, created, and lastModified.  See 
:doc:`POST_Attribute` for a description of these keys.

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

Get attributes of a group with UUID: "1a956e54-...".

.. code-block:: http

    POST /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Sun, 15 Jul 2018 16:23:43 GMT
    Content-Length: 797
    Etag: "7cbeefcf8d9997a8865bdea3bf2d541a14e9bf71"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "attributes": [
            {
                "type": {
                    "base": "H5T_STD_I8LE",
                    "class": "H5T_INTEGER"
                },
                "name": "attr1",
                "shape": {
                    "dims": [10],
                    "class": "H5S_SIMPLE"
                },
                "created": 1531174596.117736,
                "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes/attr1"
            },
            {
                "type": {
                    "base": "H5T_STD_I32BE",
                    "class": "H5T_INTEGER"
                },
                "name": "attr2",
                "shape": {
                    "dims": [2, 2],
                    "class": "H5S_SIMPLE"
                },
                "created": 1531174596.141592,
                "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes/attr2"
            }
        ],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "owner"}
        ]
    }

Sample Request - get Batch
---------------------------

Get the five attributes that occur after attribute "attr2" from a group with UUID: 
"g-45f464d8-...".

.. code-block:: http

    POST /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes?Marker=attr2&Limit=5 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X POST --header "X-Hdf-domain: /shared/tall.h5" "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes?Marker=attr2&Limit=5"

Sample Response - get Batch
---------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Sun, 15 Jul 2018 16:38:16 GMT
    Content-Length: 1767
    Etag: "9483f4356e08d12b719aa64ece09e659b05adaf2"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "attributes": [
            {
                "name": "attr3",
                "type": {
                    "base": "H5T_STD_U32BE",
                    "class": "H5T_INTEGER"
                },
                "shape": {
                    "class": "H5S_SCALAR"
                },
                "created": 1531672545.5978162,
                "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr3"
            },
            {
                "name": "attr4",
                "type": {
                    "base": "H5T_STD_I32LE",
                    "class": "H5T_INTEGER"
                },
                "shape": {
                    "class": "H5S_SCALAR"
                },
                "created": 1531667223.0914037,
                "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr4"
            },
            {
                "name": "attr5",
                "type": {
                    "base": "H5T_STD_U64LE",
                    "class": "H5T_INTEGER"
                },
                "shape": {
                    "class": "H5S_SCALAR"
                },
                "created": 1531672562.6758137,
                "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr5"
            },
            {
                "name": "attr6",
                "type": {
                    "strPad": "H5T_STR_NULLPAD",
                    "charSet": "H5T_CSET_ASCII",
                    "class": "H5T_STRING",
                    "length": 40
                },
                "shape": {
                    "class": "H5S_SIMPLE",
                    "dims": [2]
                },
                "created": 1531668943.5116098,
                "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr6"
            },
            {
                "name": "attr7",
                "type": {
                    "base": "H5T_STD_U64LE",
                    "class": "H5T_INTEGER"
                },
                "shape": {
                    "class": "H5S_SCALAR"
                },
                "created": 1531672573.915442,
                "href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes/attr7"
            }
        ],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/attributes", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e", "rel": "owner"}
        ]
    }

Related Resources
=================

* :doc:`DELETE_Attribute`
* :doc:`POST_Attributes`
* :doc:`../DatasetOps/POST_Dataset`
* :doc:`../DatatypeOps/POST_Datatype`
* :doc:`../GroupOps/POST_Group`
* :doc:`PUT_Attribute`


