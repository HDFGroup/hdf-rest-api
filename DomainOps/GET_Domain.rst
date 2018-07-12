**********************************************
GET Domain
**********************************************

Description
===========
This operation retrieves information about the requested domain.

*Note:* If the HDF Dynamic DNS Server (see https://github.com/HDFGroup/dynamic-dns) is running, 
the operations can specify the domain as part of the URI.  Example:  
http://tall.data.hdfgroup.org:7253/ 
returns data about the domain "tall" hosted on data.hdfgroup.org.  
The DNS server will determine the proper IP that maps to this domain.

If the DNS Server is not setup, specify the desired domain in the "X-Hdf-domain" line of the http
header.

Alternatively, the domain can be specified as a query parameter.  Example:

http://127.0.0.1:7253?domain=tall.data.hdfgroup.org.

If no X-Hdf-domain value is supplied, the default Table of Contents (TOC) domain is returned.

Requests
========

Syntax
------
.. code-block:: http

    GET / HTTP/1.1
    X-Hdf-domain: DOMAIN
    Authorization: <authorization_string>
    
.. code-block:: http

    GET /?domain=DOMAIN HTTP/1.1
    Authorization: <authorization_string>
    
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

root
^^^^
The UUID of the root group of this domain.

created
^^^^^^^
A timestamp giving the time the domain was created in UTC (ISO-8601 format).

owner
^^^^^
The user which owns the domain.

lastModified
^^^^^^^^^^^^
A timestamp giving the most recent time that any content in the domain has been
modified in UTC (ISO-8601 format).

hrefs
^^^^^
An array of links to related resources.  See :doc:`../Hypermedia`.

Special Errors
--------------

This implementation of the operation does not return any special errors.  For general 
information on standard error codes, see :doc:`../CommonErrorResponses`.

Examples
========

Sample Request
--------------

.. code-block:: http

    GET / HTTP/1.1
    Host: hsdshdflab.hdfgroup.org
    X-Hdf-domain: /shared/tall.h5
    Accept-Encoding: gzip, deflate
    Accept: */*
    
Sample cURL command
-------------------

.. code-block:: bash

    $ curl -X GET --header "X-Hdf-domain: /shared/tall.h5" hsdshdflab.hdfgroup.org/
    
Sample Response
---------------

.. code-block:: http

    HTTP/1.1 200 OK
    Date: Thu, 12 Jul 2018 16:08:23 GMT
    Content-Length: 638
    Etag: "e45bef255ffc0530c33857b88b15f551f371de38"
    Content-Type: application/json
    Server: nginx/1.15.0
    
.. code-block:: json

    {
        "root": "g-be5996fa-83c5-11e8-a8e6-0242ac120016",
        "created": 1531174596.0696847,
        "owner": "admin",
        "class": "domain",
        "lastModified": 1531174596.0696847,
        "hrefs": [
            {"href": "hsdshdflab.hdfgroup.org/", "rel": "self"},
            {"href": "hsdshdflab.hdfgroup.org/datasets", "rel": "database"},
            {"href": "hsdshdflab.hdfgroup.org/groups", "rel": "groupbase"},
            {"href": "hsdshdflab.hdfgroup.org/datatypes", "rel": "typebase"},
            {"href": "hsdshdflab.hdfgroup.org/groups/g-be5996fa-83c5-11e8-a8e6-0242ac120016", "rel": "root"},
            {"href": "hsdshdflab.hdfgroup.org/acls", "rel": "acls"},
            {"href": "hsdshdflab.hdfgroup.org/?domain=/shared", "rel": "parent"}
        ]
    }
    
Related Resources
=================

* :doc:`DELETE_Domain`
* :doc:`../GroupOps/GET_Group`
* :doc:`PUT_Domain`
 

 