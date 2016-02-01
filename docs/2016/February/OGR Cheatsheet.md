## PostGIS to Shapefile

```batch
ogr2ogr -f "ESRI Shapefile" mydata.shp PG:"host=myhost user=myloginname dbname=mydbname password=mypassword" "mytable"
```

## CAD To Shapefile
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
