This is a quick and dirty guide on how to set up SQLite for ArcGIS and its ST_Geometry type, since the documentation didn't exactly work as expected.

[The guide on Esri's Documentation](http://desktop.arcgis.com/en/arcmap/10.3/manage-data/databases/sqlite-and-arcgis.htm) says the only thing you need to do is install the extension.

> Load the ST_Geometry library.
> This example loads the ST_Geometry library to an SQLite database on a Windows computer:
> ```sql
SELECT load_extension('stgeometry_sqlite.dll','SDE_SQL_funcs_init');`
SELECT CreateOGCTables();
```

But before we can do this we need to do configure a few system items.

#### Download and install SQLite and the command line utility 

[from the downloads page](https://www.sqlite.org/download.html)

In my case, I downloaded: 
 - sqlite-tools-win32-x86-3120200.zip
 - sqlite-dll-win32-x86-3120200.zip

And extracted the files to C:\sqlite3

#### Modify path variables

ArcGIS doesn't automatically modify the path's to allow for installation of these extensions. In fact, I found there are two paths that need to be added. I also added C:\sqlite3 for convenience.

 - C:\Program Files (x86)\ArcGIS\Desktop10.4\DatabaseSupport\SQLite\Windows32;
 - C:\Program Files (x86)\ArcGIS\Desktop10.4\bin;
 - C:\sqlite;
 
Once these specified paths are configured, the commands above STILL don't work. Instead the message `The specified module could not be found` is displayed.

This could be because of a mixup of using 32 bit sqlite with Windows32 folder in ArcGIS. But in order to make it work, I had to use the FULL PATH the dll file.

```sql
SELECT load_extension(
  'C:\Program Files (x86)\ArcGIS\Desktop10.4\DatabaseSupport\SQLite\Windows32\stgeometry_sqlite.dll', 
  'SDE_SQL_funcs_init');
SELECT CreateOGCTables();
```
