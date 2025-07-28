**********************************************
GET Link
**********************************************

Description
===========
Return information about a link.

Requests
========

Syntax
------

.. code-block:: http

    GET /groups/<id>/links/<title> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /groups/<id>/links/<title>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

where:    
    
* *<id>* is the UUID of the group containing the link
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

Responses
=========

Response Headers
----------------

This implementation of the operation uses only response headers that are common to 
most responses.  See :doc:`../CommonResponseHeaders`.

Response Elements
-----------------

On success, a JSON response will be returned containing information on the group, dataset, or committed datatype pointed to by the link.
See :doc:`../GroupOps/GET_Group`, :doc:`../DatatypeOps/GET_Datatype`, :doc:`../DatasetOps/GET_Dataset`.

link
^^^^
A dictionary containing information about the link.
`titles` contains the name of the link.
`class` contains the link's class - one of `H5L_TYPE_HARD`, `H5L_TYPE_SOFT`, or `H5L_TYPE_EXTERNAL`.
`id` contains the link's UUID.
`collection` contains the collection (e.g. object type) of the target object - one of `groups`, `datasets`, or `datatypes`.

created
^^^^
UNIX time at which the link was created.

lastModified
^^^^
UNIX time at which the link was last modified.

hrefs
^^^^
An array of links to related resources.  See :doc:`../Hypermedia`.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request - Get group link
--------------

Get the value of link named "link1" that targets a group, from a group with UUID: "g-be5996fa-...".

.. code-block:: http

    GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/link1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command - Get group link
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/link1

Sample Response - Get group link
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
        'link': {
            'title': 'link1', 
            'class': 'H5L_TYPE_HARD', 
            'id': 'g-ba2ffa5b-58320baa-32a7-72d3d0-ba8322', 
            'collection': 'groups'
        }, 
        'created': 1707427765.1488154, 
        'lastModified': 1707427765.1488154, 
        'hrefs': 
            [
                {'rel': 'self', 'href': 'http://localhost:5101/groups/g-ba2ffa5b-58320baa-32a7-72d3d0-ba8322/links/link1'}, 
                {'rel': 'home', 'href': 'http://localhost:5101/'}, 
                {'rel': 'owner', 'href': 'http://localhost:5101/groups/g-ba2ffa5b-58320baa-32a7-72d3d0-ba8322'}, 
                {'rel': 'target', 'href': 'http://localhost:5101/groups/g-ba2ffa5b-58320baa-32a7-72d3d0-ba8322'}
            ]
    }

Sample Request - Get dataset link
--------------

Get the value of link named "dsetlink" that targets a dataset, from a group with UUID: "g-be5996fa-...".

.. code-block:: http

    GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/dsetlink HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample Response - Get dataset link
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
    'link': {'title': 'dsetlink', 'class': 'H5L_TYPE_HARD', 'id': 'd-52d9f0ff-f99bd504-f3f5-27f871-576bfc', 'collection': 'datasets'}, 
    'created': 1707428167.7223468, 
    'lastModified': 1707428167.7223468, 
    'hrefs': 
        [
            {'rel': 'self', 'href': 'http://localhost:5101/groups/g-52d9f0ff-f99bd504-da51-787771-135d8c/links/dsetlink'}, 
            {'rel': 'home', 'href': 'http://localhost:5101/'}, {'rel': 'owner', 'href': 'http://localhost:5101/groups/g-52d9f0ff-f99bd504-da51-787771-135d8c'}, 
            {'rel': 'target', 'href': 'http://localhost:5101/datasets/d-52d9f0ff-f99bd504-f3f5-27f871-576bfc'}
        ]
    }



Related Resources
=================

* :doc:`DELETE_Link`
* :doc:`GET_LinkValue`
* :doc:`GET_Links`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
* :doc:`PUT_Link`


 