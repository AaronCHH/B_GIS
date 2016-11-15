# Chapter 5: Geoprocessing using Python  

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 5: Geoprocessing using Python](#chapter-5-geoprocessing-using-python)
	- [5.1 Introduction](#51-introduction)
	- [5.2 Using the ArcPy site package](#52-using-the-arcpy-site-package)
	- [5.3 Importing ArcPy](#53-importing-arcpy)
	- [5.4 Working with earlier versions of ArcGIS](#54-working-with-earlier-versions-of-arcgis)
	- [5.5 Using tools](#55-using-tools)
	- [5.6 Working with toolboxes](#56-working-with-toolboxes)
	- [5.7 Using functions](#57-using-functions)
	- [5.8 Using classes](#58-using-classes)
	- [5.9 Using environment settings](#59-using-environment-settings)
	- [5.10 Working with tool messages](#510-working-with-tool-messages)
	- [5.11 Working with licenses](#511-working-with-licenses)
	- [5.12 Accessing ArcGIS Desktop Help](#512-accessing-arcgis-desktop-help)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->


---
## 5.1 Introduction  
## 5.2 Using the ArcPy site package  
## 5.3 Importing ArcPy  
```
import arcpy
```

```
import arcpy.mapping
import arcpy.sa
```

```
import arcpy
arcpyenv.workspace = "C:/Data"
```

```
arcpy.<class>.<property>
```

```
from arcpy import env
env.workspace = "C:/Data"
```

```
from arcpy import env as myenv
myenv.workspace = "C:/Data"
```

```
from arcpy import env as *
workspace = "C:/Data"
```

## 5.4 Working with earlier versions of ArcGIS  
```
import ArcGISscripting
gp = ArcGISscripting.create(9.3)
```

```
import ArcGISscripting
gp = ArcGISscripting.create()
```

```
import win32com.client
win32com.client.Dispatch("esriGeoprocessing.GpDispatch.1")
gp.workspace = "C:/Data"
```

## 5.5 Using tools  
```python
# arcpy.<toolname_toolboxalias>(<parameters>)
import arcpy
arcpy.env.workspace = "C:/Data"
arcpy.Clip_analysis("streams.shp","study.shp","result.shp")
```
```python
# arcpy.<toolboxalias>.<toolname>(<parameters>)
import arcpy
arcpy.env.workspace = "C:/Data"
arcpy.analysis.Clip("streams.shp","study.shp","result.shp")
```
```python
Clip_analysis(in_features, clip_features, out_feature_class, {cluster tolerance})
```

* Parameters are separated by a comma(,). Optional parameters are surrounded by curly brackets({})  
```
Buffer analysis(in_features, out_feature_c1ass, buffer_distance or fie1d ,{line_side} ,{line_end_type} ,{disso1ve_option}, {dissolve_fie1d})
```
```python
# example
import arcpy
arcpy.env.workspace = "C:/Data/study.gdb"
arcpy.Buffer_analysis("road", "buffer", "100 METERS")
```
```python
arcpy.Buffer_analysis("road ", "buffer", "100 METERS", "", "", "LIST", "Code")
arcpy.Buffer_analysis("roads", "buffer", "100 METERS", "#", "#", "LIST", "Code")
arcpy.Buffer_analysis("roads", "buffer", "100 METERS", dissolve_option = "LIST", dissolve_field="Code")
```
```python
import arcpy
arcpy.env.workspace = "C:/Data"
infc = "streams.shp"
clipfc = "study.shp"
outfc = "result.shp"
arcpy.Clip_analysis(infc, clipfc, outfc)
```
```python
import arcpy
infc = arcpy.GetParameterAsText(0)
outfc_arcpy.GetParameterAsText(1)
arcpy.Copy_management(infc, outfc)
```
* return object
```python
import arcpy
arcpy.env.workspace = "C:/Data"
mycount = arcpy.GetCount_management("streams.shp")
print mycount
```
```python
import arcpy
arcpy.env.workspace = "C:/Data/study.gdb"
buffer = arcpy.Buffer_analysis("str","str_buf","100 METERS")
count = arcpy.GetCount_management(buffer)
print count
```
```python
import arcpy
arcpy.env.workspace = "C:/Data/study.gdb "
buffer arcpy.Buffer_ana1ysis("str", "str_buf", "100 METERS")
count = arcpy.GetCount_management(buffer).getOutput(0)
print str(count)
```

## 5.6 Working with toolboxes  
```python
import arcpy
arcpy.importToolbox("C:/Data/sampletools.tbx")
```
```python
arcpy.<toolname>_<toolboxalias>
arcpy.ImpotToolbox("sampletools.tbx", mytools)
```
```python
arcpy.MyModel_mytools(<parameters>)
arcpy.mytools.MyModel(<parameters>)
```
```python
import arcpy
tools = arcpy.ListTools("*_analysis")
for tool in tools:
	print arcpy.Usage(tool)
```
```python
print help(arcpy.Clip_analysis)
```

## 5.7 Using functions  
```python
arcpy.<functionname>(<arguments>)
```
```python
import arcpy
print arcpy.Exists("C:/Data.streams.shp")
```

## 5.8 Using classes  
```python
<classname>.<property> = <value>
```
```python
import arcpy
arcpy.env.workspace = "C:/Data"
```
```python
arcpy.<classname>(parameters)
```
```python
import arcpy
prjfile = "C:/Data/myprojection.prj"
spatialref = arcpy.SpatialReference(prjfile)
```
```python
import arcpy
prjfile = "C:/Data/streams.prj"
spatialref = arcpy.SpatialReference(prjfile)
myref = spatialRef.name
print myref
```
```
NAD_1983_StateP1ane_Florida_East_FIPS_0901_Feet
```
```python
CreateFeatureclass_management(out_path, out_name, {geometry type},
+ {template}, {has_m}, {has_z}, {spatal_reference}, {config_keyword},
+ {spatial_grid_1}, {spatial_grid_2}, {spatal_grid_3 )
```
```python
import arcpy
out_path = "C:/Data"
out_name = "lines.shp"
prjfle = "C:/Data/streams.prj"
spatialref = arcpy.SpatialReference(prjfile)
arcpy.CreateFeatureclass_management(out_path, out_name, "POLYLINE", "", "", "", spatialref)
```

## 5.9 Using environment settings  
```python
arcpy.env.<environmentName>
```
```python
import arcpy
arcpy.env.workspace = "C:/Data"
```
```python
from arcpy import env
env.workspace = "C:/Data"
```
```python
from arcpy import env
env.cellSize = 30
```
```python
from arcpy import env
print env.XYTolerance
```
* To get a complete list of properties, you can use the ArcPy ListEnvironments function:  
```python
import arcpy
print arcpy.ListEnvironments()
```
```python
import arcpy
from arcpy import env
env.overwriteOutput = True
```

## 5.10 Working with tool messages  
```python
print arcpy.GetMessages()
```
```python
import arcpy
arcpy.env.workspace = "C:/Data"
infc = "stream.shp"
clipfc = "study.shp"
outfc = "result.shp"
arcpy.Clip_analysis(infc, clipfc, outfc)
print arcpy.GetMessages()
```
* Get first message only
```python
print arcpy.GetMessages(0)
```
```python
print arcpy.GetMessageCount(0)
```
```python
count = arcpy.GetMessageCount()
print arcpy.GetMessage(count - 1)
```
```python
print arcpy.GetMaxSeverity()
```
```python
import arcpy
arcpy.env.workspace = "C:/Data"
result = arcpy.GetCount_management("streams.shp")
count = result.GetMessageCount
print result.getMessage(count - 1)
```
* Example of unlicnesed
```python
import arcpy
arcpy.sa.Slope("C:/Data/dem", "DEGREE")
```
* ProductInfo
```python
import arcpy
print arcpy.ProductInfo()
```
* CheckEstension (available?)
```python
import arcpy
print arcpy.CheckExtension("spatial")
```

## 5.11 Working with licenses  
```python
import arcpyfrom arcpy import envenv.workspace = "C:/Data"
if arcpy.CheckExtension("3D") == "Available":
	arcpy.CheckOutExtension("3D")
	arcpy.Slope_3d("dem", "slope", "DEGREE")
	arcpy.CheckInExtension("3D")
else:
	print "3D Analyst license is unavailable."
```

## 5.12 Accessing ArcGIS Desktop Help  
```
Help > ArcGIS Desktop Help
Geoprocessing > Python
Geoprocessing > ArcPy
ArcPy > ArcPy function > General Data function
Geoprocessing > Tool reference
```

## Points to remember  
