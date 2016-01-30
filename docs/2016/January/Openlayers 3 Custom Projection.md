# Openlayers 3 (OL3) Custom projections using proj4

Custom projections are a little tricky in web applications. In openlayers 3 there are several steps to take to completely support a custom projection in mapping and transformations.

```javascript

//add custom projections to proj4
//This can be found on http://spatialreference.org/
proj4.defs('EPSG:26986', '+proj=lcc +lat_1=42.68333333333333 +lat_2=41.71666666666667 +lat_0=41 +lon_0=-71.5 +x_0=200000 +y_0=750000 +ellps=GRS80 +datum=NAD83 +units=m +no_defs');

//add an alias to quickly reference
proj4.defs('urn:ogc:def:crs:EPSG::26986', proj4.defs['EPSG:26986']);
var mass_proj = new ol.proj.Projection({
  code: 'urn:ogc:def:crs:EPSG::26986',
  extent: [31789.1658, 790194.4183, 337250.8970, 961865.1338],
  units: 'm'
});

//add the projection to ol
ol.proj.addProjection(mass_proj);

//add projection tranformation functions necessary
//to transform geojson data
ol.proj.addCoordinateTransforms(
  mass_proj, 'EPSG:900913',
  function forward(coords) {
    return proj4(
      proj4.defs['EPSG:26986'], proj4.defs['EPSG:900913'],
      coords);
  },
  function inverse(coords) {
    return proj4(
      proj4.defs['EPSG:900913'], proj4.defs['EPSG:26986'],
      coords);
  });
  
  ```

As noted above, the proj4 definitions for most projections can be found at [Spatial Reference](http://spatialreference.org/)
