# ArcMap Update Layer Descriptions to Stored Metadata

ArcGIS Server stores a lot of information about each layer, including SOME of the metadata that is kept in an ArcGIS shapefile, or database feature class. The reason I say some, is that only the Description of the metadata is actually stored in the layer in the mxd, and that is the only text that gets published. In addition to this problem, the metadata is not automatically updated in an mxd when you edit the metadata in the feature class, and the only way to update that metadata description is to remove the layer from the mxd, and re-add it. Or you can do it manually with a script.

The script below iterates through each layer in the current mxd and exports the metadata to an xml. This is necessary since ArcPy doesn't have direct access to the metadata. Then it parses the xml document, and searches for the 'abstract', 'purpose', and 'credit', creates a nice looking string and sets the layer description to that string. The script can easily be modified to include less or more depending on your metadata. It may not work for all metadata types, but it worked for our setup.

```python
import arcpy,xml #set local variables
dir = arcpy.GetInstallInfo("desktop")["InstallDir"] translator = dir + "Metadata/Translator/ESRI_ISO2ISO19139.xml"
mxd = arcpy.mapping.MapDocument("CURRENT")
df = arcpy.mapping.ListDataFrames(mxd)[0] temp_path = "C:/temp"
for layer in arcpy.mapping.ListLayers(mxd, "*", df[0]):
     if not layer.isGroupLayer:
         description_text = ""
         path = temp_path + '/' + layer.datasetName + '.xml'
         print path
         arcpy.ExportMetadata_conversion(layer.dataSource, translator, path)
         dom = xml.dom.minidom.parse(path)
         fields = ('abstract', 'purpose', 'credit')
         for field in fields:
             tags = dom.getElementsByTagName(field)
             print str( len(tags) ) + ' | ' + str( tags )
             if len(tags):
                 tag_string = tags[0].getElementsByTagName('gco:CharacterString')[0].childNodes[0].nodeValue
                 description_text = description_text + "\n\n" + field.capitalize() + ":\n" + tag_string
                 print description_text
                 layer.description = description_text
                   if field == 'credit':
                        layer.credit = tag_string
```
