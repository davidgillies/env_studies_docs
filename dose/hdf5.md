# HDF5

Good for data from  multiple sources and types.  You can store metadata about your data along with it.  Supports partial I/O so can read parts of files.  It also works with Matlab.  

## Data Model
Data Objects (numpy ndarrays), metadata (attributes, key-value attached to dataset) and structure (relationships).  Groups are containers like folders that hold datasets or other groups.

- Datasets are numpy arrays which have well-defined shape and size and can be sliced, e.g. support partial access.  Datasets can be multiterabytes. 
- Attributes stored for dataset.
- Groups and Attributes are accessed like dictionaries.

You can define compound data types on hdf5.  Datasets are resizable.  Compressed datasets.  Optimized tile access.

A convention is a standardized arrangement of groups, datasets and attributes used to solve a domain specific problem.

