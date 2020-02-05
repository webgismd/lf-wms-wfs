# lf-wms-wfs
leaftlet app with wms/wfs

Example see - http://delivery.openmaps.gov.bc.ca/kml/m/lf-wms-wfs-master/lfg/lfwmsi.html?l=WHSE_WATER_MANAGEMENT.GW_WATER_WELLS_LITHOLOGY_SP&s=1842&a=WELL_TAG_NUMBER&f=WELL_TAG_NUMBER%3D71003<br>

or http://delivery.openmaps.gov.bc.ca/kml/m/lfg103/lfwmsi2.html?l=WHSE_BASEMAPPING.BCGS_2500_GRID&a=OBJECTID&f=OBJECTID%3D64688<br>

Leaflet app that has URL parameters for WMS parameters (zoom to and identify functions available based on parameters given) <br>
<br>
URL PARAMETERS:<br>
WMS LAYER - l=WHSE_WATER_MANAGEMENT.GW_WATER_WELLS_LITHOLOGY_SP  (layer name)<br>
WMS LAYER STYLE - s=1842 (style name)<br>
WMS LAYER ATTRIBUTE TO FILTER ON - a=WELL_TAG_NUMBER (attribute/field name)<br>
WMS LAYER CQL_FILTER - f=WELL_TAG_NUMBER%3D71003 (encoded cql filter statement)<br>

to find style name go to http://openmaps.gov.bc.ca/geo/pub/WHSE_WATER_MANAGEMENT.GW_WATER_WELLS_LITHOLOGY_SP/wms?SERVICE=WMS&REQUEST=Getcapabilities<br><br>
to find attributes go to http://openmaps.gov.bc.ca/geo/pub/WHSE_WATER_MANAGEMENT.GW_WATER_WELLS_LITHOLOGY_SP/wfs?SERVICE=WFS&REQUEST=describefeaturetype<br><br>
to get the first feature and the feature count of a layer in JSON:
http://openmaps.gov.bc.ca/geo/pub/wfs?SERVICE=WFS&VERSION=2.0.0&REQUEST=GetFeature&outputFormat=json&typeName=WHSE_BASEMAPPING.GRID_HEX_CDN_25_SQKM_SP&count=1
