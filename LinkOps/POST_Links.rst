**********************************************
POST Links
**********************************************

Description
===========
Gets information on a collection of links from the target group(s).

Requests
========

Syntax
------

.. code-block:: http

    POST /groups/<id>/links HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    POST /groups/<id>/links?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

where:    

* *<id>* is the UUID of the group searched for the provided links if `group_ids` is not provided.

Request Parameters
------------------
A collection of links to be read must be provided in one of two ways.

If the same links are to be read from each target object, the links'
names should be provided in the list `titles`.

If each target object has a unique set of links which you wish to read, then
the target link names should be provided as the value of each key within the
dictionary `group_ids`.

titles
^^^^^
A list of the titles of links which should be read from the target group(s).
This parameter is used when the same links are to be read from all target groups.

group_ids
^^^^^
The collection of groups to read links from.

If the same link names are to be read from all groups, `group_ids` should be a list.

If each target object has a different set of links which should be read from it, `group_ids` should be 
a dictionary mapping each target object id to the names of the links to read from it.

follow_links (TBD)
^^^^
Boolean parameter specifying whether to recursively traverse groups under the target group(s) while searching for links with the provided titles.
Default value is false. This parameter is optional.

domain
^^^^^
The domain containing the links to retrieve. This 
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

On success, a JSON response will be returned with the following elements:


links
^^^^^^^^^^
A list of JSON objects with a dictionary element for each returned link.
See :doc:`GET_Link` for a description of these dictionaries.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

Get the link named "link1" and "link2" from the group "g-63ea97d5..."

.. code-block:: http

    POST /groups/g-63ea97d5-5ed538a7-eb62-1f5dd6-5db02f/links HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

.. code-block:: json
    {
        "titles": 
            [
                "link1",
                "link2"
            ]
    }

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
        'links': [
            {
                'class': 'H5L_TYPE_HARD', 
                'created': 1707496239.3933172, 
                'id': 'g-63ea97d5-5ed538a7-eb62-1f5dd6-5db02f', 
                'title': 'link1'
            },
            {
                'class': 'H5L_TYPE_SOFT',
                'created': 1707497727.0149524,
                'h5path': 'soft_link_target', 
                'title': 'link2'
            }
        ]
    }

Sample Request - Read same link title from multiple groups (TBD - #307)
---------------------------

.. code-block:: http

    POST /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/links HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

.. code-block:: json

    {
        "titles": 
            [
                "link1"
            ], 
        "group_ids": 
            [
                "g-83a603fe-5e32fffe-0b2e-8b76d6-ba7776", 
                "g-83a603fe-5e32fffe-19a6-d55996-9adb95"
            ]
    }

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
        'links': 
            {
                'g-83a603fe-5e32fffe-0b2e-8b76d6-ba7776': 
                    [
                        {
                            'class': 'H5L_TYPE_HARD', 
                            'created': 1707511487.5895188, 
                            'id': 'g-83a603fe-5e32fffe-0b2e-8b76d6-ba7776', 
                            'title': 'link1'
                        }
                    ], 
                    
                'g-83a603fe-5e32fffe-19a6-d55996-9adb95': 
                    [
                        {
                            'class': 'H5L_TYPE_SOFT', 
                            'created': 1707511487.603207, 
                            'h5path': '/g1/g2', 
                            'title': 'link1'
                        }
                    ]
            }
    }

Related Resources
=================

* :doc:`DELETE_Link`
* :doc:`POST_Links`
* :doc:`../DatasetOps/POST_Dataset`
* :doc:`../DatatypeOps/POST_Datatype`
* :doc:`../GroupOps/POST_Group`
* :doc:`PUT_Link`


