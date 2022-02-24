================================================
Download RGB Landsat 8 image for Cambodia
================================================
*Written by MEAN Sovanna, 2022*

----------

Objective
---------------

* Access image collection from LANDSAT8 in Google Earth Engine.
* Masking cloud (to replace clouding image by clear image)
* Clip Cambodia image collection from LANDSAT8

Dataset
---------------



Code
---------------

**1. Access image collection**

The image colletion from LANDSAT8SRTM can be accessed using the code snippet below:

.. code-block:: JavaScript

    //import image collection from LANDSAT8
    var dataset = ee.ImageCollection('LANDSAT/LC08/C02/T1_L2')
    .filterDate('2021-01-01', '2021-12-31');

.. .. figure:: img/STRM90_dataset.png
..     :width: 1200px
..     :align: center

**2. Masking Cloud**

.. code-block:: JavaScript

  function maskL8sr(image) {
  // Bits 3 and 5 are cloud shadow and cloud, respectively.
  var cloudShadowBitMask = (1 << 3);
  var cloudsBitMask = (1 << 5);
  // Get the pixel QA band.
  var qa = image.select('QA_PIXEL');
  // Both flags should be set to zero, indicating clear conditions.
  var mask = qa.bitwiseAnd(cloudShadowBitMask).eq(0)
                 .and(qa.bitwiseAnd(cloudsBitMask).eq(0));
  return image.updateMask(mask)}

**3. Clip Cambodia image collection from LANDSAT8**

.. code-block:: JavaScript

    // Applies scaling factors.
    function applyScaleFactors(image) {
    var opticalBands = image.select('SR_B.').multiply(0.0000275).add(-0.2);
    var thermalBands = image.select('ST_B.*').multiply(0.00341802).add(149.0);
    return image.addBands(opticalBands, null, true)
              .addBands(thermalBands, null, true);
    }

    dataset = dataset.map(applyScaleFactors)
          .map(maskL8sr)
          .median();

    // Load country features from Large Scale International Boundary (LSIB) dataset.
    var countries = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017');
    var roi = countries.filter(ee.Filter.eq('country_co', 'CB'));
    Map.addLayer(roi,{},'Cambodia')

    var ls_img = ee.Image(dataset).clip(roi)

    var visualization = {
    bands: ['SR_B4', 'SR_B3', 'SR_B2'],
    min: 0.0,
    max: 0.3,
    };

    Map.addLayer(ls_img, visualization, 'True Color (432)');


Finally, we can see how to clip Cambodia image collection from LANDSAT8 via Google Earth Engine.

----------

**Reference**

* https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC08_C02_T1_L2
