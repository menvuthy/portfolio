=====================================
Download DEM from SRTM90 dataset
=====================================
*Written by Men Vuthy, 2022*

----------

Objective
---------------

* Vizualize digital elevation model (DEM) of SRTM90 version 4 from Google Earth Engine.
* Download DEM within region of interest (Cambodia).

Dataset
---------------

The `Shuttle Radar Topography Mission (SRTM) <https://developers.google.com/earth-engine/datasets/catalog/CGIAR_SRTM90_V4#description>`__ digital elevation dataset was originally produced to provide consistent, high-quality elevation data at near global scope. This version of the SRTM digital elevation data has been processed to fill data voids, and to facilitate its ease of use.

.. figure:: img/CGIAR-SRTM98_V4.png
    :width: 1200px
    :align: left

Code
---------------

.. code-block:: JavaScript

    // Import DEM dataset
    var dataset = ee.Image('CGIAR/SRTM90_V4');
    var elevation = dataset.select('elevation');
    var slope = ee.Terrain.slope(elevation);
    Map.setCenter(105.237, 12.164, 7);
    Map.addLayer(slope, {min: 0, max: 60}, 'slope');



// Import feature of region of interest (Cambodia)
// Load country features from Large Scale International Boundary (LSIB) dataset.
var countries = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017');
var roi = countries.filter(ee.Filter.eq('country_co', 'CB'));

// Add ROI layer to interactive map
Map.addLayer(roi, {color:'green'}, 'basin');



// Clip DEM image to the target ROI
var elevation = dataset.select('elevation').clip(roi);


Map.addLayer(elevation,  {min: 0, max: 50}, 'elevation');



// Export.image.toDrive({
//   image: elevation,
//   description: 'pursat_RB',
//   scale: 90,
//   region: rb_pursat
// });


