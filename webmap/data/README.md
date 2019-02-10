## Procedure to get velocities

# get glacier contours from glims database:http://glims.colorado.edu/glacierdata/
# get velocities

## list attribute values from polygons
#ogrinfo -so -al glims_polygons.shp

## list anlys_time where line_type='glac_bound'
#iogrinfo glims_polygons.shp -dialect sqlite -sql "SELECT anlys_time FROM glims_polygons WHERE line_type='glac_bound'"

## extract glac_bound
## for argentiere
#ogr2ogr -sql "SELECT * FROM glims_polygons WHERE line_type='glac_bound' AND anlys_time='2013-03-20T00:00:00'" bound1.shp glims_polygons.shp
## mer de glace
##ogr2ogr -sql "SELECT * FROM glims_polygons WHERE line_type='glac_bound' AND anlys_time='2010-06-01T00:00:00'" bound1.shp glims_polygons.shp

## combine shapefiles
#ogr2ogr combined.shp bound1.shp 
#ogr2ogr -append -update combined.shp PATH_TO_SECOND_FILE/bound1.shp 

## get velocities in EPSG 4326 , and clip with shape file
#gdalwarp -overwrite  -s_srs EPSG:32632 -t_srs EPSG:4326 -te 6.879 45.839 7.045 45.98 -tr 0.00022 0.00022 -q -cutline combined.shp -dstnodata 0.0 Velocity_NorthSouth.tif Velocity_V.tif
#gdalwarp -overwrite  -s_srs EPSG:32632 -t_srs EPSG:4326 -te 6.879 45.839 7.045 45.98 -tr 0.00022 0.00022 -q -cutline combined.shp -dstnodata 0.0 Velocity_EastWest.tif Velocity_U.tif

## convert to asc
#gdal_translate -of AAIGrid   Velocity_V.tif Velocity_V.asc
#gdal_translate -of AAIGrid   Velocity_U.tif Velocity_U.asc
