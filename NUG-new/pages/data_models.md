---
title: NetCDF Data Models
last_updated: 2021-04-06
sidebar: nnug_sidebar
toc: false
permalink: data_models.html
---
## Start - Source material info
{%include note.html content="Two sources of text used for this page:" %}
{%include note.html content="Source 1: NUG/netcdf_data_set_components.md" %}
{%include note.html content="Source 2: * [\"Developing Conventions for netCDF-4\"](https://www.unidata.ucar.edu/software/netcdf/papers/nc4_conventions.html) by Rew and Caron" %}
{%include note.html content="See notes below for more details." %}
<!--
Also see [Unidata's Common Data Model (CDM)](https://docs.unidata.ucar.edu/netcdf-java/current/userguide/common_data_model_overview.html) and [CDM NetCDF Mapping](https://docs.unidata.ucar.edu/netcdf-java/current/userguide/cdm_netcdf_mapping.html)
-->
## End - Source material info

<!--
  Where should we put "Limitations of NetCDF" section in NUG/netcdf_introduction.md ?
  - Is the "2 GiBytes size limit" in CDF-1 part of the data model?
  - Is the "only one unlimited dimension" limitation in CDF-1, 2, and -5 part of the data model?
-->

NetCDF has two main data models. 
* The netCDF Classic Data Model, which represents a dataset with named variables, dimensions, and attributes.
* The netCDF Enhanced Data Model, which adds hierarchical structure, string and unsigned integer types, and user-defined types.  

Some file formats and encodings support subsets of the Enhanced Data Model
or minor extensions to the Classic Data Model.
For example, CDF-5 extends the Classic Data Model with the addition of
support for unsigned integer and 64-bit integer data types.
Whereas ncZarr supports a subset of the Enhanced Data Model
that does not include support for strings, user-defined types, or VLEN dimensions.

???? Should we use CDL, C, Fortran, Java to clarify some aspects of the data model? (E.g., dimension order, data types, etc.)
If so we should mention something about them here (with pointers to full descriptions).
Or add more details (and pointers) to CDL and libraries in index.md and netcdf_overview.md.

<!-- Do we explain the difference between Enhanced DM and CDM?  -->


## NetCDF Classic Data Model
{%include note.html content="
Text from [Rew & Caron] Section \"NetCDF Data Models\" paragraph 2
" %}

The Classic Data Model represents data sets using named variables, dimensions, and attributes.
A variable is a multidimensional array whose elements are all of the same type.
The shape of each variable is specified by a list of dimensions.
Dimensions are named axes that have a length.
Variables may share dimensions, indicating a common grid.
One dimension in a dataset may be of unlimited length, so data may be efficiently appended to variables along that dimension.
A variable may also have attributes, which are associated named values.
Variables and attributes have one of six atomic data types: char, byte, short, int, float, or double.

A diagram of the Classic Data Model exhibits its simplicity:
{% include image.html file="nc-classic-uml.png" alt="netCDF Classic Data Model UML" caption="" %}

## NetCDF Enhanced Data Model
{%include note.html content="
Text from [Rew & Caron] Section \"NetCDF Data Models\", paragraph 5
and from NUG/netcdf_data_set_components.md#enhanced_nc4_hdf5
" %}
The Enhanced Data Model adds groups, a string type, several unsigned integer types, and user-defined types.
Groups, like directories in a file system, can be hierarchically organized to arbitrary depth.
Each netCDF file/dataset contains a top-level, unnamed group (aka root group).
Each group may contain one or more named variables, dimensions, attributes, groups, and user-defined types.

A variable is still a multidimensional array whose elements are all of the same type.
Each variable may have attributes, and each variable's shape is specified by its dimensions, which may be shared.
However, in the enhanced data model, one or more dimensions may be of unlimited length, so data may be efficiently appended to variables along any of those dimensions.
Variables and attributes have one of twelve primitive data types or one of four kinds of user-defined types.

Dimensions are scoped such that they can be seen in all descendant groups.
That is, dimensions can be shared between variables in different groups, if they are defined in a parent group.

In netCDF-4 files, the user may also define a type.
For example a compound type may hold information from an array of C structures,
or a variable length type allows the user to read and write arrays of variable length values.

User-defined types are ~~also~~ scoped such that they can be referenced in all other groups.
That means, for example, that variables in different groups can have the same type.

Variables, groups, and types share a namespace.
Within the same group, variables, groups, and types must have unique names.
(That is, a type and variable may not have the same name within the same group,
and similarly for sub-groups of that group.)

A UML diagram of the enhanced netCDF data model
shows (in red) what it adds to the classic netCDF data model:
{% include image.html file="nc-enhanced-uml.png" alt="netCDF Enhanced Data Model UML" caption="" %}

## NetCDF Objects
### Dimensions
{%include note.html content="
Text from NUG/netcdf_data_set_components.md#dimensions
" %}

A netCDF dimension has both a name and a length.
A list of dimensions is used to specify the shape of a netCDF variable.
When variables share dimensions, it indicates a common grid.

A dimension may be used to represent a real physical dimension, for example, time, latitude, longitude, or height. A dimension might also be used to index other quantities, for example station or model-run-number.

A dimension length is either an arbitrary positive integer or unlimited.
Dimensions with an unlimited length are called unlimited dimensions or record dimensions.
Unlimited dimensions indicate that data may be efficiently appended to variables along that dimension.
A variable with an unlimited dimension can grow to any length along that dimension. The unlimited dimension index is like a record number in conventional record-oriented files.

In the netCDF classic data model, a dataset can have at most one unlimited dimension, but need not have any. If a variable has an unlimited dimension, that dimension must be the most significant (slowest changing) one. ???Thus any unlimited dimension must be the first dimension in a CDL shape and the first dimension in corresponding C array declarations.???

In the netCDF enhanced data model, a dataset may have multiple unlimited dimensions, and there are no restrictions on their order in the list of a variables dimensions.

#### Dimension advice???
It is possible (since version 3.1 of netCDF) to use the same dimension more than once in specifying a variable shape. For example, correlation(instrument, instrument) could be a matrix giving correlations between measurements using different instruments. But data whose dimensions correspond to those of physical space/time should have a shape comprising different dimensions, even if some of these have the same length.

### Attributes
### Variables
Variables are used to store the bulk of the data in a netCDF dataset.
A variable is a multidimensional array whose elements are all of the same type.
A variable has a name, a data type, and a shape given by its list of dimensions. (All three aspects of a variable must be specified when the variable is created and cannot be changed after creation. <!-- ??? Is creation time part of the data model??? -->)
The number of dimensions is called the rank (a.k.a. dimensionality) of that variable.
A scalar variable has rank 0, a vector has rank 1 and a matrix has rank 2.
A variable may also have associated attributes, which may be added, deleted or changed after the variable is created. <!-- ??? Creation time here as well.???> 

#### Coordinate Variables
## NetCDF Data Types
### String data types in netCDF
#### `Char` array Strings
#### String data type in Enhanced Data Model
#### String character encodings (UTF-8)
### NetCDF Object Names {#object_name}

#### Character Encodings (UTF-8)
#### Permitted Characters in NetCDF Names {#permitted_characters}

The names of dimensions, variables and attributes (and, in netCDF-4 files, groups, user-defined types, compound member names, and enumeration symbols) consist of arbitrary sequences of alphanumeric characters, underscore '_', period '.', plus '+', hyphen '-', or at sign '@', but beginning with an alphanumeric character or underscore. However names commencing with underscore are reserved for system use.

Beginning with versions 3.6.3 and 4.0, names may also include UTF-8 encoded Unicode characters as well as other special characters, except for the character '/', which may not appear in a name.

Names that have trailing space characters are also not permitted.

Case is significant in netCDF names.

#### Name Length {#name_length}

A zero-length name is not allowed.

Names longer than ::NC_MAX_NAME will not be accepted any netCDF define function. An error of ::NC_EMAXNAME will be returned.

All netCDF inquiry functions will return names of maximum size ::NC_MAX_NAME for netCDF files. Since this does not include the terminating NULL, space should be reserved for NC_MAX_NAME + 1 characters.

#### NetCDF Conventions {#netcdf_conventions}

Some widely used conventions restrict names to only alphanumeric characters or underscores.

> Note that, when using the DAP2 protocol to access netCDF data, there are \em reserved keywords, the use of which may result in undefined behavior.  See \ref dap2_reserved_keywords for more information.

## NetCDF Enhanced Data Model

### Dimensions
multiple unlimited
VLEN
### Groups
### New Data Types

#### String
##### String character encodings (UTF-8)


## Supported String Encodings



## Data Types

### Atomic Data Types

|---
| CDL | netCDF-C | netCDF-Java | Description | Data Models supported
|:-|:-|:-|:-|:-
| char   | NC_CHAR   | CHAR   | Characters.                          | Classic
| byte   | NC_BYTE   | BYTE   | Eight-bit integers.                  | Classic 
| short  | NC_SHORT  | SHORT  | 16-bit signed integers.              | Classic
| int    | NC_INT    | INT    | 32-bit signed integers.              | Classic
| long   | NC_LONG   | ---    | (Deprecated, synonymous with int)    | ---
| float  | NC_FLOAT  | FLOAT  | IEEE single-precision floating point (32 bits). | Classic
| real   | ---       | ---    |  (?Deprecated? Synonymous with float). | ---
| double | NC_DOUBLE | DOUBLE | IEEE double-precision floating point (64 bits). |Classic
| ubyte  | NC_UBYTE  | UBYTE  | Unsigned eight-bit integers.         | Enhanced
| ushort | NC_USHORT | USHORT | Unsigned 16-bit integers.            | Enhanced
| uint   | NC_UINT   | UINT   | Unsigned 32-bit integers.            | Enhanced
| int64  | NC_INT64  | LONG   | 64-bit signed integers.              | Enhanced
| uint64 | NC_UINT64 | ULONG  | Unsigned 64-bit signed integers.     | Enhanced
| string | NC_STRING | STRING | Variable-length string of characters | Enhanced

### User Defined Data Types

|---
| DataType | Array subclass | Array.getElementType
|:-|:-|:-
| BYTE | ArrayByte | byte.class
| SHORT | ArrayShort | short.class
| INT | ArrayInt | int.class
| LONG | ArrayLong | long.class
| FLOAT | ArrayFloat | float.class
| DOUBLE | ArrayDouble | double.class
| CHAR | ArrayChar | char.class
| STRING | ArrayObject | String.class
| STRUCTURE | ArrayStructure | StructureData.class
| SEQUENCE | ArraySequence | StructureData.class
| ENUM1 | ArrayByte | byte.class
| ENUM2 | ArrayShort | short.class
| ENUM4 | ArrayInt | int.class
| OPAQUE | ArrayObject | ByteBuffer.class


|---
| Data Type | CDL | netCDF-C | netCDF-Java | Description
|:-|:-|:-|:-|:-
| char   | char   | NC_CHAR   | CHAR | Characters.
| byte   | byte   | NC_BYTE   | BYTE | Eight-bit integers.
| short  | short  | NC_SHORT  | SHORT | 16-bit signed integers.
| int    | int    | NC_INT    | INT  | 32-bit signed integers.
|        | long   | NC_LONG   |  | (Deprecated, synonymous with int)
| float  | float  | NC_FLOAT  | FLOAT | IEEE single-precision floating point (32 bits).
|        | real   |           | |  (?Deprecated? Synonymous with float).
| double | double | NC_DOUBLE | DOUBLE | IEEE double-precision floating point (64 bits).
| ubyte  | ubyte  | NC_UBYTE  | UBYTE | Unsigned eight-bit integers.
| ushort | ushort | NC_USHORT | | Unsigned 16-bit integers.
| uint   | uint   | NC_UINT   | | Unsigned 32-bit integers.
| int64  | int64  | NC_INT64  | LONG | 64-bit signed integers.
| uint64 | uint64 | NC_UINT64 | | Unsigned 64-bit signed integers.
| string | string | NC_STRING | | Variable-length string of characters

<!--
See netCDF-Java ArrayType ilines 19-46
https://github.com/Unidata/netcdf-java/blob/01d8aef292cc7bbcee556657129bc88694613d65/cdm/core/src/main/java/ucar/array/ArrayType.java#L19-L46
-->
### Atomic Data Types
<!-- Both "atomic" and "primitive" are used to describe data types in the current NUG. -->
### NetCDF Object Names

#### Encoded as UTF-8

#### Characters allowed

