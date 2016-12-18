# Chapter 12: Creating Python functions and classes  

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 12: Creating Python functions and classes](#chapter-12-creating-python-functions-and-classes)
	- [12.1 Introduction](#121-introduction)
	- [12.2 Creating functions](#122-creating-functions)
	- [12.3 Calling functions from other scripts](#123-calling-functions-from-other-scripts)
	- [12.4 Organizing code into modules](#124-organizing-code-into-modules)
	- [12.5 Using classes](#125-using-classes)
	- [12.6 Working with packages](#126-working-with-packages)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->


---
## 12.1 Introduction  

## 12.2 Creating functions  
* The following code generates a random number between 1 and 100:  
```python
import random
x = random.randint(1, 100)
print x
```

* Python functions are de fined using the def statement, as shown in the figure.  
The def statement contains the name of the function, followed by any arguments in parens.  
The syntax of the def statement is.  
```python
def <functionname>(<arguments>):
```

* There is a colon(:) at the end of the statement, and the code following a def statement is indented the same as any block of code.   
This indented block of code is the function definition.  
For example, consider the script helloworld.py as follows:  
```python
def printmessage():
	print "Hello world"
```

* In this example, the function printmessage has no parameters, but most functions use parameters to pass values.  
Elsewhere in the same script, you can call this function directly, as follows:  
```python
printmessage()
```

* Typically, functions are quite a bit more elaborate.  
Consider the following example: You want to create a list of the names of all the fields in a table or a feature class.  
There is no function in ArcPy that does this.  
However, the ListFields function allows you to create a list of the fields in a table, and you can then use a for loop to iterate over the items in the list to get the names of the fields.  
The list of names can be stored in a list object.  
The code is as follows:  
```python
import arcpy
arcpy.env.workspace = "C:/Data"
fields = arcpy.ListFields("streams.shp")
namelist = []
for field in fields
	namelist.append(field.name)
```

* Now, say you anticipate that you will be using these lines of code quite often-in the same script or in other scripts.  
You can simply copy the lines of code, paste them where they are needed, and make any necessary changes.  
For example, it is likely you will need to replace the parameter "streams.shp" with the feature class or table of interest.  

* Instead of copying and pasting code, you can define a custom function to carry out the same steps.  
First, you need to give the function a name - for example, listfieldnames.  
The following code defines the function:  
```python
def listfieldnames():
```

* You can now call the function from elsewhere in the script by name.  
In this example, when calling the function, you want to pass a value to the function-that the name of a table or a feature class.  
To make this possible, the function needs to include a parameter to receive these values.  
The parameter needs to be included in the definition of the function, as follows:  
```python
def listfieldnames(table):
```

* Following the def statement is an indented block of code that contains what the function actually does.  
This is identical to the previous lines of code, but now the hard-coded value of the feature class is replaced by the parameter of the function:  
```python
def listfieldnames(table):
	fields = arcpy.ListFields(table)
	namelist = []
	for field in fields:
		namelist.append(field.name)
```

* The last thing needed is a way for the function to pass values, also referred to as returning values.  
This is necessary to ensure that the function not only creates the list of names, but also returns the list so it can be used by any code that calls the function.  
This is accomplished using a return statement.  
The completed description of the function is as follows:  
```python
def listfieldnames(table):
	fields = arcpy.ListFields(table)
	namelist = []
	for field in fields:
		namelist.append(field.name)
	return namelist		
```

* Once a function is defined, it can be called directly from within the same script, as follows:
```python
fieldnames = listfieldnames("C:/Data/hospitals.shp")
```

## 12.3 Calling functions from other scripts  
* Once functions are created in a script, they can be called from another script by importing the script that contains the function.  
For relatively complex functions, it is worthwhile to consider making them into separate scripts or script tools, especially if they are needed on a regular basis.  
So rather than defining a function within a script, the function becomes a script in itself that can be called from other scripts.  
Consider the earlier example of the helloworld.py script:  
```python
def printmessage():
	print "Hello World"
```

* The printmessage function can be called from another script by importing the helloworld.py script.  
For example, the script print.py does it, as follows:  
```python
import sys
import hospitals
import helloworld
helloworld.printmessage()
```

* The script print.py imports the helloworld module.  
A module name is equal to the name of the script minus the .py extension.  
The function is called using the regular syntax to call a function-that is, <module>.<function>  


* The first place Python looks for modules is the current folder, which is the folder where the print.py script is located.  
The current folder can be obtained using the following code, where sys.path is a list of system paths:  
```python
import sys
print sys.path[0]
```

* The current folder can also be obtained using the os module, as follows:  
```python
import os
print os.getcwd()
```

* To view a complete list of these paths, use the following code:  
```python
import sys
print sys.path
```

* In a typical scenario, the list will include paths to both the Python installation and the ArcGIS installation.  
The list will include paths like the following:  
```
C:\Python27\ArcGIS10.1
C:\Python27\ArcGIS10.1\Lib
C:\Python27\ArcGIS10.1\Lib\site-packages
C:\program les\ArcGIS\Desktop10.1\bin
C:\program les\ArcGIS\Desktop10.1\arcpy
C:\program Files\ArcGIS\Desktop10.1\ArcToolbox\Scripts
```

* What if the module you want to import is in a different folder-that is, not in the current folder of the script or in any of the folders in sys.path?  
You have two options, as follows:  
	1. Use a path configuration file (.pth).
	2. Append the path using code.

## 12.4 Organizing code into modules  
* By creating a script that defines a custom function, you are turning the script into a module.  
All Python script files are, in fact, modules.  
That's why you can call the function by first importing the script (module), and then using a statement such as <module>.<function>.
Recall the example:
```python
import random
x = random.randint(1, 100)
print x
```

* The random module consists of the random.py file and is located in one of the folders that Python automatically recognizes, C:\Python27\ArcGIS10.1\lib The random.py script (module) contains a number of functions, including randint.  

* Consider the example hello.py script, which contains a function as well as some test code to make sure the function works:  
```python
def printmessage():
	print "Hello world"
print message()
```

* When you import the script file as a module, you don't want the test code to run automatically, but only when you call the specific function.  
You want to be able to differentiate between running the script by itself and importing it as a module into another script.  
This is where the variable __name__ comes in (there are two underscores on each side).  
For a script, the variable has the value of "__main__".  
For an imported module, the variable is set to the name of the module.  
Using an if statement in the script that contains the function will make it possible to distinguish between a script and a module, as follows:  
```python
def printmessage():
	print 'Hello world'
if __name__ == '__main__':
	printmessage()
```

* In this case, the test of the module will be run only if the script is run by itself.  
If you import the script, no code will be run until you call the function.  

* This structure is not limited to testing.  
In some geoprocessing scripts, almost the entire script consists of one function or more, and only the very last lines of code actually call the function if, indeed, the script is run by itself.  
The structure is as follows:  
```python
import arcpy
import os
def mycooltool(<arguments>) :
	<line of code>
	<line of code>
	...
if __name__ == '__main__':
	mycooltool(<arguments>)
```

* This structure provides control of the running of the script and makes it possible to use a script in two different ways - running it by itself or calling it from another script.  

## 12.5 Using classes  
* To make a class in Python, you use the keyword class.  
Take a look at a simple example:  
```python
class Person(object):
	def setname(self, name):
		self.name = name
	def greeting(self):
		print "My name is (0).".format(self.name)
```

* A class can be thought of as a blueprint.  
It describes how to make some- thing and you can create many instances from this blueprint.  
Each object created from a class is called an instance of the class.  
Creating an instance of a class is sometimes referred to as instantiating the class.  

* Next, you will see how this class can be used.  
```python
me = Person()
```

* Using an assignment statement creates an instance of the Person class.  
Creating this instance looks like calling a function.  
Once an instance is created, you can use the properties and methods of the class, as follows:  
```python
me .setname("Abraham Lincoln")
me.greeting()
```

* Running this code prints the following:
```python
My name is Abraham Linncoln.
```

* You want to create a class called parcel that has two properties (land-use type and total assessed value) and a procedure (calculating tax) associated with it.  
For the purpose of this example, assume the property tax is calculated as follows:  
	* For single-family residential, tax = 0.05 * value
	* For multifamily residential, tax = 0.04 * value
	* For all other land uses, tax = 0 .02 * value.

* In this example, the script containing the class is called parceJclass.py and is as follows:  
```python
class Parcel(object):
	def init(self, landuse, va1ue):
		self.landuse = landuse
		self.va1ue = va1ue
def assessment(self) :
	if self.landuse == "SFR":
		rate = 0.05
	elif self.landuse == "MFR":
		rate = 0.04
	else:
		rate = 0.02
	assessment = self.va1ue * rate
	return assessment
```

* In this example, the script that uses the class is called parceltax.py and is as follows.  
```python
import parcelclass
myparcel = parcelclass.parcel("SFR", 200000)
print "Land use:", myparcel.landuse
mytax = myparcel.assessment()
print mytax
```

## 12.6 Working with packages  

## Points to remember  
