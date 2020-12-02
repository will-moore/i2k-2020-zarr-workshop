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

## NGFF spec

The Next Generation File Format Spec is at https://ngff.openmicroscopy.org/latest/. The current prototype is OME-Zarr, and the following notebook
has some examples of working with this data:

    # In the environment created above:
    $ jupyter notebook ome-zarr-intro.ipynb


## JavaScript

**zarr-lite**

Loading zarr data with zarr-lite (from Trevor Manz's [example](https://observablehq.com/@manzt/using-zarr-lite))

 - Simplest example https://jsfiddle.net/will_j_moore/tv95ybh8/1/

 - Using 'omero' channel min/max rendering settings https://jsfiddle.net/will_j_moore/vxa92bnf/77/


**vizarr**

Use https://github.com/hms-dbmi/vizarr (Trevor Manz) to browse
[OME-Zarr 5d images](https://blog.openmicroscopy.org/file-formats/community/2020/11/04/zarr-data/) from IDR.

E.g. https://hms-dbmi.github.io/vizarr?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/9822152.zarr 

Pro Tip: Open the browser developer tools and choose Network tab to see
what zarr chunks and metadata are being loaded.


**HCS Data**

Sample [OME-Zarr HCS Plates](https://blog.openmicroscopy.org/file-formats/community/2020/12/01/zarr-hcs/)

E.g. https://hms-dbmi.github.io/vizarr?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/422.zarr

Click on the images to browse Plate -> Well -> Image

## Python (napari)

Open in `napari` using a `napari` plugin: [ome-zarr](https://github.com/ome/ome-zarr-py) as installed above. Can open any [OME-zarr 5d images](https://blog.openmicroscopy.org/file-formats/community/2020/11/04/zarr-data/) from IDR.

    # e.g. 3D image with labels:
    $ napari https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/9822152.zarr

Also supports opening Plates and Wells:

    # A Well with 9 fields
    $ napari https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/5966.zarr/A/1

    # Plate with labels
    $ napari https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/2551.zarr

    # Image with labels
    $ napari https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/plates/2551.zarr/A/1/0

The labels metadata on this plate includes properties that were added from a
separate csv file using `ome-zarr` in a workflow describe on
[PR #63](https://github.com/ome/ome-zarr-py/pull/63).

We can make use of the labels properties in napari, e.g. to update color
based on property values. Open the console in napari:

    import numpy as np
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

