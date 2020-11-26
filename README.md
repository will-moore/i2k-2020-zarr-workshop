# i2k-2020-zarr-workshop



## Set-up

To run the Jupyter Notebooks and napari:

Install Anaconda https://www.anaconda.com/products/individual#Downloads

Then, to create the environment:

    $ cd i2k-2020-zarr-workshop
    $ conda env create -f environment.yml
    $ conda activate I2K_zarr_workshop
    $ pip install ome_zarr

Run a Notebook:

    $ jupyter notebook ome-zarr-intro.ipynb


Open OME-zarr [sample data](https://blog.openmicroscopy.org/file-formats/community/2020/11/04/zarr-data/) in napari:

    $ napari https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/6001247.zarr
