**********************************************
PUT Links
**********************************************

Description
===========
Creates new links to groups, datasets, or committed datatypes.
Attempting to create a link to an object that does not exist will fail for a hard link, but succeed for soft and external links.

*Note*: The link creation will fail if a link with the same name in the same location already exists.

Requests
========

Syntax
------

To create a group link:

.. code-block:: http

    PUT /groups/<id>/links HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    PUT /groups/<id>/links?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the group that will contain the new links by default

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

Request Elements
----------------

This request requires exactly one of `links` or `grp_ids` (as a dictionary) to be provided.

links
^^^^
A dictionary of links to create. 
The keys are the link titles, and the bodies are the information describing the link to create (See "Request Elements" in :docs:`./Put_Link.rst`).

grp_ids
^^^^
A set of group UUIDs for groups that will contain the newly created links. May be a list or a dictionary. 
If it is a list, each link provided in 'links' will be written to each of these groups.
If it is a dictionary, it should map each group UUID to a dictionary whose `links` key contains a dictionary mapping each link title to its information.
If it is not provided, each link will be stored in the group from the request URL.

Responses
=========

Response Headers
----------------

This implementation of the operation uses only response headers that are common to 
most responses.  See :doc:`../CommonResponseHeaders`.

Response Elements
-----------------

On success, an empty JSON response will be returned.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request - Links in a single group
----------------------------------

This request creates a hard link called "hardlnk" to the root group. The created link itself resides within the root group.

.. code-block:: http

    PUT /groups/g-51db5cdf-08b64144-d953-d45780-3ec9cc/links HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 38
    Accept: */*
    Accept-Encoding: gzip, deflate


.. code-block:: json

    {
        "links": {
                "link1": {"id": "g-51db5cdf-08b64144-d953-d45780-3ec9cc"}, 
                "link2":{"h5path": "soft_link_target"}
        }
    }

Sample Response - Links in a single group 
-----------------------------------

.. code-block:: http

   HTTP/1.1 201 Created
   Date: Sun, 15 Jul 2018 15:07:03 GMT
   Content-Length: 13
   Content-Type: application/json
   Server: nginx/1.15.0

.. code-block:: json

    {}

Sample Request - Create all links in each target group
----------------------------------

.. code-block:: http

    PUT /groups/g-51db5cdf-08b64144-d953-d45780-3ec9cc/links HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 38
    Accept: */*
    Accept-Encoding: gzip, deflate


.. code-block:: json

    {
        "grp_ids": 
            [
                "g-5c8d8f09-4609b340-d405-0781ce-813bc8", 
                "g-5c8d8f09-4609b340-716d-e1bd7f-c77cbe"
            ],
        "links": 
            {
                "link3":{"id": "g-51db5cdf-08b64144-d953-d45780-3ec9cc"}, 
                "link4": {"h5path": "soft_link_target"}
            }
    }

Sample Request - Create different link(s) in each target group
----------------------------------

.. code-block:: http

    PUT /groups/g-51db5cdf-08b64144-d953-d45780-3ec9cc/links HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Content-Length: 38
    Accept: */*
    Accept-Encoding: gzip, deflate


.. code-block:: json
    {
        "grp_ids": 
        {
            "g-53cb7319-e27507cf-db43-fb916a-fd8f47": 
                {
                    "links": {"link5":{"id": "g-53cb7319-e27507cf-db43-fb916a-fd8f47"}}
                }, 
        
            "g-53cb7319-e27507cf-0749-7ba6b7-af764d": 
                {   
                    "links": {"link6": {"h5path": "soft_link_target"}}
                }
        }
    }


Related Resources
=================

* :doc:`DELETE_Link`
* :doc:`GET_Link`
* :doc:`GET_Links`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
 

 
