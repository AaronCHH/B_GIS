# Chapter 7: Manipulating spatial data

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 7: Manipulating spatial data](#chapter-7-manipulating-spatial-data)
	- [7.1 Introduction](#71-introduction)
	- [7.2 Using cursors to access data](#72-using-cursors-to-access-data)
	- [7.3 Using SQL in Python](#73-using-sql-in-python)
	- [7.4 Working with table and field names](#74-working-with-table-and-field-names)
	- [7.5 Parsing table and field names](#75-parsing-table-and-field-names)
	- [7.6 Working with text files](#76-working-with-text-files)
	- [7.7 Points to remember](#77-points-to-remember)

<!-- tocstop -->

---
## 7.1 Introduction  
## 7.2 Using cursors to access data  

| Cursor Type | Method | Description |
|:----------- |:------ | ----------- |
| Search      | next   | Retrieves the nex row 											|
|  				    | reset  | Resets teh cursor to its starging position |
| Insert      | insertRow | Inserts a row into the table |
|  				    | next 			| Retrieves teh next row object |
| Update      | deleteRow | Inserts a row into the table |
|  				    | next    	| Retrieves teh next row object |
|							| reset			| Resets the cursor to its starting position |
|							| updateRow | Updates the current row |

* All three cursors have two required arguments: an input table and a list (or tuple) of field names.  
Search and update cursors also have several optional arguments.  
The general syntax for each cursor is as follows:
```python
arcpy.da.InsertCursor(in_table, field_names)
arcpy.da.SearchCursor(in_table, field_names, {where_clause}, {spatial_reference}, (explore_to_points})
arcpy.da.UpdateCursor(in_table, field_names, {where_clause}, {spatial_reference}, (explore_to_polnts})
```

* Following is an example of using a search cursor to iterate over the rows in a table.  
In the example code that follows, the SearchCursor function retrieves the rows in a table.  
A for loop is used to it over the rows in the table and print the values for a given field.
```python
import arcpy
fc = "C:/Data/study.gdb/roads"
cursor = arcpy.da.SearchCursor(fc, ["STREETNAME"])
for row in cursor:
	print "Streetname = {0}".format(row[0])
```

* Search and update cursors also support with statements.  
A with statement has the advantage of guaranteeing closure and release of database locks and resetting iteration, regardless of whether the cursor finished running successfully or not.  
The previous example using a with statement is coded as follows:
```python
import arcpy
fc = "C:/Data/study.gdb/roads"
with arcpy.da.SearchCursor(fc, ["STREETNAME"]) as cursor :
for row in cursor:  
	print "Streetname = {0}".format(row[0])
```

* The following code illustrates how a new row is inserted.  
A cursor object is created using the InsertCursor function.  
Once the cursor is created, the insertRow method is used to insert a list of values that will be included by the new row.
```python
cursor = arcpy.da.InsertCursor(fc, ["STREETNAME"])
x = 1
while x <= 5 :
	cursor.insertRow(["NEW STREET"])
	x += 1
```

* In the following example, the cursor object is created using the UpdateCursor function.  
In the for loop, the values of one held (Acres) in the row are updated using the values of another held (Shape_Area).  
If the Shape_Area held is in square feet, the value needs to be divided by 43,560 to obtain the area in acres.  
The row is updated using the updateRow method on the row object.
```python
import arcpy
fc = "C:/Data/study.gdb/zones"
cursor = arcpy.da.UpdateCursor(fc, ["ACRES", "SHAPE_AREA"])
for row in cursor:
	row[0] = row[1] / 43560
	cursor.updateRow([row])
```

* The deleteRow method is used to delete the row at the current position of the UpdateCursor.  
After the cursor retrieves the row object, calling the deleteRow method deletes the row, as follows:
```python
import arcpy
fc = "C:/Data/study.gdb/roads "
cursor = arcpy.da.UpdateCursor(fc, ["STREETNAME"])
for row in cursor:
	if row[0] == "MAIN ST":
	cursor.deleteRow()
```

* A typical script that creates an insert or update cursor should therefore include two del statements -  
one to delete the row object, such as del r ow, and one to delete the cursor object, such as del cursor.   
For example:
```python
import arcpy
fc = "C:/Data/study.gdb/roads"
cursor = arcpy.da.UpdateCursor(fc, ["STREETNAME"])
for row in cursor:
	if row[0] == "MAIN ST":
	cursor.deleteRow()
del row
del cursor
```
```
>>> TIP
Forgetting to use the de1 statements at the end of a script can lead to errors so be sure to include the del statements after using insert and update cursor.
Search and update cursors suppor with statements, which guarantee closure and release of database locks.
Therefore, when a with statement is used, the del statements are not needed
```

## 7.3 Using SQL in Python  
* SQL queries can be carried out in Python using the SearchCursor function.  
The syntax for SearchCursor is:
```python
SearchCursor(in_table, field_names, {where_clause}, {spatial_reference}, {fields}, {explode_to_points})
```

* The following code shows an example of using an SQL expression:
```python
import arcpy
fc = "C:/Data/study.gdb/roads"
cursor = arcpy.da.SearchCursor(fc, ["NAME", "CLASSCODE"], '"CLASSCODE" = 1')
for row in cursor:
	print row[0]
del row
del cursor
```

* To avoid confusion and to ensure the field delimiters are the correct ones, you can use the AddFieldDelimiters function.  
The syntax of this function is
```python
AddFieldDelimiters(datasource, field)
```

* The function recognizes the type of dataset being used, such as a shapefile or personal geodatabase, and adds the correct de1imiters.  
In the following example code, the field name is first assigned to a variable and the AddFieldDelimiters function is used to add the correct delimiters to the field name prior to using it in an SQL expression:
```python
import arcpy
fc = "C:/Data/zipcodes.shp"
fieldname = "CITY"
delimfield = arcpy.AddFieldDelimiters(fc, fieldname)
cursor = arcpy.da.SearchCursor(fc, ["NAME", "CLASSCODE"], delimfield + " = 'LONGWOOD'")
for row in cursor:
	print row[0]
del row
del cursor
```

* SQL is commonly used in other functions as well.  
A number of bui1t-in tools use SQL-for example, the Select tool.  
The generic syntax of the Select tool is:  
```python
Select_analysis(in_features, out_feature_class, {where_clause})
```

* The where_clause parameter is an SQL expression, and to avoid confusion over the proper field delimiters, the following example code also uses the AddFieldDelimiters function:  
```python
import arcpy
infc = "C:/Data/zipcodes.shp"
fieldname = "CITY"
outfc  ="C:/Data/zip_select.shp"
delimfield = arcpy.AddFieldDelimiters(infc, fieldname)
arcpy.Select_analysis(infc, outfc, delimfield + " = 'LONGWOOD'")
```

* The ValidateTableName function can be used to determine whether a specific table name is valid for a specific workspace.  
The syntax of this function is:
```python
ValidateTableName(name, {workspace})
```
* For example, the following script determines whether all roads is a valid table name for the file geodatabase called study:
```python
import arcpy
tablename = arcpy.ValidateTableName("all roads", "C:/Data/study.gdb")
print tablename
```

* For example, the following code uses the Copy Features tool to copy all shapefiles from a folder to a geodatabase.  
The .shp file extension is removed from the file name using the basename property and the names are validated prior to copying the shape files to feature classes, as follows:
```python
import arcpy
import os
from arcpy import env
env.workspace = "C:/Data"
outworkspace = "C:/Data/test/study.gdb"
fclist = arcpy.ListFeatureClasses()
for shape in fclist:
	fcname = arcpy.Describe(shapefile).basename
	newfcname = arcpy.ValidateTableName(fcname)
	outfc = os.path.join(outworkspace, newfcname)
	arcpy.CopyFeatures_management(shapefile, outfc)
```

* A similar approach can be applied to field names using the ValidateFieldName function.  
Fields cannot be added unless their name is valid, so unless field names are validated first, a script may fail during execution.  
The syntax of this function is:  
```python
ValidateFieldName(name, {workspace})
```

## 7.4 Working with table and field names  
* The ValidateFieldName function takes a field name and a workspace and returns a valid field name for the workspace.  
Invalid characters are rep1aced by an underscore (_).  
The following code validates the name of a new field, and then uses the returned value to add the new field using the Add Field tool:  
```python
import arcpy
fc = "C:/Data/roads.shp"
fieldname = arcpy.ValidateFieldName("NEW%^")
arcpy.AddField_management(fc, fieldname, "TEXT", "", "", 12)
```

## 7.5 Parsing table and field names  
* The ParseTableName function can be used to split the fully qualified name for a dataset into its components.
The syntax of this function is:
```python
ParseTableName(name, {workspace})
```

* The ParseTableName function returns a single string with the database name, owner name, and table name, each separated by a comma (,).  
For example, the following code uses the ParseTableName function on a feature class to obtain the components of the fully qualified name, and then splits these components into a list:  
```python
import arcpy
from arcpy import env
env.workspace = "C:/Data/study.gdb"
fc = "roads"
fullname = arcpy.ParseTableName(fc)
namelist = fullname.split(", ")
databasename = namelist[0]
ownername = namelist[1]
fcname = namelist[2]
print databasename
print ownername
print fcname
```

* A similar approach can be used to split the fully qualified name of a field in a table into its components using the ParseFieldName function.  
This function returns a single string with the database name, owner name, table name, and field name, each separated by a comma (,).  

## 7.6 Working with text files  
* You can open files using the open function, which has the following syntax:  
```python
open(name, {mode}, {buffering})
```
* The only required argument is a file name and the function returns an object.  
For example, the following code opens an existing text file (.txt) on disk.  
```python
f = open("C:/Data/sample.txt")
```

* Using only a file name as a parameter returns a file object you can read from.  
If you want to do something else, such as write to the file, this must be stated explicitly by specifying a mode.  
The most common values for the mode are as follows:  
```
r: read mode
w: write mode
+: read/write mode (added to another mode)
b: binary mode (added to another mode - for example, rb, wb, and others)
a: append mode
```

* To create a new file object, you can use the open function and specify write mode:
```python
 f = open("C:/Data/mytext.txt", "w")
```
* Several file methods exist to manipulate the contents of a text file, including write, read, and close.  
Consider the following example:  
```python
f = open("C:/Data/mytext.txt", "w")
f.write("Geographic for at on Systems")
f.close()
```

* Reading a file works as follows:
```python
f = open("C:/Data/mytext.txt")
f.read()
```

*  An optional argument can be supplied to indicate the number of charac ters to be read.  
For example:
```python
f = open("C:/Data/mytext.txt")
f.read(3)
f.read(5)
f.read()
```

* After you start reading a file, you can use the seek method to set the file’s current position without opening the file again.  
For example:
```python
f.seek(0)
f.read(10)
```

* Reading the contents of this file can be accomplished in a number of ways, starting with the read method, as follows:  
```python
f = open("C:/Data/sqltext.txt")
f.read()
```

* Next is the readline method.  
For example:
```python
f = open("C:/Data/sqltext.txt")
f.readline()
f.readline()
f.readline()
```
* Finally, the readlines method reads all the lines in the files and returns the result as a list.  
For example:  
```python
f = open("C:/Data/sqltext.txt")
f.readlines()
```

* Writing a file with multiple lines can be accomplished using the write and writelines methods.   
To add new lines, the line separators have to be used.  
For example:
```python
f = open("C:/Data/tintext.txt", "w")
f.write("Triangulated\nlrregular\nNode")
f.close()
```

* The writelines method can be used to modify the string for a particular line.  
For example:
```python
f = open ("C:/Data/tintext.txt")
lines = f.readlines()
f.close ()
lines[2] = "Network"
f = open("C:/Data/tintext.txt", "w")
f.writelines(lines)
f.close()
```

* One of the most basic ways of iterating over the contents of a file is to use the read method in a while loop.  
For example, the following code loops over every character in the file:  
```python
f = open("C:/Data/mytext.txt")
char = f.read(1)
while char:
	<function>
	char = f.read(1)
f.close ()
```

* Another way to write the same iteration is as follows:  
```python
f = open("C:/Data/mytext.txt")
while True:
	char = f.read(1)
	if not char: break
	<function>
f.close()
```

* First, files in Python are iterable, which means you can use them directly in a for loop to iterate over their lines.
The code is as follows:
```python
f = open ("C:/Data/mytext.txt")
for line in f:
	<function>
	f.close()
```

* Iteration can also be accomplished using the readline method.   
For example:
```python
f = open("C:/Data/mytext.txt")
while True:
	line = f.readline()
	if not line: break
	<function>
f.close()
```

* For relatively small files, you can also read the entire file in step using the read method (to read the entire file as a string) or the readlines method (to read the file into a list of strings).  
For example:  
```python
f = open("C:/Data/mytext.txt")
for line in readlines():
	<function>
f.close()
```

* A second alternative is to use the fileinput module instead of the open function.   
This module enables you to create an object that you can iteI over in a for loop.  
For example:
```python
import leinput
for line in fileinput.input("C:/Data/mytext.txt")
	<function>
f.close()
```

* The following script opens an existing file in read mode and creates a new output file in write mode.  
A for loop is used to iterate over the lines in the input file.  
1n the block of code that follows, the replace method is used three times to remove specific strings from each line.  
The resulting string is written to the output file.  
The code is as follows:  
```python
input = open("C:/Data/coordinates.txt")
output = open("C:/Data/coordinates/clean.txt", "w")
for line in input:
	str = line.replace("ID: ", "")
	str = str.replace(", Latitude:", "")
	str = str.replace(", Longitude:", "")
	output.write(str)
input.close()
output.close()
```

## 7.7 Points to remember  
