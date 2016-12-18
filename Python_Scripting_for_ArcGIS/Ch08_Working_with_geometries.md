# Chapter 8: Working with geometries  

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 8: Working with geometries](#chapter-8-working-with-geometries)
	- [8.1 Introduction](#81-introduction)
	- [8.2 Working with geometry objects](#82-working-with-geometry-objects)
	- [8.3 Reading geometries](#83-reading-geometries)
	- [8.4 Working with multipart features](#84-working-with-multipart-features)
	- [8.5 Working with polygons with holes](#85-working-with-polygons-with-holes)
	- [8.6 Writing geometries](#86-writing-geometries)
	- [8.7 Using cursors to set the spatial reference](#87-using-cursors-to-set-the-spatial-reference)
	- [8.8 Using geometry objects to work with geoprocessing tools](#88-using-geometry-objects-to-work-with-geoprocessing-tools)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->

---
## 8.1 Introduction  

## 8.2 Working with geometry objects  
* The following example will determine the combined length of all features in a polyline feature class:
```python
import arcpy
fc = "C:/Data/roads.shp"
cursor = arcpy.da.SearchCursor(fc, ["SHAPE@LENGTH"])
length = 0
for row in cursor:
	length += row[0]
print length
```

## 8.3 Reading geometries

 * In the following example, a search cursor and a for loop are used to iterate over the rows of a point feature class called hospitals.shp.  
A geometry token is used to retrieve the x,y coordinates of the point objects, which are then printed.  
The script is as follows:  
```python
import arcpy
fc = "C:/Data/hospitals.shp"
cursor = arcpy.da.SearchCursor(fc, ["SHAPE@XY"])
for row in cursor:
	x, y = row[0]
	print("{0}, {1}".format(x, y))
```

* The hospitals shape file, which is a point feature class, is shown in the figure Each point has a pair of x,y coordinates.

* A for loop is used to iterate over all the point objects in the array and print the x,y coordinates.  
The code is as follows:
```python
import arcpy
from arcpy impor env
env.workspace = "C:/Data"
fc = "roads.shp"
cursor = arcpy.da.SearchCursor(fc,["OIO@", "SHAPE@"])
for row cursor:
	print "Feature {0}: ".format(row[0])
	for point in row [1].getPart(0) :
		print("{0}, {1}".format(point.X, point.Y)
```

## 8.4 Working with multipart features  
* The following example code illustrates how this is accomplished for polyline and polygon feature classes:
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
fc = "roads.shp"
cursor = arcpy.SearchCursor(fc,["OIO@ ", "SHAPE@"])
for row in cursor:
	print ("Feature {0}: ".format(row[0]))
	partnum = 0
	for part in row[1] :
		print ("part {0}:".format(partnum))
		for point in part :
			print ("{0},{1}".format(point.X, point.Y))
		partnum += 1
```

## 8.5 Working with polygons with holes
* A script to read the geometry of polygons with ho1es is very simi1ar to the script in the preceding section for mu1tipart features.
The third for loop is replaced by the following:


## 8.6 Writing geometries  
* The CreateFeatureclass function can be used to create a new, empty feature class, which will be used to hold the new point objects whose coordinates are taken from the preceding list.  
The syntax of this tool is as follows:
```python
CreateFeature class_management(out_path, out_name, {geometry_type},  {template}, {has_m}, {has_z}, {spatial_reference}, {conf_keyword}, {spatlal_grid_1}, {spatial_grid_2}, {spatial_grid})
```

* The only required parameters are the path for the location of the new feature class (folder or geodatabase) and the name of the new feature class.  
The default value of the geometry is Polygon.  
There is no default for the spatial reference, so if none is specified, the coordinate system will be "unknown."   
The first part of the script is as follows:  
```python
import arcpy, fileinput, string
from arcpy mport env
env.overwriteOutput = True
infile = "C:/Data/points.txt"
fc = "C:/Data/newpoly.shp"
arcpy.CreateFeatureclass_management("C:/Data", fc, "polygon")
```

* In addition, an insert cursor is created to make it possible to create new rows-that is, new features.  
These lines of code are as follows:
```python
cursor = arcpy.da.InsertCursor(fc, ["SHAPE@"])
array = arcpy.Array()
point = arcpy.Point()
```

* Next, the properties of the point objects have to be set using the values in the text file.  
This requires the fileinput Python module to read the text file, and the split method to parse the text into separate strings for the point ID number, the x-coordinate, and the y-coordinate.  
These lines of code are as follows:  
```python
for line in fileinput.input(infile):
	point.ID, point.X, point.Y = line.split()
```

* Finally, the script needs to iterate over the lines of the input text file and create a point object for every line.  
The result is a single array with 21 point objects.  
The completed script is as follows:  
```python
import arcpy, fileinput, os
from arcpy import env
env.workspace = "C:/Data"
infile = "C:/Data/points.txt"
fc = "newpoly.shp"
arcpy.CreateFeatureclass_management("C:/Data", fc, "Polygon")
cursor arcpy.da.InsertCursor(fc, ["SHAPE@"])
array = arcpy.Array()
point = arcpy.Point()
for line in fileinput.input(infile):
	point.ID, point.X, point.Y line.split()
	line_array.add(point)
polygon = arcpy.Polygon(array)
cursor.insertRow([polygon])
fileinput.close()
del cur
```

## 8.7 Using cursors to set the spatial reference  
* The SearchCursor function is used to establish a read-only cursor on the state plane coordinates of the feature class, but the spatial reference of this cursor is set to the desired geographic coordinate system, in decimal degrees.  
This is accomplished using the following code:  
```python
import arcpy
fc = "C:/Data/hospitals.shp"
prjfile = "C:/projections/GCS_NAD_1983.prj"
spatialref = arcpy.SpatialReference(prjfile)
cursor = arcpy.da.SearchCursor(fc, ["SHAPE@"], "", spatialref)
```

* Next, an output file is created, using the open function.  
This opens the file in writing mode ("w") so that new lines of text can be written to it, follows:  
```python
output = open ("result.txt", "w")
```

* The next step is to iterate over the rows, create a geometry object for each row, and write the x, y coordinates to the output file using the write method.  
This part of the code is as follows:  
```python
for row in cursor:
	point = row[0]
	output.write(str(point.X) + "" + str(point.Y) + "\n")
```

* The coordinates are written as decimal degrees in a string, with a space (" ") separating the coordinates of each pair, and with a line break ("\n") for each point object.  
The very last step is to close the output file using the close method.  
The complete code is as follows:  
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
fc = hospitals.shp
prjfile = "C:/Projections/GCS_NAD_1983.prj"
patialref = arcpy.SpatialReference(prjfile)
cursor = arcpy.da.SearchCursor(fc, ["SHAPE@"],
output = open ( " result . txt " , " w" )
for row in cursor:
	point = row[0]
	output.write(str(point.X) + "" + str(point.Y) + "\n")
output.close( )
```

## 8.8 Using geometry objects to work with geoprocessing tools  
* For example, the following code creates a list of geometry objects from a list of coordinates, and then uses the geometry objects as input to the Buffer tool, as follows:  
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data"
coordlist = [[17.0, 20.0], [125.0, 32.0], [4.0, 87.0]]
pointlist = []
for x, y in coordlist:
	point = arcpy.Point(x, y)
	pointgeometry = arcpy.PointGeometry(point)
	pointlist.append(pointgeometry)
arcpy.Buffer_ana1ysis(pointlist, "buffer.shp", "10 METERS")
```

* Geometry objects can also be created directly as the output of geoprocessing tools.  
For example, the following code uses an empty geometry object as the output of the Copy Features tool, and the result is a list of geometry objects, as follows:  
```python
import arcpy
fc = "C:/Data/roads.shp"
geolist = arcpy.CopyFeatures_management(fc, arcpy.Geometry())
length = 0
for geometry in geolist:
	length += geometry.length
	print "Total length:" + length
```

## Points to remember  
