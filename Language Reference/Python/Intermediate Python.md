## Chapter 2: Cross-Cutting Tools
### Logging
- ERROR
- WARN
- DEBUG
- INFO

```python
  import logging
  logging.basicConfig(level=logging.INFO, filename = 'sqrt.log')
  # logging.error() or logging.warning() or logging.info() or logging.debug()
  # if filename param is specified, then the logging will not be printed to the console, but it will be saved to the file
```

### Test Driven Development (TDD)

Dev/QA/Prod

![[Pasted image 20241004101611.png]]

#### Testing Examples

name tests as "test_xxxx.py"

```python
import basicmath import div # import div function from the basicmath module
# 'pip install pytest' unless using anoconda
def test_div():
	assert div(4,2) == 2

def test_div_2():
	assert div(15,3) == 5
```

## Chapter 3: Intermediate Programming Concepts
### List Mutability and Deep Copying
> [!note]
> see [pythontutor.com]() for helpful tools for visualizing python code

```python
mammals = ['cat', 'kangaroo', 'horse']
fish = ['tuna', 'shark', 'catfish']

animals = [mammals, fish]

mammals[0] = 'mule' # this changes the first element in the mammals list, and also the element of the mammals list in the animals list

# But when we slice a list, we create a seperate copy

# when we can also copy pointers, and so when we create a copy of an entire list, we are create a new set of pointers that point to the saw data in memory. this is a 'shallow'

# we want a deep copy
import copy
students ['student1','student2', 'student3']
students_copy = copy.deepcopy(students) # this creates seperate memory location for students_copy
```

> [!note]
> Lists are passed into functions as pointers
tuples allow lists to be passed to functions without allowing the function to modify the list

### Generator and Memory Efficiency

maintain function state between calls

ex. separating loading of a list from the processing of that list

```python
for i in [1,2,3,4,5]:
	print i

for i in range(5):
	print i

def myrage(n):
	x = 0 # initialize internal state
	while x < n:
		yield x # return value but maintain state of function, it turns the function into a generator
		
def countdown(n):
	while n > 0:
		print("Computing next number...")
		yield n
		n -= 1
for i in countdown(5):
	print(i)

v = countdown(5)
next(v)

# infinite lists
def random_num_inf(low, high)
	while True: 
		yield: random.randrange(0, 180)
r = ramdon_gen_inf(0, 100)
next(r) # you can call this function infinitely and you will always get another random number
```

## Generator Case Study

```python
%time v = [ i**2 for i in range(1000000) ] # this takes seconds to execute, the entire list is loaded into memory

%time v = ( i**2 for i in range(1000000) ) # this takes micro seconds to execute
next(g) # this only computes the next element as we need it
```

```python
wwwlow = open("access-log")
total = 0
for line in wwwlog
	bytestr = line.rsplit(None,1)[1]
	if bytestr = != '-':
		total += int(bytestr)

wwwlog = open("access-log")
bytecolumn = (line.rsplit(None,1)[1] for line in wwwlog)
bytes = (int(x) for x ihn bytecolumn if x != '-')

print("Total", sum(bytes))
```

```python
import time
def follow(thefile):
	thefile.seek(0,2)
	while True:
		line = thefile.readline()
		if no line:
			time.sleep(0.1)
			contiune
		yield line

logfile = open("test-log")
loglines = follow(logfile)
type(loglines) # = generator
```

### Higher Order Functions

```python
def square(x):
	return = x*x

square(6) # = 36
f = square
f(6) # = 36
```

Simple example

```python
def summation(low, high):
	total = 0
	for i in range(low, high+1):
		val = i ** 2
		total += val
	return total

def square(x):
	return = x*x

def cube(x):
	return = x*x*x

def custom_summation(low, high, fn):
		total = 0
	for i in range(low, high+1):
		val = fn(i) # call the function that was passed in on the variable i
		total += val
	return total

custom_summation(1, 5, square)
custom_summation(1, 5, lambda i : i**2) # lamda is an anonymous function
```

### Callbacks

a callback is a function pointer: we pass into a function and it gets called later

## Chapter 4: Time Saving Features
### Decorators

```python
def fib(n):
	if n <= 1:
		return n
		
	else:
		return fib(n-2) + fib(n-1)

# if we call:
%time fib(35)
# this will take a lot of time (6.5s)
# it is very recursive: each previous fib requires the two previous fibs before that
```

```python
def logger(f): # logger is a higher order function because it is passed a funciton, f

	# f will be remembered by thre wrapper even after we exit loggoer
	# This is the concept of a closure

	# we define a new function at runtime:
	def wrapper(n):
		print("I'm going to call a function")
		v = f(n)
		print("The function returned: ", v)
		return v

	return wrapper
logger_fib = logger(fib)
logger_fin(3)
```

### Decorator Case Study

```python
def memoize(f):
	mem = {} 

	def memoized_function(n):
		if n not in mem:
			mem[n] = f(n)
		return mem[x]
		
	return memoize

fib = memoize(fib)
%time fib(36)
```

```pyhthon
@memoize # this is called a "decorator", equals fib = memoize(fib)
```

### Context Managers

```python
f = open('dummy-file', 'r')
for line in f:
	prine(line)

f.close() # people will always forget this

with open('dummy-file.txt', 'r') as r:
	for line in f:
		print(line)
```

```python
start_time = int(round(time.time() * 1000))

some_function()

end_time = int(round(time.time() * 1000))
elpased = end_time - start_time
print("Code took %d ms to run.", % elapsed)
# this is simple but ugly

from contextlib import contextmanager

@contextmanager
def timeit():
	start_time = int(round(time.time() * 1000))
	yield
	end_time = int(round(time.time()  * 1000))
	print("Code took %d ms to run.", % elapsed)	

with timeit():
	somefunction()
```

## More on Case Managers

```python
import tempfile
import shutil
import os

try: 
	name = tempfile.mkdtemp()
	print("Created temp dir: %s" % name)
	filename = os.path.join(name, "somefile.txt")
	# do some processing
	with open(filename, 'w') as f:
		print("opended file: %s" % filename)
		f.write("Dummy text")

finally:
	print("Deleting directory: %s" % name)
	shutilrmtree(name)
```

```python
@contextmanager
def tempfile:
	try: 
		name = tempfile.mkdtemp()
		print("Created temp dir: %s" % name)
		filename = os.path.join(name, "somefile.txt")
		# do some processing
		with open(filename, 'w') as f:
			print("opended file: %s" % filename)
			yield # allow custom function to be executed
	
	finally:
		print("Deleting directory: %s" % name)
		shutilrmtree(name)

with tempfile:
	f.write("dummy text")
```

## Chapter 5: Parallel and Asynchronous Programming
### Multithreading

```python
import requests # allow 
import threading

with timeit():
	threads = []
	for f in gists:
		# create workers
		t = threading.Thread(target = get_gist, args=(g,))
		t.start() # start the threads
		threads.append(t) # record threads in a list

	for t in threads:
		t.join() # wait until threads finished
		
	print("All done")
```

### Synchronization Issues and Locks

we need to lock critical sections of code

```python
lock = threading.Lock()
def f()
	global counter

	for i in range(iterations_in_one_thread):
		lock.acquire()
		v = counter
		time.sleep(0.00000000000001)
		v =+ 1
		counter = v
		lock.release()
# problems
# 1) need to know critical sections
# 2) people forget to relase lock
# it's much more common for us to know sections we want to multithread, rather than the sections we want to isolate
```

### Asynchronous Programming

```python
import asyncio
import aiohttp

async def get_gist(g)
	print('GET: ', url)
	async with aiohttp.ClientSession() as session:
		async with session.get(url) as response:
			page_test = await response.text() # the slow line of code
			g_length = len(page_text)
			print("Len: %d" % g_length)

asyncio.set_event_loop(asynicio.new_event_loop())
loop = asyncio.get_event_loop()

tasks = []
for g in gists:
	future = asyncio.ensure_future( get_gist(g) )
	tasks.append(future)

loop.run_until_complete(asyncio.wait(tasks))
loop.close() # house keeping
tasks[0].result() # we can get return values
```

## Chapter 6: Functional Programming
### Basics of Functional Programming - Map

a different paradigm

treat all computations as mathematical functions - no side effects

benefits:

- easier to predict inputs and outputs
- easier to debug
- easier to parallelize

Maps

```python
[ sqrt(i) for i in [1, 4, 9, 16] ]

x = map(sqrt, [1, 4, 9, 16] )
x = list(x)

list ( map(lamda k: 2**k, [ 1, 2, 3, 4 ]) )
```

### Filter and Reduce

```python
# filter
x = filter(str.isaplpha, ['x', 'y', '2', '3', 'a'])
list(x) # x,y,a

# reduce
reduce( add, [1, 10, 4], 0) # 0 + 1 = 1, 1 + 10 = 11, 11 + 4 = 15

```

## Chapter 7: Applications
### Plotting Intro

```python
import numpy as np # by convention 
X = np.linspace(0, 10, 40) # a numpy.ndarray of 0 to 10 in 40 steps
type(X)
C = np.cos(X) # compute cos for each value in the vector

import matplotlib.pyplot as plt
_ = plt.plot(X, V) # X = x axis, V = y axis, notice the underscore, we don't care about using the variable later
S = np.sin(X) # compute sin for a vector
plt.figure() # create a figure to add plots to
plot.plot(X, C, label='cos')
plot.plot(X, S, label='sin')
plot.xlabel("x")
plot.ylabel("sin(x)/cos(x)")
plot.legend()
```

### Pattern Matching with Regular Expressions

```python
text = "Date: 03-02-2025"
needle = "Da"
import re # regular expression package
match = re.match(needle, haystack)
match.group(0)
# . = any character
# /d = number
# /s = space
needle = "....../d/d" # six spaces and then two numbers
needle = ".{5}/s/d{2}" # brackets mean we have multiple instances of the same character
needle = ".*/d{2}" # anything any number of times and then two digits
needle = ".*?\d{2}" # anything character can occur any number of times before two digits, but stop as soon as condition is satisfied
```

### Modular Regular Expressions

```python
# we can break up one expression into pieces
str_date = ".*?"
str_day = "\d{2}"

needle = str_date + str_day # this much easier to understand!

str_time = "\d{2}:\d{2}[apAP][mM]"

str_prefix = ".*"
str_username = "[a-zA-Z0-9.]*"
str_domain = ".*\..*?" # any number of characters, dot, any number of character
str_email = str_username + "@" + str_domain

needle = str_prefix + "\s" + str_email + "\s"
```

### Extracting Matched Strings with Regular Expressions

```python
# placing parenthesis around the expression that we want to extract
# it gets added to the .group() var
```