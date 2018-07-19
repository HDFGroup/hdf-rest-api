**********************************************
GET Value
**********************************************

Description
===========
Gets data values of a dataset.

Requests
========

Syntax
------
.. code-block:: http

    GET /datasets/<id>/value HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>

.. code-block:: http

    GET /datasets/<id>/value?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>

* *<id>* is the UUID of the requested dataset.

Request Parameters
------------------

select
^^^^^^
Optionally the request can provide a select value to indicate a hyperslab selection for
the values to be returned - i.e. a rectangular (in 1, 2, or more dimensions) region of 
the dataset.   Format is the following as a url-encoded value:

[dim1_start:dim1_end:dim1_step, dim2_start:dim2_end:dim2_step, ... , dimn_start:dimn_stop:dimn_step]

The number of tuples "start:stop:step" should equal the number of dimensions of the dataset. 

For each tuple:

* start must be greater than or equal to zero and less than the dimension extent
* stop must be greater than or equal to start and less than or equal to the dimension extent
* step is optional and if provided must be greater than 0.  If not provided, the step value for that dimension is assumed to be 1.

query
^^^^^
Optionally the request can provide a query value to select items from a dataset based on a 
condition expression.  E.g. The condition: "(temp > 32.0) & (dir == 'N')" would return elements 
of the dataset where the 'temp' field was greater than 32.0 and the 'dir' field was equal to 'N'.

Note: the query value needs to be url-encoded.

Note: the query parameter can be used in conjunction with the select parameter to restrict the return set to
the provided selection.

Note: the query parameter can be used in conjunction with the Limit parameter to limit the 
number of matches returned.

Note: Currently the query parameter can only be used with compound type datasets that are
one-dimensional.

Limit
^^^^^
If provided, a positive integer value specifying the maximum number of elements to return.
Only has an effect if used in conjunction with the query parameter.


Request Headers
---------------
This implementation of the operation supports the common headers in addition to the "Accept" header value
of "application/octet-stream".  Use this accept value if a binary response is desired.  Binary data will be
more efficient for large data requests.  If a binary response can be returned, the "Content-Type" response
header will be "application/octet-stream".  Otherwise the response header will be "json".

Note: Binary responses are only supported for datasets that have a fixed-length type
(i.e. either a fixed length primitive type or compound type that in turn consists of fixed-length types).  Namely
variable length strings and variable length data types will always be returned as JSON.

Note: if a binary response is returned, it will consist of the equivalent binary data of the "data" item in the JSON
response.  No data representing "hrefs" is returned.

For other request headers, see :doc:`../CommonRequestHeaders`

Responses
=========

Response Headers
----------------

This implementation of the operation uses only response headers that are common to 
most responses.  See :doc:`../CommonResponseHeaders`.

Response Elements
-----------------

On success, a JSON response will be returned with the following elements:

value
^^^^^
A json array (integer or string for scalar datasets) giving the values of the requested 
dataset region.

index
^^^^^
A list of indices for each element that met the query condition (only provided when 
the query request parameter is used).

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

    GET /datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/value HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/value

Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 15:02:38 GMT
    Content-Length: 758
    Etag: "788efb3caaba7fd2ae5d1edb40b474ba94c877a8"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "value": [
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
            [0, 2, 4, 6, 8, 10, 12, 14, 16, 18],
            [0, 3, 6, 9, 12, 15, 18, 21, 24, 27],
            [0, 4, 8, 12, 16, 20, 24, 28, 32, 36],
            [0, 5, 10, 15, 20, 25, 30, 35, 40, 45],
            [0, 6, 12, 18, 24, 30, 36, 42, 48, 54],
            [0, 7, 14, 21, 28, 35, 42, 49, 56, 63],
            [0, 8, 16, 24, 32, 40, 48, 56, 64, 72],
            [0, 9, 18, 27, 36, 45, 54, 63, 72, 81]
        ],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/value", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013", "rel": "owner"}
        ]
    }

Sample Request - Selection
--------------------------

.. code-block:: http

    GET /datasets/a299db70-ab57-11e4-9c00-3c15c2da029e/value?select=[1:9,1:9:2] HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

*Note the use of the -g option to disable cURL's URL globbing parser*

.. code-block:: bash

    $ curl -g -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/value?select=[1:9,1:9:2]

Sample Response - Selection
---------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 15:07:20 GMT
    Content-Length: 511
    Etag: "b370a3d34bdd7ebf57a496bc7f0da7bc5a1aafb9"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "value": [
            [1, 3, 5, 7],
            [2, 6, 10, 14],
            [3, 9, 15, 21],
            [4, 12, 20, 28],
            [5, 15, 25, 35],
            [6, 18, 30, 42],
            [7, 21, 35, 49],
            [8, 24, 40, 56]
        ],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013/value", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-be8bace4-83c5-11e8-90e7-0242ac120013", "rel": "owner"}
        ]
    }

Sample Request - Query
--------------------------

Get elements from a dataset where the 'open' field is greater than or equal to 2500 and the 'close' field is less than or equal to 3000.

.. code-block:: http

    GET /datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/value?query=(open%20%3E=%202500)%20%26%20(close%20%3C=%203000) HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /home/test_user1/h5pyd_test/3.4/query_compound_dset.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /home/test_user1/h5pyd_test/3.4/query_compound_dset.h5"
      hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/value?query=\(open%20%3E=%202500\)%20%26%20\(close%20%3C=%203000\)

Sample Response - Query
-------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 15:48:22 GMT
    Content-Length: 613
    Etag: "927b5ed89616896d3dce7df8bdddac058321076a"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "index": [1, 5, 6, 7, 8, 9],
        "value": [
            ["AAPL", "20170102", 3054, 2933],
            ["AMZN", "20170103", 3021, 2788],
            ["EBAY", "20170104", 2798, 2876],
            ["AAPL", "20170104", 2834, 2867],
            ["AMZN", "20170104", 2891, 2978],
            ["EBAY", "20170105", 2973, 2962]
        ],
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/value", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-a6b9f118-807b-11e8-a81a-0242ac12000b", "rel": "root"},
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "home"},
            {"href": "hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014", "rel": "owner"}
        ]
    }

Sample Request - Query Batch
-----------------------------

Get elements where the 'open' field is less than or equal to 3000.  Limit the number of results to 5.  

.. code-block:: http

    GET /datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/value?query=(open%20%3C=%203000)&Limit=5 HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*

Sample cURL command
-------------------

*URL enclosed in quotes to prevent shell from seeing ampersand*

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /home/test_user1/h5pyd_test/3.4/query_compound_dset.h5"
      "hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/value?query=(open%20%3C=%203000)&Limit=5"

Sample Response - Query Batch
-----------------------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 19 Jul 2018 15:59:56 GMT
    Content-Length: 576
    Etag: "927b5ed89616896d3dce7df8bdddac058321076a"
    Content-Type: application/json
    Server: nginx/1.15.0

.. code-block:: json

    {
        "index": [2, 6, 7, 8, 9],
        "value": [
            ["AMZN", "20170102", 2973, 3011],
            ["EBAY", "20170104", 2798, 2876],
            ["AAPL", "20170104", 2834, 2867],
            ["AMZN", "20170104", 2891, 2978],
            ["EBAY", "20170105", 2973, 2962]
        ],
        "hrefs": [
            {"rel": "self", "href": "hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014/value"},
            {"rel": "root", "href": "hsdshdflab.hdfgroup.org/groups/g-a6b9f118-807b-11e8-a81a-0242ac12000b"},
            {"rel": "home", "href": "hsdshdflab.hdfgroup.org/"},
            {"rel": "owner", "href": "hsdshdflab.hdfgroup.org/datasets/d-a6d2ee5c-807b-11e8-947e-0242ac120014"}
        ]
    }

Related Resources
=================

* :doc:`GET_Dataset`
* :doc:`POST_Value`
* :doc:`PUT_Value`


