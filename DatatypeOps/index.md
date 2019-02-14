Committed Datatypes
===================

Committed datatypes (also known as "named types"), are objects that describe types. These types can be used in the creation of datasets and attributes.

Committed datatypes can be linked to from a Group and can contain attributes, just like a dataset or group object.

Creating committed datatypes
----------------------------

Use POST\_Datatype to create a new datatype. A complete description of the type must be sent with the POST request.

Getting information about a committed datatype
----------------------------------------------

Use the GET\_Datatype operation to retrieve information about a committed datatype. To list all the committed datatypes within a domain use GET\_Datatypes. To list the committed types linked to a particular group use ../GroupOps/GET\_Links and examine link objects with a "collection" key of "datatypes".

Deleting committed datatypes
----------------------------

Use DELETE\_Datatype to delete a datatype. Links from any group to the datatype will be deleted.

List of Operations
------------------
