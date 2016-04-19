# Running DO3SE models

Follow the build for Fortran to build the scripts without Python.  To run you need a config file, data input and sed statement e.g.

    run_do3se path/to/config.nml < path/to/data.csv | sed -e 's/^ *//' -e 's/ *$//' -e 's/ \+/,/g' >> output.csv
    
    
