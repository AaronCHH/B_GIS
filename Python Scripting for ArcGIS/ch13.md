
# Chapter 13: Creating custom tools

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 13: Creating custom tools](#chapter-13-creating-custom-tools)
	- [13.1 Introduction](#131-introduction)
	- [13.2 Why create your own tools?](#132-why-create-your-own-tools)
	- [13.3 Steps to creating a tool](#133-steps-to-creating-a-tool)
	- [13.4 Editing tool code](#134-editing-tool-code)
	- [13.5 Exploring tool parameters](#135-exploring-tool-parameters)
	- [13.6 Setting tool parameters](#136-setting-tool-parameters)
	- [13.7 Examining an example script tool](#137-examining-an-example-script-tool)
	- [13.8 Customizing tool behavior](#138-customizing-tool-behavior)
	- [13.9 Working with messages](#139-working-with-messages)
	- [13.10 Handling m essages for stand-alone scripts and tools](#1310-handling-m-essages-for-stand-alone-scripts-and-tools)
	- [13.11 Customizing tool progress information](#1311-customizing-tool-progress-information)
	- [13.12 Running a script in process](#1312-running-a-script-in-process)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->

---

## 13.1 Introduction  

## 13.2 Why create your own tools?  

## 13.3 Steps to creating a tool  
```python
# Python script : copyfeatures.py
# This script copies all feature classes from a workspace into
# a file geodatabase.
# Import the ArcPy package .
import arcpy
ìmport os
# Set the curre nt workspace .
from arcpy import env
env.workspace "C:/Data"
# Create a lis t of feature classes the current workspace .
fclist = arcpy.ListFeatureClasses()
# Copy each feature class to a file geodatabase - keep the same
# name but use the basename property to remove any file
# extensions , including.shp .
for fc in fclist:
	fcdesc = arcpy.Describe(fc)
	arcpy.CopyFeatures_management(fc, os.path.join("C:/Data/study.gdb/", fcdesc.basename))
```

## 13.4 Editing tool code  

## 13.5 Exploring tool parameters  

## 13.6 Setting tool parameters  

## 13.7 Examining an example script tool  

## 13.8 Customizing tool behavior  

## 13.9 Working with messages  
```python
import arcpy
fc = arcpy.GetParameterAsText(0)
result = arcpy.GetCount_management(fc)
fcount = int(result.getOutput(0))
if fcount == 0 :
	arcpy.AddError(fc + " has no features .")
else:
	arcpy.AddMessage(fc + " has " + str(fcount) + "features.")
```    
## 13.10 Handling m essages for stand-alone scripts and tools  

## 13.11 Customizing tool progress information  
```python
import arcpy
import os
from arcpy import env
env.workspace = arcpy.GetParameterAsText(0)
outworkspace = arcpy.GetParameterAsText(1)
fclist = arcpy.ListFeatureClasses()
fcount = len(fclist)
arcpy.SetProgressor("step", "Copying shapefiles to geodatabase.", 0, fcount, 1)
for fc in fclist:
	arcpy.SetProgressorLabel("Copying" + fc + "...")
	fcdesc = arcpy.Describe(fc)
	outfc = os.path.join(outworkspace, fcdesc.baseName)
	arcpy.CopyFeatures_management(fc, outfc)
	arcpy.SetProgressorPosition()
arcpy.ResetProgressor()
```    

## 13.12 Running a script in process  

## Points to remember  
