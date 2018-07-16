**********************************************
GET Attribute
**********************************************

Description
===========
Gets the specified attribute of a dataset, group, or committed datatype.

Requests
========

Syntax
------

To get an attribute of a group:

.. code-block:: http

    GET /groups/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /groups/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get an attribute of a dataset:

.. code-block:: http

    GET /datasets/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datasets/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

To get an attribute of a datatype:

.. code-block:: http

    GET /datatypes/<id>/attributes/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datatypes/<id>/attributes/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

where:    
    
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

type
^^^^
A JSON object representing the type of the attribute.  See :doc:`../Types/index` for 
details of the type representation.

shape
^^^^^

A JSON object that represents the dataspace of the attribute.  Subkeys of shape are:

class: A string with one of the following values:

 * H5S_NULL: A null dataspace, which has no elements
 * H5S_SCALAR: A dataspace with a single element (although possibly of a complex datatype)
 * H5S_SIMPLE: A dataspace that consists of a regular array of elements
 
dims: An integer array whose length is equal to the number of dimensions (rank) of the 
dataspace.  The value of each element gives the current size of each dimension.  Dims
is not returned for H5S_NULL or H5S_SCALAR dataspaces.

value
^^^^^
A JSON array (or string or number for scalar datasets) giving the values of the requested 
attribute.

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

Get an attribute named "attr1" from a group with UUID: "g-be5996fa-...".

.. code-block:: http

    GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes/attr1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes/attr1

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Sun, 15 Jul 2018 16:17:44 GMT
    Content-Length: 522
    Etag: "55b2e2ce2d3a2449a49cfd76c4dae635ec43a150"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "name": "attr1",
        "type": {
            "base": "H5T_STD_I8LE",
            "class": "H5T_INTEGER"
        },
        "shape": {
            "class": "H5S_SIMPLE",
            "dims": [10]
        },
        "created": 1531174596.117736,
        "lastModified": 1531174596.117736,
        "value": [97, 98, 99, 100, 101, 102, 103, 104, 105, 0],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/attributes/attr1", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "owner"}
        ]
    }

Related Resources
=================

* :doc:`DELETE_Attribute`
* :doc:`GET_Attributes`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
* :doc:`PUT_Attribute`


 