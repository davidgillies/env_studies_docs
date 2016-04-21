# HDF5

Good for data from  multiple sources and types.  You can store metadata about your data along with it.  Supports partial I/O so can read parts of files.  It also works with Matlab.  

## Data Model
Data Objects (numpy ndarrays), metadata (attributes, key-value attached to dataset) and structure (relationships).  Groups are containers like folders that hold datasets or other groups.

- Datasets are numpy arrays which have well-defined shape and size and can be sliced, e.g. support partial access.  Datasets can be multiterabytes. 
- Attributes stored for dataset.
- Groups and Attributes are accessed like dictionaries.

You can define compound data types on hdf5.  Datasets are resizable.  Compressed datasets.  Optimized tile access.

A convention is a standardized arrangement of groups, datasets and attributes used to solve a domain specific problem.

## Datasets
They 'rectangular' sets of homogenous data which can be multi-dimensional.  dataspace describes the shape and datatype the type of variables that are in it.  

    import h5py
    f = h5py.File("file.hdf5")
    dset = f.create_dataset("mydataset", (10,10), dtype='f') # name, shape, datatype.
    # shape is a 2-D structure 10 x 10.
    # datatype f is a single point float, e.g. 10.0
    dset[0,0] = 2.0
    f.close()
    f = h5py.File("file.hdf5")
    dset = f['mydataset']
    dset.shape
    dset.dtype
    dset[0,0]
    
HDFView - https://www.hdfgroup.org/products/java/release/download.html

## Attributes
Keys are strings.  Values can be any HDF5 object so you can have arrays, etc.  Attributes don't support partial I/O like a dataset.  

        dset.attrs #
        dset.attrs['new'] = 5 # set attribute.
        dset.attrs.keys()
        dset.attrs.values() 
        dset.attrs.items() # returns a list of tuples
        del dset.attrs['new']
        dset[:] # shows the dataset
        
## Groups and Links
Objects can be shared between groups without copying.  You can have links to the same data within different groups.

        grp = f.create_group('mygroup')
        subgrp = grp.create_group('mysubgroup')
        grp.items() 
        subgrp.name # shows its full path.
        dset2 = f.create_dataset("a/b/c", (100,)) # creates all the groups that don't exist plus dataset.
        f['a'] # access groups by key.
        for i in f:
            print i
        
## File Structure
You can find stuff by using the key-values that all the things have.  

To recursively work with everything in the tree..

        def func(name):
            print name 
            
        f.visit(func) # visits all items and using that function.
        
## Partial I/O
You can select out any bits of the file, scattered round, not just blocks.  HDF5 only loads data when it's used.  This means it can be slow if you specify it to use one item at a time.  You can specify slices instead of one item at a time in a loop.  You can just load the full thing with [:].

Example:

        example_data = np.arange(100).reshape(10,10) # 10 x 10 array
        dset = f.create_dataset("numbers", data=example_data)
        dset[0, 0:5] # first 5 elements of first row.
        dset[0, ::2] # every second element of first row.
        dest[:,0] # first column
        for idx in xrange(10):
            print dset[idx,:] # load rows 1 at a time.
    
## Compound Data Types
Construct a data type:

        dt = np.dtype([('Name', 'S20'), ('Age', 'i'), ('Weight', 'f')])
        a = np.zeros((10,), dtype=dt)
        a[0]=('Bert', 40, 180.0) 
        a.dtype #
        a.dtype.names # shows field names
        a[['Name', 'Weight']] # returns just the given columns.
        dset10=f.create_dataset('table', (10,), dtype=dt) # create a HDF5 dataset from it.
        data = dset10[...] # read data into numpy array.
        dset10['Name', 'Weight'] # get columns
        
        # you can mix field names and slicing
        dset10['Name', 0:2] # get first 2 names.
        dset10['Name', 'Weight', 0:2] # get name and weight for first two.
        
## Dataset Storage Layouts
There are different possible layouts for contiguous (default), chunked, compact, external.  Contiguous is normal i.e. things are accessed following rows so accessing in rows is fast, by columns is slow.  Chunked stores thing differently so you can specficy a chunk size and it will follow through the chunk.  The stuff would be stored in a different order to work better with being accessed via this method.  Chunks can be independently compressed.  But if we want to read elements from 2 chunks then both chunks will be read in.  Chunks should be between 10kb and 500kb...  

        chunky = f.create_dataset('chunkyset', (100, 640, 480), chunks=(1, 320, 240)) # here we will store 100 images.
        stretchy = f.create_dataset('stretchy', (1000,), maxshape=(5000,) ) # sets  max possible size.
        stretchy.resize(3000,) # can resize the dataset.  You can do this with multiple dimensions.
        # if you specify the maxsize to be None, you can expand it to any size.
        
        
NB: Enthought course has more stuff on compression and filters.

# Command Line Tools

        h5ls file.hdf5 # 
        h5ls -r file.hdf5 # recursive list
        h5ls file.hdf5/mygroup # list group
        h5ls -v file.hdf5/mygroup # verbose info about the group
        h5dump file.hdf5 # dump the lot to screen
        h5dump -H file.hdf5 # dump headers to screen
        h5dump -H -d /mydataset file.hdf5 # dump headers about dataset.
        h5dump -d /mydataset file.hdf5 # with data.
        h5repack -f GZIP=9 file.hdf5 out.hdf5 # compress the file.
        
There are other commands, see the HDF5 group website.  https://www.hdfgroup.org/
