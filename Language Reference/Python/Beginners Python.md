## Chapter 1
### First Program - Hello World!

```python
print ("Hello World!")
```

### Comments and Clearing Screen

```python
# Clear the Screen
import os
os.system('cls')

# This is a comment
print ("Hello World!")
```

### Variables

```python
# variable names must not contain spaces
# python people tend to use underscores rather than camel case
first_name = "John"
print (first_name) # Print the string variable
```

### Intro to Data Types

5 main ones

### Strings

```pythong
name = "John"
name = 'John'
```

#### Numbers

```python
age = 0
```

#### Lists

```python
first_name = ["John", "Bob"]
```

#### Tuples

```python
# Tuples are immutable lists
first_name = ("John", "Bob") 
```

#### Dictionary

```python
fav_pizza = {
	"John": "Pepperoni",
	"Bob": "Cheese",
	"Tina": "Supreme"
}
print (fav_pizza["John"]) # return the value for the key of John
```

### Strings

```python
talking = "My mom yelled "Clear your room"" # this will return an error
talking = 'My mom yelled "Clean your room"' # this will work
talking = "My mom yelled 'Clean your room'" # this will work
talking = "My mom yelled \"Clean your room\"" # this will work because of the escape characters
# \n is a new line character
```

### String Manipulation

```python
my_string = "my name is John Elder"

print (my_string.upper()) # this capitalizes all character in the string
# other string methods
# .lower() - makes every character lowercase
# .capitalize() - capitalizes first word
# .title() - capitalized everyone
# .swapcase() - swaps cases of each character
print (len(my_string)) # return number of characters in the string
print (my_string[0:7]) # return "my name"
# .split creates a list by breaking up the string based on the character you pass into it
```

### Math with Python

```python
print (2 + 3) # addition
print (2 - 3) # subtraction
print (2 * 3) # multiplication
print (2 / 2) # division
print (2 ** 2) # exponent
print (10 % 3) # returns the remainder, so 1 in this case
```

### Floats and Ints

```python
x = 3
int(x) # this forces x to be an integer
round(10/3, 4) # this returns 3.3333
```

### Assignment Operators

```python
# Assignment operators
num = 4 # this makes num 4
num += 1 # this adds 1 to num
num -= 2 # this substracts 1 from num
# other assignment operators: *=, /=, %=
```

### Lists

```python
names = ["John", "Bob", "Mary"]
# lists can be of any data types and they can be mixed
# you can have lists of lists
del names[0] # this deletes "John" from the list
names.append("Tina") # add Tina to the end of the list
```

### Tuples

```python
# tuples are faster to read/write at the processors level
# you can't add to tuples, but you can create new tuples from others
tuple_1 = (1, 2, 3)
tuple_2 = (4,)
tuple_3 = tuple_1 + tuple_2 # this returns (1, 2, 3, 4)
```

### Dictionary

```python
fav_pizza = {
	"John": "Pepperoni",
	"Bob": "Cheese",
	"Tina": "Supreme"
}
del fav_pizza["John"] # you can delete entires in a dictionary
fav_pizza["John"] = "Ham" # you can modify entries
fav_pizza.update({"Tim":"Green Peppers"}) # you can also add entries
```

## Chapter 2
### Comparison Operators

```python
# ==, !=, >, =>, <, =<
# string comparisons are case sensitive
```

### Conditional Statements

```python
# IF
num = 20
if (num > 10):
	print ("Your number is greater than 10")
# ELSE IF
elif (num = 3):
	print ("Your number is 30")
# ELSE
else:
	print ("Your number is not greater than 10)
```

### Multiple Conditional Statements

```python
# and, or
num = 20
if (num > 10) and (num < 100):
	print ("Your number is greater than 10 and less than 100")
```

### While Loops

```python
counter = 0
while (counter < 10):
	print ("The count is: %s" % counter)
	counter += 1
```

### For Loops

```python
name = "John"
# or name = ["John", "Bob", "Mary"]
for x in name:
	print (x)

fav_pizza = {
	"John": "Pepperoni",
	"Bob": "Cheese",
	"Tina": "Supreme"
}
for key, value in fav_pizza.items():
	print(value)
```

### FizzBuzz

```python
counter = 0
while (counter <= 100):
	if ((counter % 3 == 0) and (counter % 5 ==0):
		print ("%s - FizzBuzz" % count)

	elif (count % 3 == 0):
		print ("%s - Fizz" % count)

	elif (count % 5 == 0):
		print ("%s - Buzz" % count)

	else:
		print (count)
	counter += 1
```

## Chapter 3
### Functions

```python
def namer(first_name, last_name):
	print ("Hello %s" % first_name)
	print ("Hello %s" % last_name)

name("John", "Elder")
```

### More Functions

```python
def namer(name)
	return ("Hello %s" % name)

my_namer = namer("John")

print (my_namer)
```

### Modules

```python
# see the python module index for all of the built-in modules
# adding the below to a file namer_module.py
def namer(name)
	return ("Hello %s" % name)

# we can then call that function from a different script
import namer_module
print (namer_module.namer)
```

### Intro to Classes

Everything in python is an object

```python
# two parts defintion and instantiation

class Square:
	def __init__(self, side_length)
		self.side_length = side_length
	
	def area(self):
		return self.side_length * self.side_length

	def perimeter(self):
		return self.side_length * 4

	def report(self):
		print ("Side Length: %s" % self.side_length)
		print ("Area: %s" % self.area)
		print ("Perimeter: %s" % self.perimeter)

my_square = Square(10) # pass in a side length of 10
print (mySquare.side_length) # returns 10
print (mySquare.area())
mySquare.report()
```