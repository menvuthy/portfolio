Air Temperature
==========================


.. code-block:: JavaScript
    
    var dataset = ee.ImageCollection('ECMWF/ERA5_LAND/MONTHLY');
    var temperature = dataset.select('temperature_2m');
    var temperatureVis = {
    min: 250.0,
    max: 320.0,
    palette: [
        "#000080","#0000D9","#4000FF","#8000FF","#0080FF","#00FFFF",
        "#00FF80","#80FF00","#DAFF00","#FFFF00","#FFF500","#FFDA00",
        "#FFB000","#FFA400","#FF4F00","#FF2500","#FF0A00","#FF00FF",
    ]
    };

    Map.addLayer(temperature, temperatureVis, 'Temperature');


    // set start and end year
    var startyear = 2020;
    var endyear = 2020;
    
    // make a date object
    var startdate = ee.Date.fromYMD(startyear, 1, 1);
    var enddate = ee.Date.fromYMD(endyear + 1, 1, 1);
    
    // make a list with years
    var years = ee.List.sequence(startyear, endyear);
    // make a list with months
    var months = ee.List.sequence(1, 12);

    // Calculate monthly precipitation
    var monthlyTemp =  ee.ImageCollection.fromImages(
    years.map(function (y) {
        return months.map(function(m) {
        var w = dataset.filter(ee.Filter.calendarRange(y, y, 'year'))
                        .filter(ee.Filter.calendarRange(m, m, 'month'))
                        .sum();
        return w.set('year', y)
                .set('month', m)
                .set('system:time_start', ee.Date.fromYMD(y, m, 1));
        });
    }).flatten()
    );

    // Calculate mean monthly precipitation
    var meanMonthlyT =  ee.ImageCollection.fromImages(
    months.map(function (m) {
        var w = monthlyTemp.filter(ee.Filter.eq('month', m)).mean();
        return w.set('month', m)
                .set('system:time_start',ee.Date.fromYMD(1, m, 1));
    }).flatten()
    );


.. code-block:: python

    import numpy as np
    import geopandas as gpd
    import rasterio

    x = np.array([1,2,3])
    # print
    print(x)


.. code-block:: python

    import xarray as xr
    