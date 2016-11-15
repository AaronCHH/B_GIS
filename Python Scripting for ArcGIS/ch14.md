# Chapter 14: Sharing tools

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 14: Sharing tools](#chapter-14-sharing-tools)
	- [14.1 Introduction](#141-introduction)
	- [14.2 Choosing a method for distributing tools](#142-choosing-a-method-for-distributing-tools)
	- [14.3 Handling licensing issues](#143-handling-licensing-issues)
	- [14.4 Using a standard folder structure for sharing tool](#144-using-a-standard-folder-structure-for-sharing-tool)
	- [14.5 Working with paths](#145-working-with-paths)
	- [14.6 Finding data and workspaces](#146-finding-data-and-workspaces)
	- [14.7 Creating a geoprocessing package](#147-creating-a-geoprocessing-package)
	- [14.8 Embedding scripts and password-protecting tools](#148-embedding-scripts-and-password-protecting-tools)
	- [14.9 Documenting tools](#149-documenting-tools)
	- [14.10 Example tool: Market analysis](#1410-example-tool-market-analysis)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->

---

## 14.1 Introduction  

## 14.2 Choosing a method for distributing tools  

## 14.3 Handling licensing issues  

## 14.4 Using a standard folder structure for sharing tool  

## 14.5 Working with paths  

## 14.6 Finding data and workspaces  
* The code that references the lookup.dbf file in the script is as follows:
```python
import arcpy
import os
import sys
scriptpath = sys.path[0]
toolpath = os path.dirname(scriptpath)
tooldatapath = os.path.join(toolpath, "ToolData")
datapath = os.path.join(tooldatapath, "lookup.dbf")
```
* A similar approach can be used to reference the location of a scratch workspace.  
Scratch workspaces are very common in models, and they can also be used in scripts.  
Using the same example folder structure as before, the script to set the scratch workspace is as follows:  
```python
import arcpy
import os
import sys
from arcpy import env
scratchws = env.scratchWorkspace
scriptpath = sys.path[0]
toolpath = os.path.dirname(scriptpath)
if not env.scratchWorkspace:
	scratchws = os.path.join(toolpath, "Scratch/scratch.gdb")
```





## 14.7 Creating a geoprocessing package  

## 14.8 Embedding scripts and password-protecting tools  

## 14.9 Documenting tools  

## 14.10 Example tool: Market analysis  

## Points to remember  
