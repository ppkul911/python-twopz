# The Way of Pythonic Zen

## Rapid Prototyping in Python: Tips, tricks & guidelines for writing effective and efficient code quickly

Ever since I started coding in Python, I realized that since Python is a
dynamically-typed & interpreted language, it lends itself very well
to rapid prototyping. A little while later I started writing stuff down
about Python and programming in general so that I can have some reference
material at hand whenever I needed a quick refresher on any particular
subject matter. The result of that endeavor is what you see below. The
name, of course, is a nod to PEP20, The Zen of Python and to the book
'The Way of Zen' by Alan Watts. I intend to continue to expand on this
and keep adding more content and code snippets. Disclaimer: This is
mostly meant as a reference for myself. I cannot guarantee the fitness
of the said material. You have been warned. That being said, please feel
free to contribute! -- Pradyumna 'Prad' Kulkarni

Note: When I said Python is an interpreted language, that was a somewhat
loose definition of what we mean when we say a language is either
'compiled' or 'interpreted'. Read more on that in [THIS](https://nedbatchelder.com/blog/201803/is_python_interpreted_or_compiled_yes.html) 
wonderful article.


Contents 
========

[1000 Ways To Die: Code execution methods](#1000-ways-to-die-code-execution-methods)

[Batteries Included: The Python Standard Library](#batteries-included-the-python-standard-library)

[The Wheel Emporium: Dealing with third-party packages](#the-wheel-emporium-dealing-with-third-party-packages)

[Square Pegs & Round Holes: Python coding conventions](#square-pegs--round-holes-python-coding-conventions)

[A Product of your Environment: Maintaining Environments](#a-product-of-your-environment-maintaining-environments)

[Snakes On Your Brain: Writing Pythonic code](#snakes-on-your-brain-writing-pythonic-code)

[It's Magical!: Magic / Dunder methods & attributes](#its-magical-magic--dunder-methods--attributes)

[Weapon of Choice: Picking an IDE for the task](#weapon-of-choice-picking-an-ide-for-the-task)

[What's in a Name?: Namespace & Scope](#whats-in-a-name-namespace--scope)

[Exceptions Are The Rule: Error & Exception Handling](#exceptions-are-the-rule-error--exception-handling)

[Practice Makes Perfect: Best Practices for writing good code](#practice-makes-perfect-best-practices-for-writing-good-code)

[Talk Shop: Python glossary](#talk-shop-python-glossary)

## 1000 Ways To Die: Code execution methods

Python code can be executed in a number of ways:

-   **Line-wise** inside a Python shell which is invoked within any
    Command Line Interface (Terminal, Command Prompt, PowerShell etc)

-   **Cell-wise** inside of IDEs such as Jupyter Notebook

-   **Block-wise** is where you can select and execute particular
    portions of code inside of IDEs such as Spyder (note that sometimes
    these IDEs might refer to the selection as a 'cell')

-   **File-wise** is when all the code written in file/window is
    executed in an IDE (the good-old
    'run-everything-you-can-and-always-get-a-compile-error' method)

-   **Script-wise** is when you pass the name of your python script as
    an argument to the python CLI command python myscript.py


### Tips for Rapid Prototyping:

-   Stick to line-by-line execution (ideally with a Jupyter Lab
    Notebook) when you're starting from scratch. This allows for quick
    experimentation with variables/objects without having to worry about
    namespace and scope (more on that later).

-   When things get messy and you begin to see unexpected behavior, it's
    always a good idea to restart your kernel and start fresh. You can
    also choose to include lines above your major cell block that
    reinitialize objects, like such:

    ```python
    new_list = []
    #or
    new_dataframe = pd.DataFrame()
    ```

-   Don\'t wrap anything in functions in the beginning (that\'s why
    line-by-line execution). This will again allow for greater
    exploration of all data/objects, allowing for quick troubleshooting
    and rapid, experiment-driven development that allows your
    \'functions\' to rapidly evolve. You can always include a
    docstring-like triple quote comment to identify a logical block of
    code that operates like a function.

-   Once you have reached a functional milestone and have settled down
    on functions, use key variables in global context to easily access
    data for troubleshooting / further development.

-   This idea might be a little contentious, but I am of the opinion
    that functional code \> object-oriented code when you don\'t intend
    to concurrently work on multiple instances of your hypothetical,
    as-yet-undefined class. This really helps in keeping the control
    flow linear and clutter free in your mind and in your code.
    Abstraction is only good when it makes sense.

## Batteries Included: The Python Standard Library

Python is a super friendly and a very forgiving language. That means one
can hit the ground running in a few weeks of having learnt Python.
However, to get the most mileage out of Python, it is recommended that
one goes over, at least superficially, The Python Standard Library Some
of the reasons why you should go over the Standard Library:

-   The Library will introduce you to important built-in functions such
    as id(), type(),isinstance(), issubclass(), hasattr(), any(), all(),
    etc., which are Intrinsically linked with how Python, as a
    programming language, is designed. The ability to use these
    appropriately can exponentially boost your code's effectiveness and
    efficiency.

-   The Python standard library includes high performance datatypes
    written in C or Cython. These also include container data types
    found in *Collections*. can extend the same by inheriting from them.

### Tips for Rapid Prototyping:

-   Python can sometimes be very tricky when it comes to how the
    interpreter allows the user to address in-memory data. In Python,
    the variable itself is the pointer. This can lead to situations
    where you think you are making changes to a copy of an object
    residing in a completely different physical address in the memory,
    however all your pointers (variables) are pointing to the same data.
    This can be quickly verified by calling *id()* on the variables and
    ascertaining whether they are actually different objects in the
    memory or not.

-   You can get around the above situation by using the *copy* module of
    the PSL which allows the user to obtain a 'deep copy' of the object
    in question.

-   To build on the earlier points, it often happens that different
    objects behave differently when operated upon. Some will (1) return
    an updated version of the object (which needs to be caught),
    some (2) do not return anything (object is updated *inplace*), some
    that (3) allow you to decide what happens and some, where (4) what's
    returned is contingent on the operation being performed. Let's take
    a look at some examples:

    ```python
    capitalized_string = new_string.capitalize() #(1)updated copy of object is returned and caught
    new_list.append(new_element) #(2)inplace operation, does not return anything
    new_dataframe.reset_index(inplace=True) #(3)True argument passed for the 'inplace' parameter makes this an inplace operation
    df_with_reset_index = new_dataframe.reset_index(inplace=False) #(3)False argument passed for the 'inplace' parameter, returned object needs to be caught
    popped_element = new_list.pop() #(4)popped element is returned, operation is inplace
    ```

-   Make liberal use of the type() function to understand the objects
    you are working with. This is especially true when working with
    packages that return unknown data types or objects. A lot of times,
    you\'ll notice that the object inherits from Python's native data
    types. This will make it much easier for you to work with the object
    because you're already familiar with the behavior of the native data
    type it inherits from. This makes for a great segue into the next
    point.

-   Python diverges significantly from other, more traditional,
    programming languages by handling 'typecasting' for the programmer,
    which allows the language to be \'aware\' of the data type being
    worked on. We call this 'dynamic typing'. Since Python is
    dynamically typed, the nature of the objects you receive as an
    output can vary depending on the inputs/operators. For example, with
    some inputs you may receive a dictionary and with others, a list. In
    cases like these isinstance() can be used to separately define how
    you handle each case.


## The Wheel Emporium: Dealing with third-party packages

One of the greatest strengths of Python is the enormous library of
third-party packages that you can easily import and integrate into your
own codebase. There really is no reason to reinvent the wheel and there
probably is a package for anything that you think arguably might have
already been done before. However, it is important to maintain separate
virtual environments so that you can avoid conflicts in dependencies
(more on this later).

### Tips for Rapid Prototyping:

-   Always try and search PyPI / Anaconda Cloud to find the package with
    the function you're looking for before setting out to code it on
    your own.

-   Import only what you need. Importing whole modules when you only
    need a few parts adds unnecessary resource overhead to your project.
    There is also the added benefit of keeping your namespace clean and
    the ability to quickly discern what that particular portion of the
    code is doing by looking at the imports. Let's take a look at an
    example:

    ```python
    import os
    os.getcwd()
    # is the same as:
    from os import getcwd
    getcwd()
    ```

-   Some IDEs actually allow you to identify imports that are not being
    used by showing some sort of indicator in the 'gutter'. This can be
    an excellent tool to quickly import a 'superset' of all packages
    used across the project and then selectively getting rid of the
    packages that are not being used in that file.


## Square Pegs & Round Holes: Python coding conventions

Python, just like any other programming language, has conventions
regarding a lot of things, ranging from class declaration to the use of
underscore. Always follow PEP conventions; especially regarding
naming/declaration/formatting of classes, functions, variables and
docstrings. This will not only help improve the readability of your own
code, but also improve your ability to better understand others\' code.
It will also allow you to get around bad documentation by directly
reading the actual code, and what it does, instead of the documentation.

### Tips for Rapid Prototyping:

-   A great way of sticking to conventions, with regards to project
    structure, is to fork boilerplate code that's already available on
    Github.

-   Keep an eye out for naming conventions regarding importation of
    packages. For example, the pandas package is imported as 'pd' by
    convention.

-   This article presents an excellent overview of Python coding
    conventions prescribed in PEP8: https://realpython.com/python-pep8/


## A Product of your Environment: Maintaining Environments

Most Python projects make us of Virtual Environments that are meant to
quarantine individual development environments. This is really useful
for avoiding problems arising out of conflicts in dependencies. You can
use either *venv* (part of PSL) or *conda* (part of the Anaconda Python
distribution). I prefer conda because it plays nice with pip as well as
the Anaconda cloud repositories and also because conda offers a better
overall experience. Using conda can be a life-saver, allowing you to
fool around with isolated environments without worrying about messing up
the dependencies of other projects. Not heeding this warning can lead to
you having to reinstall a lot of things (like Python or Anaconda), and
in some particularly bad cases (\*cough\* tensorflow-gpu \*cough\*
Nvidia libraries) you might have to wipe your entire disk and reinstall
the OS from scratch. The 'Managing Environments' section of conda
documentation is a great place to start:
https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html\#

### Tips for Rapid Prototyping:

-   Try not to use the default 'base' environment in conda. Making changes to this environment can lead to
    strange and unwanted behavior is other environments. This is because
    Anaconda uses 'symbolic links' to prevent duplication of packages on
    the disk.

-   Make use of ipykernel package to bring the
    environments to your Notebooks. Usually, Notebooks default to the
    base environment and don't allow you to access other environments.
    To change that, all you need to do is install the ipykernel package
    and install the kernel for that environment. The ipykernel
    documentation on kernel installation has all the info you need:
    [**https://ipython.readthedocs.io/en/stable/install/kernel\_install.html**](https://ipython.readthedocs.io/en/stable/install/kernel_install.html)

-   Keeping your conda environments is easy.
    Conda will even handle dependency conflicts for you by
    upgrading/downgrading packages to achieve the best results. You just
    need to run two commands, one to update conda itself and another to
    update all the packages in an environment:

    ```
    conda update conda
    conda update --all
    ```


## Snakes On Your Brain: Writing Pythonic code

Here are a few tips/tricks to ensure that your code is as 'Pythonic' as
possible:

-   Keep it DRY -- DRY stands for Don't Repeat Yourself and is
    particularly applicable for Python. The code you write should be
    succinct, clean, modular and extensible. You should never rewrite
    the same code within the same file or in other parts of your larger
    project. One easy way Python allows you to do that is by having a
    folder structure and using an empty '\_\_init\_\_.py' file within
    the folder to indicate to the interpreter the presence of a package,
    from which you can then import functions and global variables. The
    same can be done for Notebooks, if you so fancy, by using a module
    called 'import\_ipynb' which will allow you to import whole
    Notebooks or parts thereof.

-   Writing code that is \'Pythonic\' obviously involves making
    extensive use of Pythonic constructs such as list comprehensions,
    lambda expressions and decorators.

    -   List comprehensions go something like this:

        ```python
        new_list = [i.some_method_call() if some_statement else some_func(i) for i in old_list]
        ```
        
        Note that you can either invoke a method on the list constituent
        object or pass the object as an argument for some other function.


    -   Lambda expressions (aka Anonymous functions) go something like this:

        ```python
        x = lambda a, b, c: a + b + c
        ````

        So when we call x(3,4,5), 11 is returned.

    -   Finally, decorators are used to augment a function (change the
        functionality of a function) by wrapping it inside a different
        function.


-   Make use of class properties (using the \@property decorator)
    wherever necessary to keep your code robust and achieve decoupling
    from the end-user / downstream usage. This is especially true for
    \'dynamic\' attributes that are liable to change post object
    initialization. Here is an excellent article on the topic:

-   Be OS agnostic: Make sure you use the OS module for handling
    paths/file system exploration/file operations. This will make your
    code OS agnostic and will get rid of all the hacky/buggy stuff you
    wrote when you didn't know any better.

-   Be aware of the OOP paradigm; everything in Python is an object. A
    string? Yes, it's an object. A function? Yes, that too. What about a
    literal? Yup.

-   

## It's Magical!: Magic / Dunder methods & attributes

```
Placeholder; will be updated soon
```

## Weapon of Choice: Picking an IDE for the task

```
Placeholder; will be updated soon
```
## What's in a Name?: Namespace & Scope

```
Placeholder; will be updated soon
```
## Exceptions Are The Rule: Error & Exception Handling

```
Placeholder; will be updated soon
```

## Practice Makes Perfect: Best Practices for writing good code

```
Placeholder; will be updated soon
```

## Talk Shop: Python glossary

```
Placeholder; will be updated soon
```
