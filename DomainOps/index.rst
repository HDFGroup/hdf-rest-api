#######################
Domains
#######################

In the HDF REST API, domains are containers for a related collection of resources, similar to a
file in the traditional HDF5 library.  In the HDF Kita implementation of the HDF REST API,
domains *are* files, but in general the HDF REST API supports alternative implementations 
(e.g. data that is stored in a database).
Most operations of the service act on a domain resource that is provided in 
the *X-Hdf-domain* http header or (alternatively) the domain query parameter.

Mapping of file paths to domain names
-------------------------------------

To convert a file path to a domain name:

#. Remove the extension
#. Determine the path relative to the data directory
#. Replace '/' with '.'
#. Reverse the path
#. Add the domain suffix (using the domain config value)

As an example, consider a server installation where the data directory is '/data'
and an HDF5 "file" is located at ``/data/myfolder/an_hdf_file.h5`` and ``hdfgroup.org``
is the base domain.  The above sequence of steps would look like the following:

#. /data/myfolder/an_hdf_file
#. myfolder/an_hdf_file
#. myfolder.an_hdf_file
#. an_hdf_file.myfolder
#. an_hdf_file.myfolder.hdfgroup.org

The final expression is what should be used in the Host field for any request that accesses
that file.  

For path names that include non-alphanumeric characters, replace any such characters with 
the string '%XX' where XX is the hexadecimal value of the character.  For example:

``this.file.has.dots.h5``

becomes:

``this%2Efile%2Ehase%2Edots``


Creating Domains
----------------
Use :doc:`PUT_Domain` to create a domain.  The domain name must follow DNS conventions
(e.g. two consecutive "dots" are not allowed).  After creation, the domain will contain
just one resource, the root group.  

Getting Information about Domains
---------------------------------

Use :doc:`GET_Domain` to retrieve information about a specific domain (specified in the Host
header), including the UUID of the domain's root group.  If the Host value is not supplied,
the service returns information in the auto-generated Table of Contents (TOC) that provides
information on domains that are available.

Deleting Domains
----------------
Use :doc:`DELETE_Domain` to delete a domain.  All resources within the domain will be
deleted!

The TOC domain cannot be deleted.

List of Operations
------------------

.. toctree::
   :maxdepth: 1

   DELETE_Domain
   GET_Domain
   PUT_Domain
    
    
