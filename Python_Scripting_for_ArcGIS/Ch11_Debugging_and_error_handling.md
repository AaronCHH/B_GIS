# Chapter 11: Debugging and error handling  

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 11: Debugging and error handling](#chapter-11-debugging-and-error-handling)
	- [11.1 Introduction](#111-introduction)
	- [11.2 Recognizing syntax errors](#112-recognizing-syntax-errors)
	- [11.3 Recognizing exceptions](#113-recognizing-exceptions)
	- [11.4 Using debugging](#114-using-debugging)
	- [11.5 Using debugging tips and tricks](#115-using-debugging-tips-and-tricks)
	- [11.6 Error handling for exceptions](#116-error-handling-for-exceptions)
	- [11.7 Raising exceptions](#117-raising-exceptions)
	- [11.8 Handling exceptions](#118-handling-exceptions)
	- [11.9 Handling geoprocessing exceptions](#119-handling-geoprocessing-exceptions)
	- [11.10 Using other error- handling methods](#1110-using-other-error-handling-methods)
	- [11.11 Watching for common errors](#1111-watching-for-common-errors)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->

---
## 11.1 Introduction

## 11.2 Recognizing syntax errors
* Syntax errors pertain to spelling, punctuation, and indentation.  
Common syntax errors result from misspelled keywords or variables, missing punctuation, and inconsistent indentation.  
See if you can spot the error in the following code:  
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data/mydata.gdb"
fclist = arcpy.ListFeatureClasses()
for fc in fclist
	count = arcpy.GetCount_management(fc)
	print count
```


## 11.3 Recognizing exceptions
* Syntax errors are frustrating, but they are relatively easy to catch compared to other errors.  
Consider the following example that has the syntax corrected:  
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data/mydata.gdb"
fclist = arcpy.ListFeatureClasses()
for fc in fclist
	count = arcpy.GetCount_management(fc)
	print count
```

## 11.4 Using debugging

## 11.5 Using debugging tips and tricks

## 11.6 Error handling for exceptions

## 11.7 Raising exceptions

## 11.8 Handling exceptions

## 11.9 Handling geoprocessing exceptions

## 11.10 Using other error- handling methods

## 11.11 Watching for common errors

## Points to remember
