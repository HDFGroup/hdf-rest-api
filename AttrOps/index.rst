########################
Attributes
########################

Like datasets (see :doc:`../DatasetOps/index`), attributes are objects that contain a 
homogeneous collection of elements
and have associated type information.  Attributes are typically small metadata objects
that describe some aspect of the object (dataset, group, or committed datatype) that 
contains the attribute.

Creating Attributes
--------------------

Use :doc:`PUT_Attribute` to create an attribute.  If there is an existing attribute
with the same name, and the `replace` parameter is provided, it will be overwritten 
by this request.  :doc:`PUT_Attributes` can be used to create/replace a collection
of attributes on a single object. You can use :doc:`GET_Attribute` to inquire if 
the attribute already exists or not. When creating an attribute, the attribute 
name, type and shape (for non-scalar attributes) is included in the request.


Reading and Writing Data
-------------------------
Unlike datasets, an attribute's data can not be
read or written partially.  Data can only be written as part of the PUT request.  
Reading the data of an attribute is done by :doc:`GET_Attribute` or :doc:`GET_AttributeValue`.

Listing attributes
------------------
Use :doc:`GET_Attributes` to get information about all the attributes of a group, 
dataset, or committed datatype.

To get information about a collection of attributes on a group, dataset, or
committed datatype, use :doc:`POST_Attributes`.

Deleting Attributes
-------------------

Use :doc:`DELETE_Attribute` to delete an attribute, and :doc:`DELETE_Attributes` to
delete a collection of attributes by name. :doc:`DELETE_Attributes` must be used for
attributes with names containing '/' or '#'.

List of Operations
------------------

.. toctree::
   :maxdepth: 1

   DELETE_Attribute
   DELETE_Attribute
   GET_Attribute
   GET_AttributeValue
   GET_Attributes
   PUT_Attribute
   PUT_Attributes
 
    
    
