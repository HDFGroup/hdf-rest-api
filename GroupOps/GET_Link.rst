**********************************************
GET Link
**********************************************

Description
===========
Returns information about a Link.

Requests
========

Syntax
------
.. code-block:: http

    GET /groups/<id>/links/<name> HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /groups/<id>/links/<name>?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the group the link is a member of.
* *<name>* is the URL-encoded name of the link.

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

link["title"]
^^^^^^^^^^^^^
The name of the link.

link["collection"]
^^^^^^^^^^^^^^^^^^
For hard links, the domain collection for which the object the link points to is a 
member of.  The value will be one of: "groups", "datasets", "datatypes".
For symbolic links, this element is not present.

link["class"]
^^^^^^^^^^^^^
Indicates the type of link.  One of the following values will be returned:

* H5L_TYPE_HARD: A direct link to a group, dataset, or committed datatype object in the domain
* H5L_TYPE_SOFT: A symbolic link that gives a path to an object within the domain (object may or may not be present).
* H5L_TYPE_EXTERNAL: A symbolic link to an object that is external to the domain
* H5L_TYPE_UDLINK: A user-defined link (this implementation only provides title and class for user-defined links)

link["h5path"]
^^^^^^^^^^^^^^
For symbolic links ("H5L_TYPE_SOFT" or "H5L_TYPE_EXTERNAL"), the path to the resource the
link references.  

link["h5domain"]
^^^^^^^^^^^^^^^^
For external links, the path of the external domain containing the object that is linked.
*Note:* The domain may or may not exist.  Use GET / with the domain to verify.

link["id"]
^^^^^^^^^^^^
For hard links, the uuid of the object the link points to.  For symbolic links this
element is not present

created
^^^^^^^
A timestamp giving the time the link was created in UTC (ISO-8601 format).

lastModified
^^^^^^^^^^^^
A timestamp giving the most recent time the group has been
modified in UTC (ISO-8601 format).

hrefs
^^^^^
An array of hypertext links to related resources.  See :doc:`../Hypermedia`.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request - Hard Link
--------------------------

.. code-block:: http

    GET /groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/g1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/g1

Sample Response - Hard Link
---------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 12 Jul 2018 19:53:43 GMT
    Content-Length: 560
    Etag: "70c5c4f2f7cac9f7f155fe026f4c492f65e3fb8e"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "lastModified": 1531174596.2666101,
        "created": 1531174596.2666101,
        "link": {
            "id": "g-be6eb652-83c5-11e8-b9ee-0242ac12000a",
            "collection": "groups",
            "class": "H5L_TYPE_HARD",
            "title": "g1"
        },
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016/links/g1", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "owner"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be6eb652-83c5-11e8-b9ee-0242ac12000a", "rel": "target"}
        ]
    }

Sample Request - Soft Link
--------------------------

.. code-block:: http

    GET /groups/g-beb56bba-83c5-11e8-87e1-0242ac12000c/links/slink HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-beb56bba-83c5-11e8-87e1-0242ac12000c/links/slink

Sample Response - Soft Link
---------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 12 Jul 2018 20:05:11 GMT
    Content-Length: 417
    Etag: "7bd777729ac5af261c85c7e3b87ef0045739bf77"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "created": 1531174597.2404227,
        "lastModified": 1531174597.2404227,
        "link": {
            "title": "slink",
            "class": "H5L_TYPE_SOFT",
            "h5path": "somevalue"
        },
        "hrefs": [
            {"rel": "self", "href": "hsdshdflab.hdfgroup.org/groups/g-beb56bba-83c5-11e8-87e1-0242ac12000c/links/slink"},
            {"rel": "home", "href": "hsdshdflab.hdfgroup.org/"},
            {"rel": "owner", "href": "hsdshdflab.hdfgroup.org/groups/g-beb56bba-83c5-11e8-87e1-0242ac12000c"}
        ]
    }

Sample Request - External Link
------------------------------

.. code-block:: http

    GET /groups/g-beaaa824-83c5-11e8-a8e6-0242ac120016/links/extlink HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-beaaa824-83c5-11e8-a8e6-0242ac120016/links/extlink

Sample Response - External Link
-------------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 12 Jul 2018 20:25:26 GMT
    Content-Length: 448
    Etag: "1b7a228acdb19f7259ed8a1b3ba4bc442b405ef9"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/groups/g-beaaa824-83c5-11e8-a8e6-0242ac120016/links/extlink", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-beaaa824-83c5-11e8-a8e6-0242ac120016", "rel": "owner"}
        ],
        "lastModified": 1531174596.5889843,
        "created": 1531174596.5889843,
        "link": {
            "h5path": "somepath",
            "title": "extlink",
            "class": "H5L_TYPE_EXTERNAL",
            "h5domain": "somefile"
        }
    }

Sample Request - User Defined Link
----------------------------------

*TODO*

.. code-block:: http

    GET /groups/0262c3a6-a069-11e4-8905-3c15c2da029e/links/udlink HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ 

Sample Response - User Defined Link
-----------------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Tue, 20 Jan 2015 05:56:00 GMT
    Content-Length: 576
    Etag: "2ab310eba3bb4282f84d643fcc30e591da485576"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "link": {
            "class": "H5L_TYPE_USER_DEFINED", 
            "title": "udlink"
        }, 
        "created": "2015-01-16T03:47:22Z",
        "lastModified": "2015-01-16T03:47:22Z", 
        "hrefs": [
            {"href": "http://tall_with_udlink.test.hdfgroup.org/groups/0262c3a6-a069-11e4-8905-3c15c2da029e/links/udlink", "rel": "self"}, 
            {"href": "http://tall_with_udlink.test.hdfgroup.org/groups/0260b214-a069-11e4-a840-3c15c2da029e", "rel": "root"}, 
            {"href": "http://tall_with_udlink.test.hdfgroup.org/", "rel": "home"}, 
            {"href": "http://tall_with_udlink.test.hdfgroup.org/groups/0262c3a6-a069-11e4-8905-3c15c2da029e", "rel": "owner"}
        ]       
    }

=================

* :doc:`DELETE_Link`
* :doc:`GET_Links`
* :doc:`PUT_Link`
 

 