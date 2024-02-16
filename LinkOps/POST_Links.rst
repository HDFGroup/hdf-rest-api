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
A collection of links to be read must be provided in one of three ways.

If the same links are to be read from each target group, the links'
names should be provided in the list `titles`.

If each target object has a unique set of links which you wish to read, then
the target link names should be provided as the value of each key within the
dictionary `grp_ids`.

Alternatively, to return all links with titles that match a pattern, the `pattern` parameter may be provided.

By default, the group provided in the request URL is targeted. If group ids are provdied via `grp_ids`, those group(s) will be targeted instead.

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

follow_links
^^^^
Boolean parameter specifying whether to recursively traverse groups under the target group(s) while searching for links with the provided titles.
Default value is false. This parameter is optional.

pattern
^^^^
If this parameter is provided, HSDS will only return links that match this pattern according to Unix pathname pattern expansion rules.
This parameter is optional.

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
If a single group is targeted, this is a list of objects which each describe a link.
If more than one group is targeted or searched, this is an object mapping every searched group id
to each group's returned link objects. A group id may be mapped to an empty object if no links were found under that group.

See :doc:`GET_Link` for a description of the individual link objects.

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

Sample Request - Read same link title from multiple groups
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
        "grp_ids":
            [
                "g-83a603fe-5e32fffe-0b2e-8b76d6-ba7776", 
                "g-83a603fe-5e32fffe-19a6-d55996-9adb95"
            ]
    }

Sample Response - Read same link from multiple groups
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
        'links': {
            'g-a0ff86b8-a4759af1-2877-0e302c-fd1279': [
                {'id': 'g-a0ff86b8-a4759af1-2877-0e302c-fd1279', 'class': 'H5L_TYPE_HARD', 'created': 1708100156.1945097, 'title': 'link1'}
            ],
            'g-a0ff86b8-a4759af1-18b6-fd1c34-2f30c2': [
                {'id': 'g-a0ff86b8-a4759af1-2877-0e302c-fd1279', 'class': 'H5L_TYPE_HARD', 'created': 1708100156.2084465, 'title': 'link1'}
            ]
        }
    }

Sample Request -  Search for link in all subgroups
---------------------------

In this example, `link2` exists in a subgroup, and the request is sent to the root group with the `follow_links` parameter enabled to search all subgroups.

.. code-block:: http

    POST /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/links?follow_links=1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

.. code-block:: json

    {
        "titles":
            [
                "link2"
            ],
    }

Sample Response - Search for link in all subgroups
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
        'links': {
            'g-6c472826-634d6313-e4cf-a0aeeb-c5eb9b': [
                {'h5path': 'soft_link_target', 'class': 'H5L_TYPE_SOFT', 'created': 1708101851.0649424, 'title': 'link2'}
            ],
            'g-6c472826-634d6313-9c71-ea8f30-337e96': []
        }
    }

Sample Request -  Search for name pattern in all subgroups
---------------------------

This examples searches all groups recursively from the group with id `g-...3511.` and returns all links with titles that match the pattern `*1`.
Note that in this case, the group used in the request URL is ignored.

.. code-block:: http

    POST /groups/g-45f464d8-883e-11e8-a9dc-0242ac12000e/links?follow_links=1&pattern=*1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

.. code-block:: json

    {
        "grp_ids": [
            "g-791e9ee1-692bbd99-f196-1669e1-a33511"
        ]
    }

Sample Response - Search for link in all subgroups
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
        'links': {
            'g-791e9ee1-692bbd99-f196-1669e1-a33511': [
                    {'id': 'g-791e9ee1-692bbd99-ceb3-4d4bdd-93e70b', 'class': 'H5L_TYPE_HARD', 'created': 1708102965.0489032, 'title': 'g1'},
                    {'id': 'g-791e9ee1-692bbd99-f196-1669e1-a33511', 'class': 'H5L_TYPE_HARD', 'created': 1708102965.0385375, 'title': 'link1'}
                ],
            'g-791e9ee1-692bbd99-ceb3-4d4bdd-93e70b': [
                {'id': 'g-791e9ee1-692bbd99-f196-1669e1-a33511', 'class': 'H5L_TYPE_HARD', 'created': 1708102965.0537355, 'title': 'link1'}
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


