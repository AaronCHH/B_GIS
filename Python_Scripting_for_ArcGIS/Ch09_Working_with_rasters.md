# Chapter 9: Working with rasters  
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 9: Working with rasters](#chapter-9-working-with-rasters)
	- [9.1 Introduction](#91-introduction)
	- [9.2 Listing rasters](#92-listing-rasters)
	- [9.3 Describing raster properties](#93-describing-raster-properties)
	- [9.4 Working with raster objects](#94-working-with-raster-objects)
	- [9.5 Working with the ArcPy Spatial Analyst module](#95-working-with-the-arcpy-spatial-analyst-module)
	- [9.6 Using map algebra operators](#96-using-map-algebra-operators)
	- [9.7 Using the ApplyEnvironment function](#97-using-the-applyenvironment-function)
	- [9.8 Using classes of the arcpy.sa module](#98-using-classes-of-the-arcpysa-module)
	- [9.9 Using raster functions to work with NumPy arrays](#99-using-raster-functions-to-work-with-numpy-arrays)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->

---
## 9.1 Introduction

## 9.2 Listing rasters
* The ListRasters function returns a Python list of rasters in a workspace.  
The syntax of the function is:  
```python
ListRasters({wild_card}, {raster_type})
```

* An optional wild_card parameter can be used to limit the list based on the name of the rasters.  
The optional raster_type parameter can be used to limit the list based on the type of raster-for example, JPEG or TIFF.  
The following code illustrates the use of the ListRasters function to print a list of the rasters in a workspace:  
```python
import arcpy
from arcpy import env
env.workspace = "C:/raster"
rasterlist = arcpy.listRasters()
for raster in rasterlist:
	print raster
```

* The parameters of the ListRasters function can be used to filter the results.  
For example, the following code prints a list of the rasters in the workspace that are in the ERDAS IMAGINE format:  
```python
import arcpy
from arcpy import env
env.workspace = "C:/raster"
rasterlist = arcpy.ListRasters("*", "IMG")
for raster in rasterlist:
	print raster
```


## 9.3 Describing raster properties
* The general dataType property can be used to determine the type of data element.  
All properties, however, are accessed using the same Describe function.  
The following code illustrates the use of the Describe function, which returns an object with properties that can be accessed, in this case for printing.  
```python
# pt1.py
import arcpy
from arcpy import env
env.workspace = "C:/raster"
raster = "landcover.tif"
desc = arcpy.Describe(raster)
print desc.dataType
```

* Once it has been determined that an element is a raster dataset, these properties can be accessed.  
For example, the following code includes additional properties used to describe the TIFF file.  
```python
# pt2.py
import arcpy
from arcpy import env
env.workspace = "C:/raster"
raster = "landcover.tif"
desc = arcpy.Describe(raster)
print desc.dataType
print desc.bandCount
print desc.compressionType
```

* For this particular example, the code returns values of 30.0 by 30.0 and U8 - this means the cell size is 30 by 30 meters and the pixel type is an unsigned 8-bit integer.  
These properties do not report the unit type, which has to be obtained from the Spatial Reference property.  
For example, the following code determines the name of the spatial reference and the unit:  
```python
spatialref = desc.spatialReference
print spatialref.name
print spatialref.linearUnitName
```

* For multiband rasters, however, the specific band needs to be specified.  
Without a particular band being specified, properties such as cell size, height, width, and pixel type cannot be accessed.  
Specific bands are referenced using Band_1 , Band_2, and so on.  
The following code illustrates h ow the properties for a band in a multiband raster dataset are accessed:  
```python
# pt3
import arcpy
from arcpy import env
env.workspace = "C:/raster"
rasterband = "img.tif/Band_1"
desc = arcpy.Describe(rasterband)
print desc.meanCellHeight
print desc.meanCellWidth
print desc.pixelType
```


## 9.4 Working with raster objects
* ArcPy also contains a Raster class that is used to reference a raster dataset.  
A raster object can be created in two ways: (1) by referencing an existing raster on disk and (2) by using a map algebra statement.  
The syntax for the Raster class is  
```python
Raster(inRaster)
```

* The following code illustrates how to create a raster object by referencing a raster on disk:  
```python
import arcpy
myraster = arcpy.Raster("C:/raster/elevation")
```

* When using map algebra statements, the code looks something like the following:   
```python
import arcpy
outraster arcpy.sa.Slope("C:/raster/elevation")
```

* Raster objects have only one method: save.  
The raster object (the variab1e and associated dataset) returned from a map a1gebra expression is temporary by default.  
This means the variable and the referenced dataset are de1eted when the variab1e goes out of scope-for example, when ArcGIS is closed or when a stand-alone script is closed.  
The save method can be called to make the raster object permanent.  
The syntax of the save method is  
```
save({name})
```

* In the earlier example, the raster object outraster is temporary but can be made permanent using the following code:  
```python
import arcpy
outraster = arcpy.sa.Slope("C:/raster/elevation)
outraster.save("C:/raster/slope")
```

## 9.5 Working with the ArcPy Spatial Analyst module
* Consider the following code that runs the Slope tool:
```python
import arcpy
elevraster = arcpy.Raster("C:/raster/elevation")
outraster = arcpy.sa.Slope(elevraster)
```

* Notice that the Slope tool is called using arcpy.sa.Slope, which appears to follow the regular syntax used for all tools: arcpy.<toolboxalias>.<toolname>.  
However, the alternative arcpy.<toolname>_<toolboxalias> syntax does not apply here, and arcpy.Slope_sa is not valid.  
Because sa is a module, and not just the alias of a toolbox, the code can be simplified as follows:  
```python
import arcpy
from arcpy.sa import *
elevraster = arcpy.Raster("C:/raster/elevation")
raster = Slope(elevraster)
```

## 9.6 Using map algebra operators
* In addition to providing access to all the Spatia1 Analyst geoprocessing tools, the arcpy.sa module also includes a number of map algebra operators.  
Most of these operators are available as geoprocessing too1s under the Math toolset in the SpatialAna1yst toolbox yet are also available as operators in Python.  
Consider the following example, which converts e1evation values from feet to meters using the Times too1:  
```python
import arcpy
from arcpy.sa import *
elevraster = arcpy.Raster("C:/raster/elevation")
utraster = Times(elevraster, "0.3048")
outraster.save("C:/raster/elev_m")
```

* Instead of using the Times tool, the map algebra operator (*) can be used.  
The second-to-last line of code would look as follows:  
```python
outraster = elevraster * 0.3048
```

* Consider the example of a suitability model in which you create three different rasters, each representing a different factor in the suitability model.  
In the final analysis step, you want to add these three rasters together and determine the average suitability score.  
Your code could look something like the following:  
```python
import arcpy
from arcpy.sa import *
fl = arcpy.Raster("C:/raster/factorl")
f2 = arcpy.Raster("C:/raster/factor2")
f3 = arcpy.Raster("C:/raster/factor3")
templraster = Plus(fl, f2)
temp2raster = Plus(templraster, f3 )
outraster = Divide(temp2raster, "3")
outraster.save("C:/raster/final")
```

* The Plus tool has to be used twice to add all three rasters together because the tool can use only two inputs at a time.  
The Divide tool is used to divide the sum of the three rasters by 3.  
Using map algebra expressions, this code can be reduced as follows:  
```python
import arcpy
from arcpy.sa import *
f1raster = arcpy.Raster("C:/raster/factor1")
f2raster = arcpy.Raster("C:/raster/factor2")
f3raster = arcpy.Raster("C:/raster/factor3")
outraster = (f1 + f2 + f3 ) / 3
outraster.save("C:/raster/final")
```

* It looks very much like the Python code used earlier.  
In effect, the map algebra operators in the arcpy.sa module allow you to create Raster Calculator-style expressions directly in Python.  
You can also call the Raster Calculator tool using the following syntax:  
```python
RasterCalculator(expression, output_raster)
```

## 9.7 Using the ApplyEnvironment function
* In addition to the geoprocessing tools in the Spatial Analyst toolbox, there is one more function: the ApplyEnvironment function.  
This function copies an existing raster and applies the current environment settings.  
The syntax of the function is:  
```python
ApplyEnvironment(inraster)
```

* This function allows you to change things like the extent or the cell size or to apply an analysis mask.  
The following code illustrates how the ApplyEnvironment function is used to set a new cell size of 30 and apply an analysis mask based on an existing shape file:  
```python
import arcpy
from arcpy import env
from arcpy.sa import *
elevfeet = arcpy.Raster ("C:/raster/elevation")
elevmeter = elevfeet * 0.3048
env.cellsize = 30
env.mask = "C:/raster/studyarea.shp"
elevrasterclip = ApplyEnvironment(elevmeter)
elevrasterclip.save("C:/raster/dem30_m")
```
* Not all environment settings apply to the ApplyEnvironment function.  
They are limited to the following: Cell Size, Current Workspace, Extent, Mask, Output Coordinate System, Scratch Workspace, and Snap Raster These are the most relevant environment settings when working with rasters.  


## 9.8 Using classes of the arcpy.sa module
* The tool dialog box in the figure shows an examp1e of a land-use raster being reclassified into a number of va1ues as part of a suitability model.  
```python
Reclassify(in_raster, reclass_field, remap, {missing_values})
```

* Typing all the values of this table would be rather complicated since this table can have many different entries.    
Instead, the remap parameter is expressed as a remap object.  
There are two different Remap classes, depending on the nature of the reclassification:

* The syntax of the RemapValue object is
```python
RemapValue(remapTable)
```

* A remapTable object is defined using a Python of lists that each contain old and new values, similar to the reclassification table on the tool dialog box.  
The syntax of a remap table for use in a RemapValue object is:  
```python
[[oldValue1, newValue1], [oldValue2, newValue2], ... ]
```  

* The following code illustrates the use of a remap object to carry out a reclassification of a raster representing land use:  
```python
import arcpy
from arcpy import env
from arcpy.sa import *
env.workspace = "C:/raster"
myremap = RemapValue([["Barren", 1], ["Mixed Forest", 4], ["Coniferous Forest", 0 ], ["Cropland", 2], ["Grassland", 3], ["Shrub",3], ["Water", 0]])
outreclass = Reclassify("landuse", "S_VALUE", myremap)
outreclass.save("C:/raster/lu_recl")
```

* The RemapRange object works in a similar manner but uses value ranges rather than individual values.  
The syntax of a remap table for use in a RemapRange object is  
```python
[[startValue, endValue, newValue], ...]
```

* The following code illustrates the use of a remap object to carry out a reclassification of a raster of elevation:  
```python
import arcpy
from arcpy import env
from arcpy . sa i mport *
env.workspace ("C:/raster")
myremap = RemapRange([[1, 1000, 0], [1000, 2000, 1], [2000, 3000, 2], [3000, 4000, 3]])
outreclass = Reclassify("elevation", "TYPE", myremap)
outreclass.save("C:/raster/elev_recl")
```

* Notice that the end value of the first range is the same as the start va1ue of the second range, and so on.  
This type of remap table is common when data is continuous, as in the case of a raster of elevation.  
In addition to the Reclassify tool, remap tables are a1so used in the Weighted Overlay tool.  


* There are many other classes in the arcpy.sa module.  
They can be grouped into a number of categories based on logical functionality.  
Table 9.2 lists these categories.  

* Among the more widely used classes, in addition to the Remap classes already discussed, are the Neighborhood classes, which define neighborhoods of different shapes and sizes.  
Consider, for example, the Focal Statistics tool.  
This tool, as well as other tools in the Neighborhood toolbox, requires the definition of a specific neighborhood.  

* The neighborhood settings vary with the type of neighborhood.  
For example, for the default rectangular neighborhood, settings include height and width in cell or map units.  
However, for the wedge neighborhood, the parameters include start angle and end angle and a radius in cell or map units.  

* Because of the variability of these parameters, neighborhood functions include a neighborhood object.  
For example, the syntax of the Focal Statistics tool is as follows:  
```python
FocalStatistics(in_raster, {neighborhood}, {statistics_type}, {ignore_nodata})
```

* For example, the syntax of the NbrRectangle object is  
```python
NbrRectangle({width}, {height}, {units})
```

* The following code defines a neighborhood object and uses it in the FocalStatistcs function:
```python
import arcpy
from arcpy import env
from arcpy.sa import *
env.workspace = "C:/raster"
mynbr = NbrRectangle(5, 5, "CELL")
outraster = FocalStatistics ("landuse", mynbr, "VARIETY")
outraster.save("C:/raster/lu_var")
```  

* In this example, the output is a raster of land cover based on a rectangular neighborhood of 5 cells by 5 cells.  

## 9.9 Using raster functions to work with NumPy arrays
* Rather than trying to build a tool in ArcGIS that carries out these specialized functions , you could write a script tool that converts a raster to a NumPy array, and then calls specialized functions from the SciPy package.  
A generic script would look as follows:  
```python
import arcpy, scipy
from arcpy.sa import *
inRaster = arcpy.Raster("C:/raster/myraster")
my_array = RasterToNumPyArray(inRaster)
outarray = scipy.<module>.<function>(my_array)
outraster = NumPyArrayToRaster(outarray)
outraster.save("C:/raster/result")
```
* This is a simplified example and references a generic SciPy function, yet it illustrates how NumPy array functions can be used to export data for processing in another environment and to import the result back into an ArcGIS-compatible format-all within the same Python script.  
More information on NumPy (Numerical Python) and SciPy (Scientific Library for Python) can be found at http://numpy.scipy.org and http://www.scipy.org, respectively  

## Points to remember
