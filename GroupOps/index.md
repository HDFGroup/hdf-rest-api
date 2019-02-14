Groups
======

Groups are objects that can be used to organize objects within a domain. Groups contain *links* which can reference other objects (datasets, groups or committed datatypes). There are four different types of links that can be used:

-   hard: A direct link to a group, dataset, or committed datatype object in the domain.
-   soft: A symbolic link that gives a path to an object within the domain (object may or may not be present).
-   external: A symbolic link to an object that is external to the domain.
-   user-defined: A user-defined link (this implementation only provides title and class for user-defined links).

Groups all have attributes which can be used to store meta-data about the group.

Creating Groups
---------------

Use the POST\_Group operation to create new Groups. Initially the new group will have no links and no attributes.

Getting information about Groups
--------------------------------

Use GET\_Group to get information about a group such as the attribute count, link count and creation and modification times.

To retrieve the UUIDs of all the groups in a domain, use GET\_Groups.

To retrieve the links of a group use GET\_Links. Use GET\_Link to get information about a specific link.

To get a group's attributes, use ../AttrOps/GET\_Attributes.

Updating Links
--------------

To create a hard, soft, or external link, use PUT\_Link.

To delete a link use DELETE\_Link.

*Note*: deleting a link doesn't delete the object that it refers to.

Deleting Groups
---------------

Use DELETE\_Group to remove a group. All attributes and links of the group will be deleted.

*Note:* deleting a group will not delete any objects (datasets or other groups) that the the group's links point to. These objects may become *anonymous*, i.e. they are not referenced by any link, but can still be accessed via a `GET` request with the object UUID.

List of Operations
------------------
