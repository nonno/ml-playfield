# Python

Python is extensible: if you know how to program in C it is easy to add a new built-in function or module to the interpreter, either to perform critical operations at maximum speed, or to link Python programs to libraries that may only be available in binary form (such as a vendor-specific graphics library). Once you are really hooked, you can link the Python interpreter into an application written in C and use it as an extension or command language for that application.

The Python `interpreter` is usually installed as /usr/local/bin/python3.9 on those machines where it is available.
By default, Python source files are treated as encoded in UTF-8.

When commands are read from a tty, the interpreter is said to be in `interactive mode`. In this mode it prompts for the next command with the primary prompt, usually three greater-than signs (>>>); for continuation lines it prompts with the secondary prompt, by default three dots (...).

Python does automatic memory management (reference counting for most objects and garbage collection to eliminate cycles). The memory is freed shortly after the last reference to it has been eliminated.

`Comments` in Python start with the hash character, #.

If a `variable is not defined` (assigned a value), trying to use it will give you an error.

## Data types

The integer numbers (e.g. 2, 4, 20) have type `int`, the ones with a fractional part (e.g. 5.0, 1.6) have type `float`.
In addition to int and float, Python supports other types of numbers, such as `Decimal` and `Fraction`. Python also has built-in support for `complex numbers`, and uses the j or J suffix to indicate the imaginary part.

`Strings` can be enclosed in single quotes ('...') or double quotes ("...") with the same result. They are immutable.
If you don’t want characters prefaced by `\` to be interpreted as `special characters`, you can use `raw strings` by adding an r before the first quote.
Two or more string literals (i.e. the ones enclosed between quotes) next to each other are automatically concatenated.

### Lists

It can be written as a list of comma-separated values (items) between square brackets. Lists might contain items of different types, but usually the items all have the same type.
They are mutable.
It is possible to nest lists (create lists containing other lists).

```py
squares = [1, 4, 9, 16, 25]
squares[0]  # indexing returns the item
# 1
squares[-1]
# 25
squares[-3:]  # slicing returns a new list
# [9, 16, 25]
```

Methods like insert, remove or sort that only modify the list have no return value printed - they return the default None. This is a design principle for all mutable data structures in Python.
Lists can act as `stacks` (LIFO) using `append` and `pop`.
Lists can act as `queues` (FIFO) using `collections.deque`, append and `popleft`.

#### List Comprehensions

List comprehensions provide a concise way to create lists.
A list comprehension consists of brackets containing an expression followed by a for clause, then zero or more for or if clauses.

```py
squares1 = []
for x in range(10):
    squares1.append(x**2)

squares2 = list(map(lambda x: x**2, range(10)))

squares3 = [x**2 for x in range(10)]
```

```py
[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
# [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

```py
matrix = [
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12],
]
[[row[i] for row in matrix] for i in range(4)]
# [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

### Tuples

A tuple consists of a number of values separated by commas. They can be nested. They are immutable.
They may be input with or without surrounding parentheses.

```py
t = 12345, 54321, 'hello!'
t[0] # 12345
t # (12345, 54321, 'hello!')

u = t, (1, 2, 3, 4, 5)
u # ((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))

t = 12345, 54321, 'hello!'
x, y, z = t # unpacking a tuple
```

### Sets

A set is an unordered collection with no duplicate elements.
Curly braces or the `set()` function can be used to create sets.
Similarly to list comprehensions, `set comprehensions` are also supported.

```py
basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
print(basket) # {'orange', 'banana', 'pear', 'apple'}

a = {x for x in 'abracadabra' if x not in 'abc'}
a # {'r', 'd'}
```

### Dictionaries

Dictionaries are indexed by keys, which can be any immutable type; strings and numbers can always be keys.
A pair of braces creates an empty dictionary: {}.
The `dict()` constructor builds dictionaries directly from sequences of key-value pairs

```py
tel = {'jack': 4098, 'sape': 4139}
tel['guido'] = 4127
tel['jack'] # 4098
'guido' in tel # True

dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
```

## Code

`Indentation` is Python's way of grouping statements. At the interactive prompt, you have to type a tab or space(s) for each indented line. When a compound statement is entered interactively, it must be followed by a blank line to indicate completion (since the parser cannot guess when you have typed the last line). Note that each line within a basic block must be indented by the same amount.

```py
# multiple assignment
a, b = 0, 1
while a < 10:
    print(a)
    a, b = b, a+b ## a+b is evaluated before
```

### if

```py
x = int(input("Please enter an integer: "))
if x < 0:
    x = 0
    print('Negative changed to zero')
elif x == 0:
    print('Zero')
else:
    print('More')
```

### for

Python’s for statement iterates over the items of any sequence (a list or a string), in the order that they appear in the sequence.
When looping through a sequence, the position index and corresponding value can be retrieved at the same time using the `enumerate()` function.

```py
words = ['cat', 'window', 'defenestrate']
for w in words:
    print(w)
```

`range` generate arithmetic progressions.

```py
for i in range(5):
    print(i)

list(range(0, 10, 3)) # min, limit, step
# [0, 3, 6, 9]

for i, v in enumerate(['tic', 'tac', 'toe']):
    print(i, v)
```

### Loops clauses

The `break` statement, like in C, breaks out of the innermost enclosing for or while loop.

The `continue` statement, also borrowed from C, continues with the next iteration of the loop

Loop statements may have an `else` clause; it is executed when the loop terminates through exhaustion of the iterable (with for) or when the condition becomes false (with while), but not when the loop is terminated by a break statement.

### `pass` statement

The pass statement does nothing. It can be used when a statement is required syntactically but the program requires no action.

```py
class MyEmptyClass:
    pass

def initlog(*args):
    pass   # Remember to implement this!
```

### `match` statement

Patterns can be arbitrarily nested.
Most literals are compared by equality, however the singletons True, False and None are compared by identity.
Like unpacking assignments, tuple and list patterns have exactly the same meaning and actually match arbitrary sequences.

```py
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
        case 500 | 501 | 502:
            return "Server error"
        case _:
            return "Something's wrong with the internet"

match points:
    case []:
        print("No points")
    case [Point(0, 0)]:
        print("The origin")
    case [Point(x, y)]:
        print(f"Single point {x}, {y}")
    case [Point(0, y1), Point(0, y2)]:
        print(f"Two on the Y axis at {y1}, {y2}")
    case _:
        print("Something else")
```

### Functions

The execution of a function introduces a new symbol table used for the local variables of the function. More precisely, all variable assignments in a function store the value in the local symbol table; whereas variable references first look in the local symbol table, then in the local symbol tables of enclosing functions, then in the global symbol table, and finally in the table of built-in names.
When a function calls another function, or calls itself recursively, a new local symbol table is created for that call.
Even functions without a return statement do return a value, this value is called `None`.

```py
def fib(n):    # write Fibonacci series up to n
    """Print a Fibonacci series up to n."""
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()

# Now call the function we just defined:
fib(2000)
```

#### Function argument values

The default values are evaluated at the point of function definition in the defining scope. The default value is evaluated only once. This makes a difference when the default is a mutable object such as a list, dictionary, or instances of most classes. 
In a function call, keyword arguments must follow positional arguments.

It is possible to mark certain parameters as `positional-only`. If positional-only, the parameters' order matters, and the parameters cannot be passed by keyword. Positional-only parameters are placed before a `/`. Use positional-only if you want the name of the parameters to not be available to the user. This is useful when parameter names have no real meaning. For an API, use positional-only to prevent breaking API changes if the parameter’s name is modified in the future.

To mark parameters as `keyword-only`, indicating the parameters must be passed by keyword argument, place an `*` in the arguments list just before the first keyword-only parameter. Use keyword-only when names have meaning and the function definition is more understandable by being explicit with names or you want to prevent users relying on the position of the argument being passed.

```py
def ask_ok(prompt, retries=4, reminder='Please try again!'):
    while True:
        ok = input(prompt)
        if ok in ('y', 'ye', 'yes'):
            return True
        if ok in ('n', 'no', 'nop', 'nope'):
            return False
        retries = retries - 1
        if retries < 0:
            raise ValueError('invalid user response')
        print(reminder)

ask_ok('question')
ask_ok('question', 2)
ask_ok('question', reminder='please')
ask_ok(retries=2, prompt='question')
```

It's possible to specify that a function can be called with an arbitrary number of arguments. These arguments will be wrapped up in a tuple.
Normally, these `variadic arguments` will be last in the list of formal parameters, because they scoop up all remaining input arguments that are passed to the function.

```py
def write_multiple_items(file, separator, *args):
    file.write(separator.join(args))
```

#### Lambda expressions

Small anonymous functions can be created with the `lambda` keyword.
Lambda functions can be used wherever function objects are required. They are syntactically restricted to a single expression. Semantically, they are just syntactic sugar for a normal function definition. Like nested function definitions, lambda functions can reference variables from the containing scope.

```py
def make_incrementor(n):
    return lambda x: x + n

f = make_incrementor(42)
f(0) # 42
f(1) # 43
```

### Coding style and convetions

The first line should always be a short, concise summary of the object's purpose. For brevity, it should not explicitly state the object's name or type.
The first non-blank line after the first line of the string determines the amount of indentation for the entire documentation string.

```py
def my_function():
    "Do nothing, but document it."
    pass

print(my_function.__doc__)
# Do nothing, but document it.
```

#### PEP 8

- Use 4-space indentation, and no tabs.
- Wrap lines so that they don't exceed 79 characters.
- Use blank lines to separate functions and classes, and larger blocks of code inside functions.
- When possible, put comments on a line of their own.
- Use spaces around operators and after commas, but not directly inside bracketing constructs: `a = f(1, 2) + g(3, 4)`.
- `UpperCamelCase` for classes and `lowercase_with_underscores` for functions and methods. Always use `self` as the name for the first method argument

### Modules

Python has a way to put definitions in a file and use them in a script or in an interactive instance of the interpreter. Such a file is called a module; definitions from a module can be imported into other modules or into the main module.

The file name is the module name with the suffix .py appended. Within a module, the module’s name (as a string) is available as the value of the global variable `__name__`.

A module can contain executable statements as well as function definitions. These statements are intended to initialize the module. They are executed only the first time the module name is encountered in an import statement.

Each module has its own private symbol table, which is used as the global symbol table by all functions defined in the module. Thus, the author of a module can use global variables in the module without worrying about accidental clashes with a user’s global variables.

Modules can import other modules. It is customary but not required to place all import statements at the beginning of a module (or script, for that matter).

To speed up loading modules, Python caches the compiled version of each module in the `__pycache__` directory under the name module.version.pyc, where the version encodes the format of the compiled file; it generally contains the Python version number.

Python comes with a library of standard modules, described in a separate document, the Python Library Reference. Some modules are built into the interpreter; these provide access to operations that are not part of the core of the language but are nevertheless built in, either for efficiency or to provide access to operating system primitives such as system calls.
The set of such modules is a configuration option which also depends on the underlying platform. For example, the winreg module is only provided on Windows systems.

The built-in function `dir()` is used to find out which names a module defines. It returns a sorted list of strings. Without arguments, dir() lists the names you have defined currently.

### Packages

Packages are a way of structuring Python’s module namespace by using “dotted module names”. For example, the module name `A.B` designates a submodule named `B` in a package named `A`.

Users of the package can import individual modules from the package.

When using `from package import item`, the item can be either a submodule (or subpackage) of the package, or some other name defined in the package, like a function, class or variable. The import statement first tests whether the item is defined in the package; if not, it assumes it is a module and attempts to load it. If it fails to find it, an `ImportError` exception is raised.

```py
# option 1
import sound.effects.echo
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)

# option 2
from sound.effects import echo
echo.echofilter(input, output, delay=0.7, atten=4)

# option 3
from sound.effects.echo import echofilter
echofilter(input, output, delay=0.7, atten=4)
```

When packages are structured into subpackages, you can use absolute imports to refer to submodules of siblings packages, but also relative imports.

```py
from . import echo
from .. import formats
from ..filters import equalizer
```

### String format

```py
year = 2016
event = 'Referendum'
f'Results of the {year} {event}'
# 'Results of the 2016 Referendum'

yes_votes = 42_572_654
no_votes = 43_132_495
percentage = yes_votes / (yes_votes + no_votes)
'{:-9} YES votes  {:2.2%}'.format(yes_votes, percentage)
```

### Reading and Writing files

It is good practice to use the with keyword when dealing with file objects. The advantage is that the file is properly closed after its suite finishes, even if an exception is raised at some point.

`Pickle` is a protocol which allows the serialization of arbitrarily complex Python objects. As such, it is specific to Python and cannot be used to communicate with applications written in other languages. It is also insecure by default: deserializing pickle data coming from an untrusted source can execute arbitrary code, if the data was crafted by a skilled attacker.

```py
with open('workfile') as f:
    for line in f:
        print(line, end='')

read_data = f.read()

f.write('This is a test\n')
```

```py
import json
x = [1, 'simple', 'list']
json.dumps(x)
```

### Exceptions

```py
try:
    result = x / y
except ZeroDivisionError:
    print("division by zero!")
else:
    print("result is", result)
finally:
    print("executing finally clause")
```

```py
raise ValueError

raise NameError('HiThere')

raise RuntimeError from exc # Exception Chaining
```

### Scope

A scope is a textual region of a Python program where a namespace is directly accessible.

Although scopes are determined statically, they are used dynamically. At any time during execution, there are 3 or 4 nested scopes whose namespaces are directly accessible:

- the innermost scope, which is searched first, contains the local names

- the scopes of any enclosing functions, which are searched starting with the nearest enclosing scope, contains non-local, but also non-global names

- the next-to-last scope contains the current module's global names

- the outermost scope (searched last) is the namespace containing built-in names

The statements executed by the top-level invocation of the interpreter, either read from a script file or interactively, are considered part of a module called `__main__`.

```py
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)

# After local assignment: test spam
# After nonlocal assignment: nonlocal spam
# After global assignment: nonlocal spam
# In global scope: global spam
```

### Classes

Class definitions, like function definitions (def statements) must be executed before they have any effect.

When a class defines an `__init__()` method, class instantiation automatically invokes `__init__()` for the newly-created class instance.

The special thing about methods is that the instance object is passed as the first argument of the function. In our example, the call `x.f()` is exactly equivalent to `MyClass.f(x)`.

If the same attribute name occurs in both an instance and in a class, then attribute lookup prioritizes the instance.

Often, the first argument of a method is called self. This is nothing more than a convention: the name self has absolutely no special meaning to Python. Methods may call other methods by using method attributes of the self argument.

Derived classes may override methods of their base classes. There is a simple way to call the base class method directly: just call `BaseClassName.methodname(self, arguments)`.

Use `isinstance()` to check an instance’s type: isinstance(obj, int) will be True only if `obj.__class__` is int or some class derived from int.

Use `issubclass()` to check class inheritance: issubclass(bool, int) is True since bool is a subclass of int. However, issubclass(float, int) is False since float is not a subclass of int.

Private instance variables that cannot be accessed except from inside an object don't exist in Python. However, there is a convention that is followed by most Python code: a name prefixed with an underscore (e.g. _spam) should be treated as a non-public part of the API (whether it is a function, a method or a data member). 

```py
class MyClass:
    """A simple example class"""
    i = 12345 # class variable

    def __init__(self, name):
        self.name = name # instance variable

    def f(self):
        return 'hello world'

class DerivedClassName(MyClass):
    pass

# Multiple inheritance
class DerivedClassName(Base1, Base2, Base3):
    pass
```

```py
# an empty class can be used to emulate a record
class Employee:
    pass

john = Employee()  # Create an empty employee record

# Fill the fields of the record
john.name = 'John Doe'
john.dept = 'computer lab'
john.salary = 1000
```

### Generators

They are written like regular functions but use the `yield` statement whenever they want to return data. Each time next() is called on it, the generator resumes where it left off (it remembers all the data values and which statement was last executed).

```py
def reverse(data):
    for index in range(len(data)-1, -1, -1):
        yield data[index]
```

### Regular expressions

The re module provides regular expression tools for advanced string processing.

```py
import re
re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest')

re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')
```

### Mathematics

```py
import random
random.choice(['apple', 'pear', 'banana'])

random.sample(range(100), 10)   # sampling without replacement
random.random()    # random float
random.randrange(6)    # random integer chosen from range(6)

import statistics
data = [2.75, 1.75, 1.25, 0.25, 0.5, 1.25, 3.5]
statistics.mean(data)
statistics.median(data)
statistics.variance(data)
```

### Dates and Times

```py
# dates are easily constructed and formatted
from datetime import date
now = date.today()
now

now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")

# dates support calendar arithmetic
birthday = date(1964, 7, 31)
age = now - birthday
age.days
```

### Logging

```py
import logging
logging.debug('Debugging information')
logging.info('Informational message')
logging.warning('Warning:config file %s not found', 'server.conf')
logging.error('Error occurred')
logging.critical('Critical error -- shutting down')
```

### Decimal Floating Point Arithmetic

The decimal module offers a Decimal datatype for decimal floating point arithmetic. Compared to the built-in float implementation of binary floating point, the class is especially helpful for having control over precision.

```py
from decimal import *
round(Decimal('0.70') * Decimal('1.05'), 2) # Decimal('0.74')

round(.70 * 1.05, 2) # 0.73
```