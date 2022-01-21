=====================================
Download DEM from SRTM90 dataset
=====================================
*Written by Men Vuthy, 2020*

----------

Objective
---------------

* Vizualize digital elevation model (DEM) of SRTM90 version 4 from Google Earth Engine.
* Download DEM within region of interest (Cambodia).


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


