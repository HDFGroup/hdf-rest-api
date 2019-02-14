Attributes
==========

Like datasets (see ../DatasetOps/index), attributes are objects that contain a homogeneous collection of elements and have associated type information. Attributes are typically small metadata objects that describe some aspect of the object (dataset, group, or committed datatype) that contains the attribute.

Creating Attributes
-------------------

Use PUT\_Attribute to create an attribute. If there is an existing attribute with the same name, it will be overwritten by this request. You can use GET\_Attribute to inquire if the attribute already exists or not. When creating an attribute, the attribute name, type and shape (for non-scalar attributes) is included in the request.

Reading and Writing Data
------------------------

Unlike datasets, an attribute's data can not be read or written partially. Data can only be written as part of the PUT request. Reading the data of an attribute is done by GET\_Attribute.

Listing attributes
------------------

Use GET\_Attributes to get information about all the attributes of a group, dataset, or committed datatype.

Deleting Attributes
-------------------

Use DELETE\_Attribute to delete an attribute.

List of Operations
------------------
