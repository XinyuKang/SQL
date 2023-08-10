# Some Python Notes

## [Decorators in Python](https://www.geeksforgeeks.org/decorators-in-python/)
Decorators allow us to wrap another function in order to extend the behaviour of the wrapped function, without permanently modifying it.

### Function is First Class Object in Python
Functions in Python can be used or passed as arguments because:
- a function is an instance of the Object type.
- You can store the function in a variable.
- You can pass the function as a parameter to another function.
- You can return the function from a function.
- You can store them in data structures such as hash tables, lists, …

#### Example 1: treating the function as objects
```
def shout(text):
    return text.upper()
 
print(shout('Hello'))

# takes the function object referenced by a shout and creates a second name pointing to it, yell
yell = shout
 
print(yell('Hello'))
```
#### Example 2: Passing the function as an argument
```
# Python program to illustrate functions
# can be passed as arguments to other functions
def shout(text):
	return text.upper()

def whisper(text):
	return text.lower()

def greet(func):
	# storing the function in a variable
	greeting = func("""Hi, I am created by a function passed as an argument.""")
	print (greeting)

greet(shout)
greet(whisper)
```

#### Example 3: Returning functions from another function
```
# Python program to illustrate functions
# Functions can return another function

def create_adder(x):
	def adder(y):
		return x+y

	return adder

add_15 = create_adder(15)

print(add_15(10))
```

### Decorators
In Decorators, functions are taken as the argument into another function and then called inside the wrapper function.
```
@gfg_decorator
def hello_decorator():
    print("Gfg")
```
is equivalent to
```
def hello_decorator():
    print("Gfg")
hello_decorator = gfg_decorator(hello_decorator)
```
#### Example of the decorated function has arguments and return values
```
def hello_decorator(func):
	def inner1(*args, **kwargs):
		
		print("before Execution")
		
		# getting the returned value
		returned_value = func(*args, **kwargs)
		print("after Execution")
		
		# returning the value to the original frame
		return returned_value
		
	return inner1


# adding decorator to the function
@hello_decorator
def sum_two_numbers(a, b):
	print("Inside the function")
	return a + b

a, b = 1, 2

# getting the value through return of the function
print("Sum =", sum_two_numbers(a, b))
```

#### Example of chaining decorators
```
# code for testing decorator chaining
def decor1(func):
	def inner():
		x = func()
		return x * x
	return inner

def decor(func):
	def inner():
		x = func()
		return 2 * x
	return inner

@decor1
@decor
def num():
	return 10

@decor
@decor1
def num2():
	return 10

print(num())  # similar to decor1(decor(num))
print(num2()) # similar to decor(decor1(num2))
```

## [*args and **kwargs in Python](https://www.geeksforgeeks.org/args-kwargs-python/)
```
def myFun(arg1, arg2, arg3):
	print("arg1:", arg1)
	print("arg2:", arg2)
	print("arg3:", arg3)


# Now we can use *args or **kwargs to
# pass arguments to this function :
args = ("Geeks", "for", "Geeks") # tuple or list or anything iterable
myFun(*args)

kwargs = {"arg1": "Geeks", "arg2": "for", "arg3": "Geeks"}
myFun(**kwargs)
```

### *args
Used to pass `non-keyworded arguments`. 
Using the *, the variable that we associate with the * becomes `iterable` meaning you can do things like iterate over it
```
def myFun(arg1, *argv):
	print("First argument :", arg1)
	for arg in argv:
		print("Next argument through *argv :", arg)

myFun('Hello', 'Welcome', 'to', 'GeeksforGeeks')
```

### **kwargs
The double star allows us to pass through `keyword arguments` (and any number of them).
One can think of the kwargs as being a `dictionary` that maps each keyword to the value that we pass alongside it. 
That is why when we iterate over the kwargs there `doesn’t seem to be any order` in which they were printed out.
```
def myFun(arg1, **kwargs):
	for key, value in kwargs.items():
		print("%s == %s" % (key, value))

# Driver code
myFun("Hi", first='Geeks', mid='for', last='Geeks')

```
