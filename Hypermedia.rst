*************************
Hypermedia
*************************

The HDF REST API supports the REST convention of **HATEOAS** or *Hypermedia as the Engine of 
Application State*.  The idea (see http://en.wikipedia.org/wiki/HATEOS for a full 
explanation) is that each response includes links to resources related to 
the requested resource.

For example, consider the request for a dataset: ``GET /datasets/<id>``.  The response
will be a JSON representation of the dataset describing its type, shape, and other
aspects.  Related resources to the dataset would include:

 * the dataset's attributes
 * the dataset's value
 * the dataset collection of the domain
 * the root group of the domain the dataset is in
 * the domain resource
 
So the ``GET /datasets/<id>`` response includes a key ``hrefs`` that contains
a JSON array.  Each array element has a key: ``href`` - the related resource, and a key:
``rel`` that denotes the type of relation.   Example:

.. code-block:: json
       
    {
        "hrefs": [
            {"href": "http://tall.test.hdfgroup.org/datasets/<id>", "rel": "self"}, 
            {"href": "http://tall.test.hdfgroup.org/groups/<id>", "rel": "root"}, 
            {"href": "http://tall.test.hdfgroup.org/datasets/<id>/attributes", "rel": "attributes"}, 
            {"href": "http://tall.test.hdfgroup.org/datasets/<id>/value", "rel": "data"}, 
            {"href": "http://tall.test.hdfgroup.org/", "rel": "home"}
        ] 
    }

This enables clients to "explore" the API without detailed knowledge of it.

This is the list of relations used in the HDF REST API:

 * attributes - the attributes of the resource 
 * data - the resource's data (used for datasets)
 * database - the collection of all datasets in the domain
 * groupbase - the collection of all groups in the domain
 * home - the domain the resource is a member of
 * owner - the containing object of this resource (e.g. the group an attribute is a member of)
 * root - the root group of the domain the resource is a member of
 * self - this resource
 * typebase - the collection of all committed types in the domain
