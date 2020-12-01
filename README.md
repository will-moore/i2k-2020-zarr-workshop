# i2k-2020-zarr-workshop

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/will-moore/i2k-2020-zarr-workshop/HEAD)


## Set-up

To run the Jupyter Notebooks and napari:

Install Anaconda https://www.anaconda.com/products/individual#Downloads

Then, to create the environment:

    $ cd i2k-2020-zarr-workshop
    $ conda env create -f environment.yml -n i2k
    $ conda activate i2k

This is sufficient to run a Notebook:

    $ jupyter notebook ome-zarr-intro.ipynb

To run `napari` we need to add:

    $ pip install napari[all]
    $ pip install ome_zarr

    $ napari ... (see below)


# Demo

## JavaScript

Simple example with zarr-lite (Trevor Manz)
https://jsfiddle.net/will_j_moore/tvjsxgw4/9/

https://jsfiddle.net/will_j_moore/vxa92bnf/77/


Use https://github.com/hms-dbmi/vizarr (Trevor Manz) to browse
[OME-zarr 5d images](https://blog.openmicroscopy.org/file-formats/community/2020/11/04/zarr-data/) from IDR.

E.g. https://hms-dbmi.github.io/vizarr?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/9822152.zarr 


HCS Data:

 - idr0002: 96 Wells, multi-T https://hms-dbmi.github.io/vizarr?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/422.zarr
 - idr0033: 384 Wells, 9 fields https://hms-dbmi.github.io/vizarr?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/5966.zarr
 - idr0004: 68 Wells, multi-Z https://hms-dbmi.github.io/vizarr?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/1751.zarr
 - idr0094 https://hms-dbmi.github.io/vizarr?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/7825.zarr
 - idr0001: 96 Wells, 6 fields https://hms-dbmi.github.io/vizarr?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/2551.zarr

 - Can view chunks and `.zattrs` being loaded via dev-tools Network
 - Can browse Plate -> Well -> Image

## napari

Open in napari using a napari plugin: [ome-zarr](https://github.com/ome/ome-zarr-py). Install with `pip install ome-zarr`. Can open any [OME-zarr 5d images](https://blog.openmicroscopy.org/file-formats/community/2020/11/04/zarr-data/) from IDR.

    # e.g. 3D image with labels:
    $ napari https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/6001247.zarr

Also supports opening a Plate:

    $ napari https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/422.zarr

    # with labels
    $ napari https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/2551.zarr

We can make use of the labels properties in napari, e.g. to update color
based on property values. Open the console in napari:

    layer = viewer.layers[-1]

    for c, value in enumerate(layer.properties["area (µm)"]):
        index = layer.properties["index"][c]
        if value > 60:
            layer.color[index] = np.array([1, 0, 0, 1], dtype=np.float32)
        elif value < 30:
            layer.color[index] = np.array([0, 1, 0, 1], dtype=np.float32)
        else:
            layer.color[index] = np.array([1, 1, 1, 1], dtype=np.float32)
    layer.color_mode = "direct"

