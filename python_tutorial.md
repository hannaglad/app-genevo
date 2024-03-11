# Python basics crash-course 

The point of this document is to give you a **very** brief overview of the most imporant concepts in python (and programming in general). It is very, very far from being a comprehensive tutorial. 

The goal is mostly to give you some of the vocabulary that will help you to look things up when you run into problems. 

We will be diving into some pretty abstract coding in this project, so I thought it would be nice to have the base vocabulary all in one place as a kind of mini-reference, especially if you have never coded in python before. 

## Basic operators

### Math

* `+` : Addition
* `-` : Subtraction:
* `*` : Multiplicaton: 
* `/` : Division 
* `//` :Floor division (returns the lowest, rounded whole number of a division)
* `%` : Modulo (returns the remainder of a division)
* `**` : To the power of, eg : `3**2 = 9` 


>`**` is **NOT** to be confused with `^`, which performs a bitwise `XOR` (for those who are familiar). It will not give intended results ;)

### Comparisons

* `>` : greater than
* `<` : less than
* `> =` : greater or equal than
* `< =` : less or equal than
* `= =` : equal
* `! =` : not equal 

>Note: the double don't actually have a space inbetween them. It's just that otherwise markdown shows it like this : `<=`
 

### Logical operators

* `and` : if both conditions are true
* `or` : if either one of the conditions are true 
* `not` : if neither conditions are true

 ---


## Variables, Data Structures, and Classes

As in any programming language, a *variable* is a reference to some value (or *object instance*) that can be used and modified within our code. 

To declare a variable in python, we  use the `=` sign. If the variable already exists, doing so will overwrite the value currently assigned to the variable. 

To see the value that is currently assigned to a variable, we can use the `print()` function. 


```python
x = 7
y = 3

result = x+y
print(result)
>>> 10 

result = x+x
print(result)
>>> 14 
```

*Objects* can be of different types (or *data structures*). 

The most common object types are; 
* integers : Whole numbers
* floats : Decimal point numbers
* strings : Text
* booleans : Values that are either False (0) or True (1)


Python will infer what object type a variable should be when you declare it. You can see the type of a variable by printing the result of the `type()` function. 

```python
a = 1
print(type(a))
>>> <class 'int' >

b = 1.6
print(type(b))
>>> <class 'float'>

c = "hello"
print(type(c))
>>> <class 'str'>

# Anything in quotation marks will be a string
d = "4"
print(type(d))
>>> <class 'str'>

```

Operations will behave differently depending on the types of objects on which they are used. Some operations don't make sense on some object types, so they will raise an error

```python
# Adding two ints produces an int
print(a+a)
>>> 2

# Adding an int and a float produces a float
print(a+b)
>>> 1.6 

# Adding two strings will concatenate the strings
e = "_world" 
print(c+e)
>>> "hello_world"

# Adding an int and a string raises an error
print(a+c)
>>> Traceback (most recent call last): 
TypeError: unsupported operand type(s) for +: 'int' and 'str
```

You may have noticed in the above examples that the `type()` function outputs the word `class` before the true name of a variable type. 

A `class` is simply the template defining an `object`. Classes contain the information about what a particular object type is, and all of the instructions about what can be done with this object (called *methods*). This is what allows for `+` to mean different things for `strings` than it does for `ints` or `floats`. 


>Note : At a basic level, you don't need to worry too much about all of this. We will not go into learning how to define classes and methods ourselves. However, these are important concepts and vocabulary in programming, and will become relevant when we start working with deep learning models, so its important to have a basic understanding of what they are. 

## Lists, Methods, and Mutability 

As their name suggests, `lists` are simply a class of object that contain instances of other objects or variables. To define a list in python, we use `[]`

```python
# A list containing instances of integers
integer_list = [1, 2, 3, 4]

# A list containing instances of strings
string_list = ["a", "b", "c", "d"]

# A list containing mixed object instances
mixed_list = ["a", 1.5, 7, "hello"]

a = 1
b = 2
c = 3
d = 4

# A list containing variables
variable_list = [a,b,c,d]
```

As mentioned previously, a *method* is some operation that we can apply to an object instance. Objects have specific methods, for example lists have `count`, `sort`, and `reverse` (among others)

To access a method, we use the following syntax : `variable_name.method_name(arguments)`

```python
# Define a variable referencing a list
my_list = [1,3,3,9,7,20,3,3]

# Count the number of '3's in the list
number_of_threes = my_list.count(3)
print(number_of_threes)
>>> 4
```


Some methods will operate *in place*. This means that applying it will change the variable without needing to redefine it. In fact, trying to define a new variable using an in place method will not work. 



```python
# We apply the sort method to our list, which operates in place
my_list.sort() 
print(my_list)
>>> [1,3,3,3,3,7,9,20]

# We try to define a new variable using the reverse method, which also operates in place
reversed_list = my_list.reverse() 
print(reversed_list)
>>> None
print(my_list)
>>> [20,9,7,3,3,3,3,1]
```

So how can we have one sorted list and one reversed list starting from the same original list ? 

We need to *copy* the original list into a new variable before applying in-place methods. But in python, we need to be very careful about the way we make copies of variables!  

Intuitively, we might do the following; 

```Python
# We create the original list
my_list = [1,3,3,9,7,20,3,3]

# We create a new variable and assign the original list to it
new_list = my_list 

# We check that the lists are the same
print(my_list == new_list)
>>> True 

# Now we sort the original list
my_list.sort() 

# We check that it worked
print(my_list)
>>> [1,3,3,3,3,7,9,20]

# Now we check the value of list we kept unsorted
print(new_list)
>>> [1,3,3,3,3,7,9,20]

# !! Wait, what is going on here ? Our new list is also sorted ! 
```

This is because of a thing called *mutability*. When we use the `=` sign to create a "copy" of a mutable object, we aren't actually making a copy of the object. We are simply making a copy to the *reference* of that object. More technically, both variables point to the same place in the computer's memory. 

In the previous example, no matter what we do to `my_list`, it will also change the value of `new_list`.

In order to create a copy of a mutable object that will not affect the original values assigned to that object, we need to make a *shallow* copy. 

> Side Note : We could also make a *deep* copy, but in general cases this is avoided, as it results in doubling the space taken in memory. I am only referencing the terminology here so that you can [look it up](https://realpython.com/python-mutable-vs-immutable-types/#making-copies-of-lists) if you are interested in learning more. 

Mutable objects will usually have a `.copy()` method, which creates a shallow copy. 

```Python
# We create the original list
my_list = [1,3,3,9,7,20,3,3]

# We use .copy() to create a copy
new_list = my_list.copy() 

# We check if new_list and my_list are the same
new_list == my_list 
>>> True 

# We sort the original list
my_list.sort() 

# We check again if new_list and my_list are the same
new_list == my_list 
>>> False 
```

If an object is *immutable*, we don't have to worry about shallow copies. Using the `=` sign will produce the expected behaviour (making a copy that is separate from the original)

Here are the mutabilities of common datatypes in python. 
* Immutable objects :
    * Numbers (ints, floats)
    * Booleans
    * Strings
    * Tuples 
    * Bytes (don't worry about what these are )

* Mutable objects : 
    * Lists
    * Dictionnaries
    * Sets 
    * Numpy arrays (for later)

> Note : I will not be going over tuples, dictionnaries, or sets in this brief tutorial. I encourage you to learn about these by yourself, as we may encounter them when working with models. However, they are somewhat intuitive once you have understood how to work with lists and arrays. 

## Functions, Conditionals, and Loops
As in any programming language, the point of it all ins't just to declare a bunch of variables and print their values. We want to be able to *do* things to these variables. 

### Functions
We have seen what methods are in the previous section. More generally, a method is a *function* (except it belongs to a specific class)

A *function* is a set of instructions, a list of operations to apply on variables, regardless of their actual values. Functions are the backbone of good code. They allow us to avoid rewriting the same code over and over again to do the same thing. 

>Functions take *arguments*,  do something with those arguments, and then *return* value(s). 

>Arguments create variables within the function, called *local variables*. Thse can be operated on or used in logical statements.

To create a function in python, we use the word `def` followed by `:`

```Python
# We define a greet function that takes an argument "name"
def greet(name):
    print("Hello", name)

# We call the function with different values for "name"
greet("John")
>>> "Hello John" 

greet("Mary")
>>> "Hello Mary" 
```


Notice however, what happens when we do the following : 
 ```Python
 x = greet("Sam")
 >>> "Hello Sam" 
 print(x)
 >>> None  
``` 
>When we call the function, the print statement executes and we see `Hello Sam` on the screen. However, you may have thought that the `print(x)` statement would also result in "Hello Sam". Instead, it results in `None`. This is because we have not told our function to `return` anything. So, it returns nothing ! 

```Python
def greet(name):
    print("Hello", name)
    result = "I have greeted" + name 
    return result 

# Calling the function makes the print statement execute
x = greet("Sam")
>>> "Hello Sam" 

# The value returned by the function is "result"
print(x)
>>> "I have greeted Sam" 
```

### Conditional statements 
Now, what if we want our function to behave differently depending on the value passed to it ? We can use *conditional statements* 

> Conditional statements are : 
> `if`, `else if`, and `else`
> and they do exactly what they say they do. They check whether a statement is true or false. Conditional statements can also exist outside of functions of course.

--- 

#### Important note : Indentation

In python, indentation is very important. For example, when we define a function, the code executed within that function should be indented by a tab. **It will not work otherwise**. This is also true for conditional statements. 

```Python
a = 7 
b = 6

# The code to execute if the statement is true is indented by a tab underneath the conditional
if a > b:
    print("True")

>>> True
```

---

Now, imagine we are writing a program to generate a to-do list depending on the weather. If the weather is sunny, our to-do list should contain outside activities. If the weather is not rainy, it should contain inside activities. 

Pay attention to the indentation ! 

```Python
# Define our function
def get_todo_list(weather_condition):
# Checks if the condition is "sunny"
    if weather_condition == "sunny" : 
        to_do_list = ["Weed garden", "Mow Lawn"]
    
# If it is not, it checks whether it is "rainy"
    else if weather_condition == "rainy" : 
        to_do_list = ["Clean bathroom", "Vacuum"]
    
# If it is neither ;
    else : 
        print("I don't know the weather")
        to_do_list = None
    
    return to_do_list 

print(get_todo_list("sunny"))
>>>["Weed garden", "Mow Lawn"]
    
print(get_todo_list("rainy"))
>>>["Clean bathroom", "Vacuum"]

print(get_todo_list("hello"))
>>> "I don't know the weather" 
>>> None
```



> Why do we need the `else if` statement here ? Could we not have used another `if`. Try to think about what would happen? How could you write this function without using `else if` ? \
>**Hint** : The todo list would be `None` when weather condition is "sunny". 

### Loops 

Loops are *iterative* statements. They are bits  of code that execute `for` a specific number of times, or `while` a condition is true. 

In python, `for` loops operate on [iterators](https://docs.python.org/3/tutorial/classes.html#iterators). You don't really need to know exactly what this means. A list is an iterator; this probably gives you enough intuition to use `for` loops correctly

```Python
for number in 6:
    print(number)

>>> Traceback (most recent call last):
>>> File "<stdin>", line 1, in <module>
>>> TypeError: 'int' object is not iterable

for number in [1,2,3,4,5,6]:
    print(number)

>>> 1
>>> 2
>>> 3
>>> 4
>>> 5
>>> 6
```
> A handy tip to know; to get a list like the one above without having to type it out manually, we can use the `range` function 
> ``````
> print(range(6))
>>>> [1,2,3,4,5,6]

`While` loops are a bit different. They will evaluate some condition at the end of every iteration, and depending on whether the condition is true or false, they will continue or stop. 

```Python
x = 0 
while x < 3:
    print(x)
    x = x+1
print("done")

>>> 0
>>> 1 
>>> 2
>>> "done"
```

> At the end of this loop, what is the value of x ? Why ? 

We need to be carefull with `while` loops, because we can end up with a loop that will never stop iterating! For example, a typical mistake is to not update the variable we use to check the condition

```Python
x = 0 
while x < 3:
    print(x)

# This loop will run forever, because x is always 0 
```
