# Chapter 4: Learning Python language fundamentals  

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Chapter 4: Learning Python language fundamentals](#chapter-4-learning-python-language-fundamentals)
	- [4.1 lntroduction](#41-lntroduction)
	- [4.2 Locating Python documentation and resources](#42-locating-python-documentation-and-resources)
	- [4.3 Working with data types and structures](#43-working-with-data-types-and-structures)
	- [4.4 Working with numbers](#44-working-with-numbers)
	- [4.5 Working w ith variables and naming](#45-working-w-ith-variables-and-naming)
	- [4.6 Writing statements and expressions](#46-writing-statements-and-expressions)
	- [4.7 Using strings](#47-using-strings)
	- [4.8 Using lists](#48-using-lists)
	- [4.9 Working with Python objects](#49-working-with-python-objects)
	- [4.10 Using functions](#410-using-functions)
	- [4.11 Using methods](#411-using-methods)
	- [4.12 Working with strings](#412-working-with-strings)
	- [4.13 Working with lists](#413-working-with-lists)
	- [4.14 Working with paths](#414-working-with-paths)
	- [4.15 Working with modules](#415-working-with-modules)
	- [4.16 Controlling workf1 ow using conditional statements](#416-controlling-workf1-ow-using-conditional-statements)
	- [4.17 Controlling workflow using loop structures](#417-controlling-workflow-using-loop-structures)
	- [4.18 Getting user input](#418-getting-user-input)
	- [4.19 Commenting scripts](#419-commenting-scripts)
	- [4.20 Working with code in the PythonWin editor](#420-working-with-code-in-the-pythonwin-editor)
	- [4.21 Following coding guidelines](#421-following-coding-guidelines)
	- [Points to remember](#points-to-remember)

<!-- tocstop -->

---
## 4.1 lntroduction  

## 4.2 Locating Python documentation and resources  
```
All Programs > ArcGIS > Python2.7 > Python Manuals
```
* http://docs.python.org
* http://www.python.org
* https://wiki.python.org/moin/BeginnersGuide
* https://wiki.python.org/moin/BeginnersGuide/NonProgrammers

## 4.3 Working with data types and structures  

## 4.4 Working with numbers  
```python
3 * 8  
16 / 4
17 % 4
7 / 3
7 / 3.0
```

## 4.5 Working w ith variables and naming  
* Style Guide for Python Code http://www.python.org/dev/peps/pep-0008/

```python
x = 17
x * 2
# ---
x, y, z = 1, 2, 3
```

## 4.6 Writing statements and expressions
```python
x = 2 * 17
print x
```

## 4.7 Using strings  
```python
print "I said: 'Let's go!'"
```
```python
x = "G"
y = "I"
z = "S"
print x + y + z
```
```python
x = "Geographic"
y = "Information"
z = "Systems"
print x + " " + y + " " + z
```
```python
# wrong
temp = 32
print "The temperature is " + temp + " degrees"
# correct
temp = 32
print "The temperature is " + str(temp) + " degrees"
```

## 4.8 Using lists  
```
mylist = [1, 2, 4, 8, 16 , 32 , 64]
```
```
mywords = ["jpg", "bmp", "tif", "img"]
print mywords
```
## 4.9 Working with Python objects  
```python
name = "Paul"
name
id(name)
type(name)
```
```python
var1 = 100
type(var1)
```
```python
var2 = 2.0
type(var2)
```

## 4.10 Using functions  
```python
pow(2,3)
print dir(__builtins__)
print pow.__doc__
```

## 4.11 Using methods  
```python
topic = "Geographic Information Systems"
topic.count("i")
```

## 4.12 Working with strings  
```python
mytext = "GIS is cool"
print mytext.lower()
print mytext.upper()
print mytext.title()
```
```python
mystring = "Geographic Information Systems"
mystring[0]
mystring[23]
mystring[-1]
mystring[11:22]
mystring[11:]
mystring.find["Info"]
mystring.find["info"]
"Info" in mystring
```

* join  

```python
list_gis = ["Geographic" , "Information", "Systems"]
string_gis = " "
string_gis.join(list_gis)
```

* split

```python
pythonstring = "Geoprocessing using Python scripts"
pythonlist = pythonstring.split(" ")
pythonlist
```

* strip, rstrip, lstrip

```
# allows you to remove any combination of characters in any order from the ends of an existing string
mytext = "Commenting scripts is good"
mytext.strip("Cdo")
mytext.rstrip("Cdo")
mytext.lstrip("Cdo")
```

* replace

```
mygis = "Geographic Information Systems"
mygis.replace("Systems", "Science")
```
```
myfile = "streams.shp"
myfile.replace(".shp", "")
```
```
temp = 100
print "The temperature is {0} degrees".format(temp)
```
```
username = "Paul"
password = "surf$&9*"
print "{0}'s password is {1}".format(username, password)
```

## 4.13 Working with lists  
```
cities = ["Austin", "Baltimore", "Cleveland", "Denver", "Eugene"]
```
```
print len(cities)
```
```
cities.sort(reverse = True)
print cities
```
```
cities.sort
print cities
```
```
cities[1]
cities[-1]
cities[-2]
cities[2:4]
cities[2:]
cities[:2]
```
```
"Baltimore" in cities
"Seattle" in cities
```

* del

```
cities = ["Austin", "Baltimore", "Cleveland", "Denver", "Eugene"]
del cities[2]
cities
```

* append

```
cities = ["Austin", "Baltimore"]
cities.append("Cleveland")
cities
```

* count

```
yesno = ["True", "True", "False", "True", "False"]
yesno.count("True")
```

* extend

```
list1 = [1, 2, 3, 4]
list2 = [11, 12, 13, 14]
list1.extend(list2)
list1
```

* index

```
mylist = ["The", "quick", "fox", "jumps", "over", "the", "lazy", "dog"]
mylist.index("the")
```

* insert

```
cities = ["Austin", "Cleveland", "Denver", "Eugene"]
cities.insert(1, "Baltimore")
cities
```

* pop

```
cities = ["Austin", "Baltimore", "Cleveland", "Denver", "Eugene"]
cities.pop(3)
```

* remove

```
numbers = [1, 0, 1, 0, 1, 0, 1, 0, 1, 0]
numbers.remove(0)
numbers
```

## 4.14 Working with paths  
```
1. Use a forw slash (/) - for example,
"C:/EsriPress/Python/Data".

2. Use two backslashes (\\) - for example,
"C:\\EsriPress\\Python\\Data".

3. Use a string literal by placing the letter r before a string - for example,
r"C:\EsriPress\Python\Data".
The letter r stands for "raw string," which means that a backslash will not be read as an escape character
```

## 4.15 Working with modules  
```
import math
math.cos(1)

print math.cos.__doc__
dir(math)

from math import cos
cos(1)

import itme
print dir(time)

time.time()

time.localtime()

time.asctime()

import keyword
print keyword.kwlist
```

## 4.16 Controlling workf1 ow using conditional statements  
```python
import random
x = random.randint(0, 6)
print x
if x == 6:
	print "You win!"
```
```
== Eqaual to
!= Not equal to
```

## 4.17 Controlling workflow using loop structures  
```python
import random
x = random.randint(0, 6)
print x
if x == 6:
	print "You win!"
elif x == 5:
	print "Try again!"
else:
	print "You lose!"		
```

```python
# infinite loop
i = 0
while i <= 10:
	print i
```

```python
i = 12
while i <= 10:
	print i
	i += 1
```

```python
mylist = ["A", "B", "C", "D"]
for letter in mylist:
	print letter
```

```python
import sys
x = int(sys.argv[1])
y = int(sys.argv[2])
print x * y
```

## 4.18 Getting user input  

## 4.19 Commenting scripts  

## 4.20 Working with code in the PythonWin editor  

## 4.21 Following coding guidelines  

## Points to remember  
