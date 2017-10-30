
---
title: 'Cloudy Bing Aerials'
category: 'Data'
tags: 
    - Bing Maps
    - ArcGIS API for JavaScript
---

I have a client using Bing Maps for its base imagery within a custom ArcGIS API for Javascript application. Think [VETiledLayer](https://developers.arcgis.com/javascript/3/jsapi/vetiledlayer-amd.html) with a paid Bing Maps license. All was well with Bing Aerials over the last several years up until a few months ago. That's when the clouds rolled in, [literally](https://binged.it/2lnLxlq).

TODO: iFrame the map

So what does one do? Open a Microsoft case of course. After several back-and-forth exchanges, a surprising hack was provided by MSFT. Below is the key part of the [gist](https://gist.github.com/sbuscher/858ebb740d93423d9eada0c465c24277): 

TODO: maybe add a gist instead...

```javascript
function init() {
    esrimap = new esri.Map("map");

    //Creates the Virtual Earth layer to add to the map
    veTileLayer = new esri.virtualearth.VETiledLayer({
        bingMapsKey: prompt("Please enter your bing maps key"),
        mapStyle: esri.virtualearth.VETiledLayer.MAP_STYLE_AERIAL_WITH_LABELS
    });

    /* Work Around - Start */

    //Cache a reference to the layer initializer function. 
    var temp = veTileLayer._initLayer;

    //Override the initializer function. 
    veTileLayer._initLayer = function (a, b) {

        //Override the map tile URL generation ID.
        a.resourceSets[0].resources[0].imageUrl = a.resourceSets[0].resources[0].imageUrl.replace(/g=[0-9]+/, 'g=5772'); 

        //Call the original initializer.
        temp(a, b);
    };

    //Force the layer to refresh.
    veTileLayer._getTileInfo();

    /* Work Around - End */
}
``` 

To summarize, `veTileLayer._initLayer` is overriden in order to specify a version other than default to be used. Note that the API code is obfuscated, hence the `a' and 'b' references. When debugging, the initial value of imageUrl is: 

`"http://ecn.{subdomain}.tiles.virtualearth.net/tiles/h{quadkey}.jpeg?g=5934&mkt={culture}"`  

Therefore, the _initLayer override function replaces the current version 5934 with previous version 5772. Gotta love how that `temp` reference to `_initLayer` is used within the override to call itself. 

The moral of the story? There is a possibility of using previous versions of tiled images. In this case an older version was better quality than the new...in certain areas. The caveat is other areas now might be out of date. At the end of the day the Bing Maps team has to contact their vendor, likely [Digital Globe](https://www.digitalglobe.com/), to update the image cache for the cloudy areas identified. Not holding my breath for that fix to happen anytime soon.  

:wq
