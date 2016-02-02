# OGR2OGR Cheatsheet

OGR is an incredibly versitile utility for any GIS data transfer task and should probably be a tool in everyone who needs to work with different types of data. OGR2OGR takes countless parameters (hence the cheatsheet) but when used correctly it can be used to automate tasks that would take hours using other methods. These are all batch files, but they can be easily converted to linux or mac bash files. 

## PostGIS to Shapefile
We'll start small. This one takes a single table and turns it into a shape file. Set the parameters and run. Easy enough.

```batch
set host="192.168.0.1"
set user="admin"
set password="password"
set dbname="gisdata"
set table="table"
set output="mydata.shp"
ogr2ogr -f "ESRI Shapefile" %output% PG:"host=%host% user=%user% dbname=%dbname% password=%password%" "%table%"
```

## CAD To Shapefile
This one is a little more exciting. It takes a cad file, and converts it into three shapefiles (points, lines, polygons). In order for this to work, it needs to have a dxf file input. To create a dxf from a dwg, AutoCad's Save As... menu can be used.

```batch
set ogr_path="C:\OSGeo4W\bin\ogr2ogr"
set dwg_path=%1
set output_path=%2

echo ---------------------------------
echo Starting script with parameters input filepath:%1 output filepath:%2_type.shp
echo ---------------------------------

%ogr_path% "%output_path%_points.shp" "%dwg_path%" -where "ogr_geometry = 'POINT'"
%ogr_path% "%output_path%_lines.shp" "%dwg_path%" -where "ogr_geometry = 'LINESTRING'"
%ogr_path% "%output_path%_polygon.shp" "%dwg_path%" -where "ogr_geometry = 'POLYGON'"
```

## Esri Geodatabase to PostGIS
This one I've found incredibly useful. It quickly transfers many layers from a Esri Geodatabase (In fact all layers), and places them into a schema in PostgreSQL. It can even reproject the database. Just set the parameters at the top and run the script.

```batch
set default_gdb=C:/data.gdb
set default_schema=mydata
set default_host=192.168.0.1
set default_database=gisdata
set default_user=gis
set PGPASSWORD=gis
set default_projection="EPSG:3857"
set ogr_path="C:\OSGeo4W\bin\ogr2ogr"
set pg_path="C:\Program Files (x86)\PostgreSQL\9.4\bin\psql"

echo Creating schema %schema%...
%pg_path% -h %default_host% --dbname=%default_database% --username=%default_user% --command="CREATE SCHEMA %schema%;"
echo Transferring data...
%ogr_path% -f "PostgreSQL" PG:"host=%default_host% dbname=%default_database% user=%default_user% active_schema=%schema%" -progress -overwrite -lco OVERWRITE=YES -t_srs %default_projection% -skipfailures %gdb%
```
