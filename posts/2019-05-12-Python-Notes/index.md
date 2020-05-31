# Python Notes


* [Basics](#basics)
* [Strings](#strings)
* [Numbers](#numbers)
* [Lists](#lists)
* [Tuples](#tuples)
* [Dictionaries](#dictionaries)
* [Sets](#sets)
* [Input](#input)
* [Functions](#functions)
* [OOP](#oop)
* [Random](#random)
* [Files](#files)
* [Exceptions](#exceptions)
* [Testing](#testing)
* [References](#references)

## Basics

1. If-elif-else

2. `dir(random)` prints all the attributes of the random module.

3. `help(random.randint)` gives the help.

4. Everything in Python is an object. Python is object-based.

5. `type()` gives the type of the object.

## Strings

1. Change Case:`.title()` `.upper()` `.lower()`
2. f-strings are preferred.

    ```python
    name = "foo"
    print(f"The name is {name}.")
    ```

3. Stripping Whitespace: `.lstrip()` `.rstrip()` `.strip()`

4. `join`: joins the elements of an iterable (list/tuple/dictionary) into a single string:

    ```python
    A = ["A", "B", "C"]
    x = "#".join(A) # A#B#C
    ```

5. Multiline string (' and " are equivalent):

    ```python
    a = ''' First line
    second line
    third line
    '''
    ```

6. string to list: `string.split("delimiter")`

    ```python
    my_string = "Hello World"
    my_list = my_string.split(" ") # ["Hello", "World"]
    ```

## Numbers

1. We can group digits using underscores to make large numbers more readable:

    ```python
    universe_age = 14_000_000_000
    print(universe_age)
    # Output: 14000000000
    ```

2. Multiple Assignment: `x, y, z = 1, 2, 3`

3. Constants: no-built in constants, use uppercase as a convention (like in C/C++): `MAX = 100`

4. Exponential: Use \*\*: `a = 2 ** 3`

5. range(): `range(1,11,2)` is 1, 3, 5, 7, 9

## Lists

1. Adding:
    * Insert: `A.insert(0, foo)`
    * Append: `A.append(foo)`
    * Extend: `A.extend([1,2])`
    * Concatenate: `A += B`

2. Removing:
    * By index
        * del: `del A[0]`
        * pop():
            * `last = A.pop()`
            * `first = A.pop(0)`
    * By value
        * remove(): `A.remove(foo)`

3. Organizing:
    * Sorting (Alphabetically): `A.sort()` `A.sort(reverse=True)` `print(sorted(A))`
    * Reversing (Chronologically): `A.reverse()`

4. `min(A) max(A) sum(A)`

5. List Comprehensions: `A = [a ** 2 for a in range(1,3)]` `A = [1,4]`

6. Slicing (not end-inclusive): `A = [0,1,2,3,4]`
    * `print(A[1:4])` gives 1,2,3
    * `print(A[:4])` is equivalent to `print (A[0:4])`
    * `print(A[1:])` is equivalent to `print (A[1:5])`
    * `print(A[-3:])` is equivalent to `print (A[2:])`
    * `print(A[0:5:2])` or `print(A[::2])` gives every second letter.
    * `print(A[::-1])` prints backwards.
    * Slicing is nondestructive, while list methods change the state of a list.

7. Copying: `B = A[:]` (full slicing) instead of `B = A`. Or `B = A.copy()`.

8. Checking existence:

    ```python
    A = [1,2,3,4,5]
    if 1 in A:
        print("1 in A")
    if 6 not in A:
        print("6 not in A")
    ```

9. With while loops:

    ```python
    while A: # while A is not empty

    while 1 in A: # while there is 1 in A
    ```

## Tuples

1. Tuples can't be modified: `A = (1,2)` `A[0] = 3` doesn't work. (Immutable list)

2. But tuples can be reassigned: `A = (1,2)` `A = (3,2)` works.

3. For a single-object tuple like `t = ("Python")`, it becomes a string. But if we add a comma, it becomes a tuple `t = ("Python",)`

## Dictionaries

1. Basic usage:

    ```python
    A = {"language": "python", "age": 19}
    print(A["language"])
    A["height"] = 190 # adding a new pair
    A["language"] = "C++" # modifying
    ```

2. get() (When not sure if the key exists): `print (A.get("weight", "no weight assigned"))`

3. Looping through:

    ```python
    for k,v in A.items(): # keys and values

    for k in A.keys(): # equivalent to the line below
    for k in A: # since looping through the keys is default
    for k in sorted(A.keys()) # sorted

    for v in A.values(): # just values
    for v in set(A.values()) # unique values
    ```

4. Lists and dictionaries can be nested into each other or themselves.

5. Check for membership with `in` and `not in`:

    ```python
    if 'bananas' in fruits:
        fruits['bananas'] += 1
    else: # not in
        fruits['bananas'] = 1
    ```

6. `setdefault()` to avoid KeyError: `fruits.setdefault(fruit, 0)`

7. `import pprint` `pprint.pprint()` pretty-print for complex data structures.

## Sets

1. Basic usage:

    ```python
    languages = {"python", "C++", "C", "python"}
    print (languages)
    # Output {'python', 'C++', 'C'}
    word = "hello"
    wordSet = set(word)
    ```

2. `.union()`, `.difference()`, `.intersection()`

## Input

1. Reading an int: `n = int(input("Please input a number: "))`

## Functions

1. Optional keyword arguments that avoid confusion:

    ```python
    def minus(a, b):
        return (a - b)

    print(minus(a=1, b=2)) # is equivalent to the line below
    print(minus(b=2, a=1))
    ```

2. Default values for the parameters can be added:

    ```python
    def minus(a, b = 0):
        return (a - b)

    print(minus(1))
    ```

3. We can make an argument optional by using None or "":

    ```python
    def build_person(first, last, age=None):
        person = {"first_name": first, "last_name": last}
        if age:
            person["age"] = age
        return person
    ```

4. To prevent a function from modifying a list, pass the list with full slicing: `foo(A[:])`

5. Variadic functions:

    ```python
    def print_languages(*languages): # The * makes an empty tuple and packs any value it receives
        for language in languages:
            print(f"- {language}")
    # more generically, *args
    # **kwargs for key-value pairs
    ```

6. Importing:

    ```python
    from module_name import func_1, func_2, func_3 # importing multiple functions

    from module_name import func_1 as f # alias
    import module_name as m # alias
    from module_name import * # all functions
    # We must import everything at the beginning of each file
    ```

## OOP

1. Default value:

    ```python
    class foo:
        def __init__(self, value):
            self.value = 0
    ```

2. Inheritance:

    ```python
    from foo import Foo
    class Foo(Bar): # Bar is the parent class
        def __init__(self, value):
            super().__init__(value) # superclass
    # we can also override a method by redefining it in the child class.
    ```

3. Conventions:
    * Capitalize the first letter of each word, **without** underscores. Instances and module names use underscores and are in lowercase.

## Random

1. randint:

    ```python
    from random import randint
    print(randint(1,6))
    ```

2. choice:

    ```python
    from random import choice
    A = [1,2,3,4,5,6]
    print(choice(A))
    ```

## Files

1. Reading an entire file:

    ```python
    with open("file.txt") as file_obj: # "r" is the default mode
        contents = file_obj.read()
        # lines = file_obj.readlines() -> a list of lines
        # for line in lines:
            #print(line.rstrip())
        print(contents.rstrip()) # Removing the additional \n
        # no need to close, python will do it automatically
    ```

2. Reading line by line:

    ```python
    with open("file.txt") as file_obj:
        for line in file_obj:
            print(line.rstrip())
    ```

3. Writing:

    ```python
    # python only writes strings, use str() if necessary
    with open("file.txt", "w") as file_obj: # "a", "r+"
        file_obj.write("Python.")
    ```

## Exceptions

1. Basic try-except-else:

    ```python
    try:
        print(1/0)
    except ZeroDivisionError:
        print("Can't divide by 0.")
    else: # optional
        print("Success")
    ```

2. Failing silently using *pass*:

    ```python
    try:
        with open("file.txt") as file_obj:
            content = file_obj.read()
    except FileNotFoundError:
        pass
    else:
         print(len(content.split())) # word count
    ```

## Testing

1. Basic unit testing:

    ```python
    import unittest
    from file import function_1:

    class Func1Test(unittest.TestCase):
        def test(self):
            result = function_1(value)
            self.assertEqual(result, foo)

    if __name__ = "__main__":
        unittest.main()
    ```

2. setUp() method can be used to test a class

## References

* [Python Crash Course, 2nd Edition: A Hands-On, Project-Based Introduction to Programming](https://www.amazon.com/Python-Crash-Course-2nd-Edition/dp/1593279280/ref=sr_1_1?keywords=python+crash+course&qid=1558808134&s=gateway&sr=8-1)

* [Head First Python](https://www.goodreads.com/book/show/8933914-head-first-python?ac=1&from_search=true&qid=6qxJAxXHSN&rank=1)

