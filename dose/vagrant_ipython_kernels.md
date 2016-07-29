# Vagrant with multiple ipython kernels

Install the kernels for ipython (assuming some version installed).

    python2 -m pip install ipykernel
    python2 -m ipykernel install --user
    
This command will show you where the kernel has been installed.  

    jupyter kernelspec list
    
This will show which kernels jupyter knows about.  Sometimes it seems to pick them up ok, if not, just move the newly installed kernel into the location shown for the kernelspec list.

You can do this the other way around for python3 if you need to.

It should show up now in the ipython notebook 'new' button dropdown and also in the 'kernel' button of individual notebooks.
