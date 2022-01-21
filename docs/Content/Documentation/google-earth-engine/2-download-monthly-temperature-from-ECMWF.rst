=======================================================================
Download monthly average temperature from ECMWF Climate dataset
=======================================================================
*Written by Men Vuthy, 2022*

----------

Objective
---------------

* Vizualize digital elevation model (DEM) of SRTM90 version 4 from Google Earth Engine.
* Download DEM within region of interest (Cambodia).

Dataset
---------------

`ERA5-Land <https://developers.google.com/earth-engine/datasets/catalog/ECMWF_ERA5_LAND_MONTHLY>`__ is a reanalysis dataset providing a consistent view of the evolution of land variables over several decades at an enhanced resolution compared to ERA5. ERA5-Land has been produced by replaying the land component of the ECMWF ERA5 climate reanalysis. Reanalysis combines model data with observations from across the world into a globally complete and consistent dataset using the laws of physics. Reanalysis produces data that goes several decades back in time, providing an accurate description of the climate of the past. This dataset includes all 50 variables as available on CDS.

The data presented here is a subset of the full ERA5-Land dataset post-processed by ECMWF. Monthly-mean averages have been pre-calculated to facilitate many applications requiring easy and fast access to the data, when sub-monthly fields are not required.

ERA5-Land data is available from 1981 to three months from real-time. More information can be found at the `Copernicus Climate Data Store <https://cds.climate.copernicus.eu>`__.

.. figure:: img/ERA5-Climate.png
    :width: 600px
    :align: center

Code
---------------

**1. Visualize dataset**

ECMWF Climate Reanalysis dataset can be visualized using the code snippet below:

.. code-block:: JavaScript
    
    // Import dataset
    var dataset = ee.ImageCollection('ECMWF/ERA5_LAND/MONTHLY');

    // Select temperature band
    var temperature = dataset.select('temperature_2m');

    // Set palette for data range visualization
    var temperatureVis = {
        min: 250.0,
        max: 320.0,
        palette: [
            "#000080","#0000D9","#4000FF","#8000FF","#0080FF","#00FFFF",
            "#00FF80","#80FF00","#DAFF00","#FFFF00","#FFF500","#FFDA00",
            "#FFB000","#FFA400","#FF4F00","#FF2500","#FF0A00","#FF00FF",
        ]
    };

.. figure:: img/world-temp-image.png
    :width: 1200px
    :align: center

**2. Download DEM to Google Drive**