## Final map to the antarctic vegetation


### GEE code for antarctic vegetation map:

var shetlands = 
    /* color: #d63000 */
    /* shown: false */
    ee.Geometry.Polygon(
        [[[-63.27106581673961, -62.943471600212455],
          [-63.05564147576564, -63.37962234407149],
          [-62.30426894173961, -63.45860063659696],
          [-60.21686659798961, -62.993402389757804],
          [-59.71149550423961, -62.66227912367752],
          [-53.57013808236461, -61.27365457092311],
          [-53.81073734585601, -61.00734873753302],
          [-54.45334814901915, -60.88311254167704],
          [-55.75988837874111, -60.86833046645117],
          [-57.18464003548961, -61.573193877849036],
          [-58.30524550423961, -61.843958002725444],
          [-59.11823378548961, -62.071214515543005],
          [-60.04108534798961, -62.25078109597661],
          [-61.16169081673961, -62.45977845138799]]]),
    peninsula = 
    /* color: #ff9999 */
    /* shown: false */
    ee.Geometry.Polygon(
        [[[-59.95761783573391, -63.47027086385727],
          [-60.00156314823391, -63.72430838809856],
          [-59.90268619510891, -63.850478274051596],
          [-58.10092838260891, -64.54856148515674],
          [-57.39780338260891, -64.62870606429087],
          [-56.87045963260891, -64.58159077408938],
          [-56.49692447635891, -64.34954328808792],
          [-56.49692447635891, -64.12031784565966],
          [-57.04624088260891, -63.966442709295826],
          [-56.71665103885891, -63.68050259008884],
          [-55.61801822635891, -63.621989359411735],
          [-55.22251041385891, -63.46536326642057],
          [-54.60727603885891, -63.53399306066353],
          [-54.32163150760891, -63.32760830259313],
          [-54.73911197635891, -63.27330915962219],
          [-54.58530338260891, -63.169362717394144],
          [-55.49716861698391, -62.88032566483948],
          [-56.84848697635891, -62.85026153286475],
          [-57.13413150760891, -63.074993547686454],
          [-58.25473697635891, -63.278249674148896],
          [-58.91391666385891, -63.18423509431014]]]);


// Eliana Lima da Fonseca
// Laboratório de Geotecnologias Aplicadas
// UFRGS - Universidade Federal do Rio Grande do Sul
// Porto Alegre city, Brazil
// eliana.fonseca@ufrgs.br

//legend
var title = ui.Label('North Antarctic Peninsula and South Shetlands Vegetation Map');
title.style().set('position', 'top-center');
Map.add(title);

//inicial zoom
Map.centerObject(ee.Geometry.Point(-59.5,-63), 7);
Map.setOptions('satellite');

// WATER MASK
var collection1 = ee.ImageCollection('COPERNICUS/S2');
// 1. minimun value composite
var B9min1 = collection1.select('B9').reduce(ee.Reducer.min());
// 2. remap
var B9min_class1 = ee.Image(1)
          .where(B9min1.gt(0).and(B9min1.lte(120)), 2)
          .where(B9min1.gt(120).and(B9min1.lte(10000)), 3)
// 3.select class to built the mask
var mask1 = B9min_class1.eq(3)


//NDVI 
var SY = 2016 //start year
var EY = 2021 //end year
var collection = ee.ImageCollection('COPERNICUS/S2')
.filter(ee.Filter.calendarRange(SY,EY,'year'))
.filter(ee.Filter.calendarRange(1,4,'month'))

function addNDVI(image) {
var ndvi_calc = image.expression
("(nir - red)/(nir+red)", 
{ nir : image.select("B8"),
  red : image.select("B4")}).rename("NDVI")
return image.addBands (ndvi_calc)
}

var ndviCollection_all = collection.map(addNDVI) 

//maximum value composite
var ndvi_mosaico_all = ndviCollection_all.qualityMosaic('NDVI')
var ndvi_all = ndvi_mosaico_all.select(['NDVI'])

//remap
var ndvi_all_class = ee.Image(1)
          .where(ndvi_all.gt(-1).and(ndvi_all.lte(0).and(mask1.eq(1))), 2)
          .where(ndvi_all.gt(0).and(ndvi_all.lte(0.05).and(mask1.eq(1))), 4)
          .where(ndvi_all.gt(0.05).and(ndvi_all.lte(0.1).and(mask1.eq(1))), 5)
          .where(ndvi_all.gt(0.1).and(ndvi_all.lte(0.15).and(mask1.eq(1))), 6)
          .where(ndvi_all.gt(0.15).and(ndvi_all.lte(0.2).and(mask1.eq(1))), 7)
          .where(ndvi_all.gt(0.2).and(ndvi_all.lte(0.25).and(mask1.eq(1))), 8)
          .where(ndvi_all.gt(0.25).and(ndvi_all.lte(0.3).and(mask1.eq(1))), 9)
          .where(ndvi_all.gt(0.3).and(ndvi_all.lte(0.35).and(mask1.eq(1))), 10)
          .where(ndvi_all.gt(0.35).and(ndvi_all.lte(0.4).and(mask1.eq(1))), 11)
          .where(ndvi_all.gt(0.4).and(ndvi_all.lte(0.45).and(mask1.eq(1))), 12)
          .where(ndvi_all.gt(0.45).and(ndvi_all.lte(0.5).and(mask1.eq(1))), 13)
          .where(ndvi_all.gt(0.5).and(ndvi_all.lte(0.55).and(mask1.eq(1))), 14)
          .where(ndvi_all.gt(0.55).and(ndvi_all.lte(0.6).and(mask1.eq(1))), 15)
          .where(ndvi_all.gt(0.6).and(ndvi_all.lte(0.65).and(mask1.eq(1))), 16)
          .where(ndvi_all.gt(0.65).and(ndvi_all.lte(0.7).and(mask1.eq(1))), 17)
          .where(ndvi_all.gt(0.7).and(ndvi_all.lte(0.75).and(mask1.eq(1))), 18)
          .where(ndvi_all.gt(0.75).and(ndvi_all.lte(0.8).and(mask1.eq(1))), 19)
          .where(ndvi_all.gt(0.8).and(ndvi_all.lte(0.85).and(mask1.eq(1))), 20)
          .where(ndvi_all.gt(0.85).and(ndvi_all.lte(0.9).and(mask1.eq(1))), 21)
          .where(ndvi_all.gt(0.9).and(ndvi_all.lte(0.95).and(mask1.eq(1))), 22)
          .where(ndvi_all.gt(0.95).and(ndvi_all.lte(1).and(mask1.eq(1))), 23)

//print the maps
var ndvi_south_shetlands = ndvi_all_class.clip(shetlands)
var ndvi_peninsula = ndvi_all_class.clip(peninsula)

var palette =['3182bd', '3182bd', 'd9d9d9', 'd9d9d9','d9d9d9', '00441b', '238b45','238b45','238b45','238b45','238b45','238b45','238b45','66c2a4','66c2a4', '66c2a4','66c2a4','66c2a4','66c2a4','66c2a4','66c2a4','66c2a4']; 

Map.addLayer(ndvi_south_shetlands, {min:2, max:23, palette:palette}, 'ndvi_south_shetlands')
Map.addLayer(ndvi_peninsula, {min:2, max:23, palette:palette}, 'ndvi_peninsula')


//legend the maps
var legend = ui.Panel({style: {position:'top-left', padding: '8px 15px'}});
var legendTitle = ui.Label({value: 'Legend', style: { fontSize: '13px', margin: '0 0 4px 0', padding: '0' }});
legend.add(legendTitle);
var makeRow = function(color, name) {var colorBox = ui.Label({style: {backgroundColor: '#' + color, padding: '8px', margin: '0 0 4px 0'}});
var description = ui.Label({ value: name, style: {margin: '0 0 4px 6px'}});
return ui.Panel({ widgets: [colorBox, description],layout: ui.Panel.Layout.Flow('horizontal')});};
var names = ['Algaes', 'Lichens', 'Mosses', 'masked pixels','masked pixels'];
var palettel  = ['00441b', '238b45','66c2a4', '3182bd', 'd9d9d9'];
for (var i = 0; i < 3; i++) { legend.add(makeRow(palettel[i], names[i]))}  
Map.add(legend);

### Link directly for the GEE:
https://code.earthengine.google.com/?accept_repo=users/eliana_ufrgs/vegetation_map


### Subsets shapefiles:

https://doi.org/10.5281/zenodo.5636633

