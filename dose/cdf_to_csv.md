#netCDF to CSV Conversion

Install netCDF4 for python.  I have canopy installed so used its package manager to install it.  Install zarray using the package manager too.

    import xarray
    db = xarray.open_dataset('/Users/david/Downloads/ModisLandUse_d01-2.nc') # .nc is the netCDF extension.
    df2 = db.to_dataframe() # convert to pandas dataframe.
    df2.to_csv('/Users/david/ModisLandUse_d01.csv')
    
That's it!
