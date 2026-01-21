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
with the same name, it will be overwritten by this request.  You can use
:doc:`GET_Attribute` to inquire if the attribute already exists or not.
When creating an attribute, the attribute name, type and shape (for non-scalar
attributes) is included in the request.


Reading and Writing Data
-------------------------
To write data to an attribute, use the :doc:`../DatasetOps/PUT_Value` operation.

To read data from an attribute, use the :doc:`../DatasetOps/GET_Value` operation.

Unlike datasets, an attribute's data can not be read or written partially. As part of
the :doc:`../DatasetOps/GET_Value` operation, the entire set of data values for the
attribute is returned. As part of the :doc:`../DatasetOps/PUT_Value` operation, the
entire set of data values for the attribute must be updated.

Listing attributes
------------------
Use :doc:`GET_Attributes` to get information about all the attributes of a group, 
dataset, or committed datatype.

Deleting Attributes
-------------------

Use :doc:`DELETE_Attribute` to delete an attribute.

List of Operations
------------------

.. toctree::
   :maxdepth: 1

   DELETE_Attribute
   GET_Attribute
   GET_Attributes
   PUT_Attribute
   ../DatasetOps/GET_Value
   ../DatasetOps/PUT_Value

