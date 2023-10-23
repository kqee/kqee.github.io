---
layout: post
title:  "python decorators"
date: 2023-10-14 06:53:00 -0000
categories: python
---

* * *
table of contents:

* TOC
{:toc}
* * *

## preamble

decorators provide a simple syntax for calling [higher-order functions](http://en.wikipedia.org/wiki/Higher-order_function).

decorators simply mean a function that takes another function and extends its behaviour.

## functions

before knowing how decorators work, it is imperative to know how functions work, because decorators are technically functions:

```py
def multiply_by_three(number):
    return number * 3

multiply_by_three(10) # returns 30
```

function above takes an integer and multiply it by 3.

a function can also be passed to a function, a popular function is `map()`:

```py
array = [5, 5, 5 ,5, 5]
def multiply_by_ten(5):
    return iter * 10
print(map(multiply_by_ten, array))
# outputs [50, 50, 50, 50, 50]
```

the code above simply multiplies all items in the array by ten, with the help of `map()` function.

here are more examples of passing a function as a parameter:

```py
def curse(name):
    return f"{name} has been cursed, HP halved"

def remove_curse(name):
    return f"{name}(s) the curse has been undid"

def do_this(func):
    return func("kyle")

do_this(curse)
do_this(remove_curse)
# try running the code
```

notice that `do_this` returns value of the function `func()` (the parameter which accepts functions), in this case, `func()` is `curse()`
and `remove_curse()`.

python allows you to return functions from functions, imagine the output below:

```py
def func(option: int):
    def first():
        return "I am the 1st!"

    def second():
        return "I am the 2nd!"

    if num == 1:
        return first
    elif num == 2:
        return second
    else:
        return "I am the last..."

first = func(1)
second = func(2)
print(first())
print(second())
# try running above
```

the `func()` function is the parent function, whilst all others function inside the *parent function* are referred to as *child functions*. The *parent function* takes an integer, then it decides if the integer is 1 or 2, else return `"I am the last..."`, otherwise, return a function accordingly.

## decorators

**FYI**, decorators are merely syntatic sugar. Now you've understood how function works, we're moving on to decorators!

below is a simple decorator:
```py
def decorator(f):
    def wrapper():
        print("wrapper called")
        f()
        print("reached the end of wrapper")
    return wrapper

def something():
    print("something")

deco = decorator(something)
# decorator() returns a function; that is wrapper()
deco()
# try running the code
```

`decorator(f)` function takes a function, then we create a wrapper function, invoke the `f()` function (the parameter of `decorator(f)`) in `wrapper()`. Afterwards, we return the `wrapper()` function (NOT the value, the function itself).

above is simply an explicit way of decorating functions, now, the syntatic sugar:

```py
def decorator(f):
    def wrapper():
        print("wrapper called")
        f()
        print("reached the end of wrapper")
    return wrapper

@decorator
def something():
    print("something")

something()
```

maybe it looks even nicer now:

```py
@decorator
def something():
    [...]
```

is an easier way of typing `deco = decorator(something)`.

## decorators(bonus)

what if we want to put arguments to our decorators? It's quite useful.

Imagine if we need to repeat a function for a few times, usually this can be done with a loop, but we want it to be done with a decorator.

So, if the decorator has an argument:

```py
@decorator(arg)
def something():
    pass
```

it translates to:

```py
foo = decorator(arg)(something)
```

thus, we need to have the third inner function:

```py
def decorator(num):
    def wrapper(func):
        def wrapper1():
            for _ in range(num):
                func()
        return wrapper1
    return wrapper

@decorator(5)
def hello():
    print("hello")
```

if you don't understand, I too do not understand, I am writing this whilst having a headache I guess that is it for now...

## refrences

* <https://realpython.com/primer-on-python-decorators/>