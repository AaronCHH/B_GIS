# Chapter 6: Exploring spatial data  

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 6: Exploring spatial data](#chapter-6-exploring-spatial-data)
	- [6.1 Introduction](#61-introduction)
	- [6.2 Checking for the existence of data](#62-checking-for-the-existence-of-data)
	- [6.3 Describing data](#63-describing-data)
	- [6.4 Listing data](#64-listing-data)
	- [6.5 Using lists in for loops](#65-using-lists-in-for-loops)
	- [6.6 Working with lists](#66-working-with-lists)
	- [6.7 Working with tuples](#67-working-with-tuples)
	- [6.8 Working with dictionaries](#68-working-with-dictionaries)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->

---
## 6.1 Introduction  

## 6.2 Checking for the existence of data  
```python
arcpy.Exists(<dataset>)
import arcpy
print = arcpy.Exists("C:/Data/streams.shp")
```

## 6.3 Describing data  

```python
import arcpy
<variable> = arcpy.Describe(<input dataset>)
```

```python
import arcpy
desc = arcpy.Describe("C:/Data/stream.shp")
print desc.shapeType
```

```python
import arcpy
arcpy.env.workspace = "C:/Data"
infc = "stream.shp"
clipfc = "study.shp"
outfc = "result.shp"
desc = arcpy.Describe(clipfc)
type = desc.shapeType
if type == "Polygon":
	arcpy.Clip_analysis(infc, clipfc, outfc)
else:
	print "The clip features are not polygon"
```

```python
import arcpy
fc = "C:/Data/streams.shp"
desc = arcpy.Describe(fc)
sr = desc.spatialReference
print "Dataset type: " + desc.datasetType
print "spatial reference: " + sr.name
```

```python
import arcpy
arcpy.env.workspace = "C:/Data/study.gdb"
element = "road"
desc = arcpy.Describe(element)
print "Data type: " + desc.dataType
print "File path: " + desc.path
print "Catalog path: " + desc.catalogPath
print "File name: " + desc.file
print "Base name: " + desc.baseName
print "Name: " + desc.name
```

## 6.4 Listing data  
```
[syntax]
The ArcPy list functions include
ListFields, ListIndexes, ListDatasets,
ListFeatureClasses, ListFiles, ListRasters, ListTables,
ListWorkspaces, and ListVersions .
```
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
fclist = arcpy.ListFeatureClasses()
print fclist
```
* The List FeatureClasses function returns a list of feature classes in the current workspace.  
	The syntax of the function is:
```python
ListFeatureClasses({wild card}, {feature type}, {feature dataset})
```
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
fclist = arcpy.ListFeatureClasses()
print fclist
```
* The wild card can be used to limit the list by name.  
```python
fclist = arcpy.ListFeatureClasses("w*")
```
* The second parameter in the ListFeatureClasses function is the feature
type.   
```python
fclist = arcpy.ListFeatureClasses("", "point")
```
* To create a list of raster datasets in the current workspace, you
can use the ListRasters function.
```python
ListRasters({wild_card), {raster_type})
```
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
rasterlist = arcpy.ListRasters()
```
```raster
rasterlist = arcpy.ListRasters("","tif")
```
* Another list function is ListFields.  
 	This function lists the fields in a feature class or table in a specified dataset.   
	 The syntax is
```python
ListFields(dataset, {wild card}, {field type})
```
* The ListFields function has three parameters-name, field type, and
dataset-of which the dataset is required.
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
fieldlist = arcpy.ListFieds("roads.shp")
```  
* The two optional parameters allow you to restrict the list by name or field
type.  
The following code creates a list of all the fields in a shapefile that are
integers:
```python
fieldlist = arcpy.ListFields("roads.shp", "", "Integer")
```
* The ListFields function returns a list of &eld objects.  
By contrast, most of the other list functions return a list of strings.  
Field object properties include the field name, alias, type, and length.
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
fieldlist = arcpy.ListFields("roads.shp", "", "String")
for field in fieldlist:
	print field.name + " " + str(field.length)
```

## 6.5 Using lists in for loops  
* A for loop can be used to iterate over the list, one element at a time, and when there
are no values left to be iterated, the loop is finished.
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
tifflist = arcpy.listRasters("","TIF")
for tiff in tifflist:
	arcpy.BuildPyramids_management(tiff)
```

* Iteration using a for loop can also be used in combination with the
ListFields function to provide a detailed description of the fields and
their properties.
```python
import arcpy
from arcpy mport env
env.workspace = "C:/Data"
fieldlist = arcpy.ListFields("roads.shp")
for field in fieldlist:
	print "{0} is a type of {1} with a length of {2}".format(field.name, field.type, field.length)
```

## 6.6 Working with lists  
* Lists are a versatile Python type and can be manipulated in many different ways.
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data/study.gdb"
fcs = arcpy.ListFeatureClasses()
print len(fcs)
```

* Lists can be sorted using the sort method.  
The default sorting is in alphanumerical order, but it can be reversed using the reverse argument of the sort method.
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data/study.gdb"
fcs = arcpy.ListFeatureClasses()
fcs.sort()
print fcs
fcs.sort(reverse = True)
print fcs
```

## 6.7 Working with tuples  
* The syntax of a tuple is simple - separate a set of values with commas (, ), and you have a tuple. For example, the following code returns a tuple with f1 ve elements:
```
>> 1, 2, 3, 4, 5
The result is (1, 2, 3, 4, 5)
```
```
>>> x = ("a", "b", "c")
>>> x[0]
The result is 'a'.
```
* However, the sequence of elements cannot be modified.
So list operations such as deleting, appending, removing, and others are not supported by tuples.
```
>>> x("a", "b", "c", "d", "e", "f", "g")
>>> x[2:5]
The result is ('c', 'd', 'e') .
```
* If you cannot modify a tuple, why are tuples important?  
First, some built-in Python functions and modules return tuples-in which case, you have to deal with them.  
Second, tuples are often used in dictionaries, which are covered next.

## 6.8 Working with dictionaries  
```
cities = ["Austin", "Baltimore", "Cleveland", "Denver"]
states = ["Texas", "Maryland", "Ohio", "Colorado"]
>>> states[cities.index("Cleveland")]
```
* A dictionary item consists of a key, followed by a colon (:), and then the corresponding value.   
Items are separated by a comma (,).
The dictionary itself is surrounded by curly brackets ({})
```
statelookup{"Austin":"Texas", "Baltimore":"Maryland", "Cleveland":"Ohio", "Denver":"Colorado"}
statelookup["Cleveland"]
```

* Dictionaries can be created and populated at the same time，as in the preceding statelookup example.  
You can also create an empty diction ary fust usi only curly brackets ({}), and then add items to it.
Here is the code to create a new empty dictionary:
```python
zoning = {}
zoning["RES"] = "Residential"
zoning["IND"] = "Industry"
zoning["WAT"] = "Water"
print zoning
```

* Items can be modified using the same syntax.  
Setting the value using a key that is already in use overwrites the existing value.
```python
zoning["IND"] = "Industrial"
print zoning
```

* Items can be deleted using square brackets ([]) and the keyword del as follows:
```python
del zoning("WAT")
zoning.keys()
zoning.values()
zoning.items()
```

* Dictionaries are not common in ArcPy, but there is one function that uses them: GetInstallinfo(install_name).  
This function returns a Python dictionary that contains the information on the installation type properties.
```python
import arcpy
install = arcpy.GetInstallInfo()
for key in install:
	print "{0}:{1}".format(key, install[key])
```

## Points to remember  
