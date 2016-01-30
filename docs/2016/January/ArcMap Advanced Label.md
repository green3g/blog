# Advanced Labeling in ArcMap - One feature, many labels
When labeling items, sometimes you may want to have multiple values for a label, like for example, a polygon may contain several different zoning codes in a city block. How can we label each block with the specific zones that it it contains?

Essentially, we have three tables:

A polygon block table with the block ID

Block ID | SHAPE
-------- | -------
1        | Polygon
2        | Polygon
3        | Polygon
4        | Polygon

A polygon zone table with a zone code

Zone Code | SHAPE
--------- | -------
Com       | Polygon
Res       | Polygon
Ind       | Polygon

We can do a spatial join that is one to many Zone polygons to Block polygons which produces a 3rd table.

Zone Code | Block ID | SHAPE
--------- | -------- | -------
Com       | 1        | Polygon
Ind       | 1        | Polygon
Res       | 1        | Polygon
Ind       | 3        | Polygon
Com       | 3        | Polygon
Com       | 2        | Polygon
Res       | 4        | Polygon

Now we can relate the Block table (#1) to the Zone-Block table (#3)

The script below helps automate this by accepting a function and the table names as parameters and outputting the formatted lines for each one-many item.

```python
# Calculates the dynamic expression for a layer and can be used
# to dynamically display related records text for data driven pages
# or feature labels

# Change the FindLabel parameter to the id field of the main table (ArcMap uses brackets in the [Field_Name])
# This id should be related to each of the related tables
# Also change the first line of FindLabel. Set parameter equal to the FindLabel parameter
def FindLabel ( [Block_ID] ):
    return get_lookup_text([Block_ID])

# create formatter functions to return what you want displayed
# for each row. These should return plain text for each row.

# returns a string with the block zone
def get_block(row):
    return row["Zone_Code"]

# change the lookup tables and assign their appropriate formatter function
# Properties of each lookup item:
# table: the name of the table in the mxd
# row_formatter: the function that will format each row's properties and return
# the text string
# id_field: the name of the field in the table that is related to the
# id passed in to FindLabel
# fields: list of fields by name (optiononal) default '*'
lookup_tables = [{
    "table": "Related_Zones",
    "row_formatter": get_parcel,
    "id_field": "Block ID",
    "fields": ['Block_ID', 'Zone_Code'],
    "header": 'Block Zones:',
},
#other lookup tables...
]

#the guts
def get_lookup_text(id):
    # init the return value
    text = ''
    if id is not None:
        for table in lookup_tables:
            if table.has_key('header'):
                text += "{} \n".format(table['header'])
            if type(id) is str or type(id) is unicode:
                query = "{} = '{}'".format(table["id_field"], id)
            else:
                query = "{} = {}".format(table["id_field"], id)
            fields = table['fields'] if table.has_key('fields') else '*'
            with arcpy.da.SearchCursor(table["table"], fields, where_clause=query) as rows:
                for row in rows:
                    dictionary = dict()
                    for idx, val in enumerate(rows.fields):
                        dictionary[val] = row[idx]
                    text += "{} \n".format(table["row_formatter"](dictionary))
    return text
```
