# lf-wms-wfs
leaftlet app with wms/wfs

Example see - https://webgismd.github.io/lf-wms-wfs/lfg/lfwmsi.html?l=WHSE_WATER_MANAGEMENT.GW_WATER_WELLS_LITHOLOGY_SP&s=1842&a=WELL_TAG_NUMBER&f=WELL_TAG_NUMBER%3D71003<br>

or https://webgismd.github.io/lf-wms-wfs/lfg/lfwmsi.html?l=WHSE_BASEMAPPING.BCGS_2500_GRID&a=OBJECTID&f=OBJECTID%3D64688<br>

or https://webgismd.github.io/lf-wms-wfs/lfg/lfwmsi.html?l=WHSE_CADASTRE.PMBC_PARCEL_FABRIC_POLY_FA_SVW&a=PLAN_ID&f=PLAN_ID%3D495161

Leaflet app that has URL parameters for WMS parameters (zoom to and identify functions available based on parameters given) <br>
<br>
URL PARAMETERS:<br>
WMS LAYER - l=WHSE_WATER_MANAGEMENT.GW_WATER_WELLS_LITHOLOGY_SP  (layer name)<br>
WMS LAYER STYLE - s=1842 (style name)<br>
WMS LAYER ATTRIBUTE TO FILTER ON - a=WELL_TAG_NUMBER (attribute/field name)<br>
WMS LAYER CQL_FILTER - f=WELL_TAG_NUMBER%3D71003 (encoded cql filter statement)<br>

to find style name go to:<br> http://openmaps.gov.bc.ca/geo/pub/WHSE_WATER_MANAGEMENT.GW_WATER_WELLS_LITHOLOGY_SP/wms?SERVICE=WMS&REQUEST=Getcapabilities<br><br>
to find attributes go to:<br> http://openmaps.gov.bc.ca/geo/pub/WHSE_WATER_MANAGEMENT.GW_WATER_WELLS_LITHOLOGY_SP/wfs?SERVICE=WFS&REQUEST=describefeaturetype<br><br>
to get the first feature and the feature count of a layer in JSON:<br>
http://openmaps.gov.bc.ca/geo/pub/wfs?SERVICE=WFS&VERSION=2.0.0&REQUEST=GetFeature&outputFormat=json&typeName=WHSE_BASEMAPPING.GRID_HEX_CDN_25_SQKM_SP&count=1<br><br>

references: <br> 
https://docs.geoserver.org/<br>
https://docs.geoserver.org/latest/en/user/services/index.html#services<br>
https://catalogue.data.gov.bc.ca/dataset/6164a2af-d3ac-4e92-8dbe-51a93bb5e24b<br>
https://leafletjs.com/reference-1.6.0.html
