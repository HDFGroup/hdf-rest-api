**********************************************
GET Links
**********************************************

Description
===========
Returns all the links for a given group.

Requests
========

Syntax
------
.. code-block:: http

    GET /groups/<id>/links HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /groups/<id>/links?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the group the links to be returned are a member of.

Request Parameters
------------------
This implementation of the operation uses the following request parameters (both 
optional):

Limit
^^^^^
If provided, a positive integer value specifying the maximum number of links to return.

Marker
^^^^^^
If provided, a string value indicating that only links that occur after the
marker value will be returned.


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

links
^^^^^
An array of JSON objects giving information about each link returned.
See :doc:`GET_Link` for a description of the link response elements.

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

    GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 12 Jul 2018 20:34:59 GMT
    Content-Length: 916
    Etag: "49edcce6a8f724108d41d52c98002d6255286ff8"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "links": [
            {
                "id": "g-be6eb652-83c5-11e8-b9ee-0242ac12000a",
                "title": "g1",
                "class": "H5L_TYPE_HARD",
                "collection": "groups",
                "created": 1531174596.2666101,
                "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/g1",
                "target": "hsdshdflab.hdfgroup.org/groups/g-be6eb652-83c5-11e8-b9ee-0242ac12000a"
            },
            {
                "id": "g-bf15f8b8-83c5-11e8-8ad9-0242ac120009",
                "title": "g2",
                "class": "H5L_TYPE_HARD",
                "collection": "groups",
                "created": 1531174597.2827835,
                "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/g2",
                "target": "hsdshdflab.hdfgroup.org/groups/g-bf15f8b8-83c5-11e8-8ad9-0242ac120009"
            }
        ],
        "hrefs": [
            {"rel": "self", "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links"},
            {"rel": "home", "href": "hsdshdflab.hdfgroup.org/"},
            {"rel": "owner", "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016"}
        ]
    }

Sample Request Batch
--------------------

.. code-block:: http

    GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links?Marker=g1&Limit=1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

*URL enclosed in quotes to prevent shell from seeing ampersand*

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links?Marker=g1&Limit=1"

Sample Response Batch
---------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 12 Jul 2018 20:41:25 GMT
    Content-Length: 597
    Etag: "221affdeae54076d3493ce8ce0ed80ddb89c6e27"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "links": [
            {
                "id": "g-bf15f8b8-83c5-11e8-8ad9-0242ac120009",
                "title": "g2",
                "class": "H5L_TYPE_HARD",
                "collection": "groups",
                "created": 1531174597.2827835,
                "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/g2",
                "target": "hsdshdflab.hdfgroup.org/groups/g-bf15f8b8-83c5-11e8-8ad9-0242ac120009"
            }
        ],
        "hrefs": [
            {"rel": "self", "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links"},
            {"rel": "home", "href": "hsdshdflab.hdfgroup.org/"},
            {"rel": "owner", "href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016"}
        ]
    }

Related Resources
=================

* :doc:`DELETE_Link`
* :doc:`GET_Link`
* :doc:`GET_Group`
* :doc:`PUT_Link`


 