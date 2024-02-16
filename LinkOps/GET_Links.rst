**********************************************
GET Links
**********************************************

Description
===========
Returns information on all the links in the target group(s).

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

where:    
    
* *<id>* is the UUID of the group containing the links
    
Request Parameters
------------------

domain
^^^^^
The domain containing the link's parent object. This 
parameter is optional if the domain is specified in the request headers.

pattern
^^^^
If this parameter is provided, HSDS will only return links that match this pattern according to Unix pathname pattern expansion rules.
This parameter is optional.

Limit
^^^^
An integer limit on the number of links that this request will return.
If not provided, HSDS will return all links.

Marker
^^^^
This parameter is used along with `Limit` to iterate through links in multiple requests.
This is the name of the link after which HSDS will begin returning links, in the requested order. (See 'CreateOrder')
For example, if the links `link1, link2, ... link100` exist in that order, then a request to return all links with the marker `link25` will return the set of links `link26 link27 ... link100`.

follow_links
^^^^
Boolean parameter specifying whether to recursively traverse groups under the target group and return links within those groups.
Default value is false. This parameter is optional.

CreateOrder
^^^^
Boolean parameter specifying whether the links should be returned in order of their creation.
Default value is false. This parameter is optional.
This requires the target group to track link creation order.

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

links
^^^^
A list of dictionaries containing information on each returned link. See :doc:`./GET_Link.rst` for the keys within these dicitonaries.

Special Errors
--------------

This implementation of the operation does not return special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request - Get links in group
--------------

Get the value all links in the group "g-aba449b2..."

.. code-block:: http

    GET /groups/g-aba449b2-92b57d6f-232c-c13a1a-3df5e7/links HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command - Get links in group
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/groups/g-aba449b2-92b57d6f-232c-c13a1a-3df5e7/links

Sample Response - Get links in group
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
        'links': [
            {
                'id': 'd-aba449b2-92b57d6f-1776-8e36e6-a48cb2', 
                'class': 'H5L_TYPE_HARD', 
                'created': 1707491709.028682, 
                'title': 'dsetlink', 
                'collection': 'datasets', 
                'target': 'http://localhost:5101/datasets/d-aba449b2-92b57d6f-1776-8e36e6-a48cb2', 
                'href': 'http://localhost:5101/groups/g-aba449b2-92b57d6f-232c-c13a1a-3df5e7/links/dsetlink'
            }, 
            {
                'id': 'g-aba449b2-92b57d6f-5d18-809b50-f727a7', 
                'class': 'H5L_TYPE_HARD', 
                'created': 1707491709.0103745, 
                'title': 'g1', 
                'collection': 'groups', 
                'target': 'http://localhost:5101/groups/g-aba449b2-92b57d6f-5d18-809b50-f727a7', 
                'href': 'http://localhost:5101/groups/g-aba449b2-92b57d6f-232c-c13a1a-3df5e7/links/g1'
            }, 
            {
                'h5path': 'soft_link_target', 
                'class': 'H5L_TYPE_SOFT', 
                'created': 1707491709.0028286, 
                'title': 'link2', 
                'href': 'http://localhost:5101/groups/g-aba449b2-92b57d6f-232c-c13a1a-3df5e7/links/link2'
            }, 
        ]
    }

Sample Request - Get all links in domain that start with 'dset'
--------------

Because the group"g-aba449b2..." is the root group, specifying recursive traversal with the `follow_links` parameter
will search over all links in the domain that follow the pattern `dset*`.

.. code-block:: http

    GET /groups/g-00659343-38b3da69-88ed-1bcbb0-3b52e1/links?pattern=dset*&follow_links=1 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*


Related Resources
=================

* :doc:`DELETE_Link`
* :doc:`GET_LinkValue`
* :doc:`GET_Links`
* :doc:`../DatasetOps/GET_Dataset`
* :doc:`../DatatypeOps/GET_Datatype`
* :doc:`../GroupOps/GET_Group`
* :doc:`PUT_Link`


 