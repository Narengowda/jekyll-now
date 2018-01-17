---
title: Collection of Python tricks for faster and better coding
undefined: python questions for learning the hardway
created_date: 2018-01-17 00:00:00 +0530
date: 2018-01-17 21:51:01 +0000
---
Monkey patch a object/class

    
    class Foo(object):
        def __init__(self, arg1, arg2, **kwargs):
            #do stuff with arg1 and arg2
            self.__dict__.update(kwargs)
    
    f = Foo('arg1', 'arg2', bar=20, baz=10)
    # Now f is a Foo object with two extra attributes

Chaining comparision operator

    >>> x = 5
    >>> 1 < x < 10
    True
    
    # This is equal to
    >>> (1 < x) and (x < 10)
    

Set operations

    >>> # Schortcut to create set
    >>> s = {1, 2, 3, 4}
    >>> 
    >>> a = set([1,2,3,4])
    >>> b = set([3,4,5,6])
    >>> a | b # Union
    {1, 2, 3, 4, 5, 6}
    >>> a & b # Intersection
    {3, 4}
    >>> a < b # Subset
    False
    >>> a - b # Difference
    {1, 2}
    >>> a ^ b # Symmetric Difference
    {1, 2, 5, 6}
    

Create objects at run time !!

    >>> NewType = type("NewType", (object,), {"x": "hello"})
    >>> n = NewType()
    >>> n.x
    "hello"
    which is exactly the same as
    
    >>> class NewType(object):
    >>>     x = "hello"
    >>> n = NewType()
    >>> n.x
    "hello"

Writing descriptors

The following code creates a class whose objects are data descriptors which print a message for each get or set. Overriding __getattribute__ is alternate approach that could do this for every attribute. However, this descriptor is useful for monitoring just a few chosen attributes:

    class RevealAccess(object):
        """A data descriptor that sets and returns values
           normally and prints a message logging their access.
        """
    
        def __init__(self, initval=None, name='var'):
            self.val = initval
            self.name = name
    
        def __get__(self, obj, objtype):
            print 'Retrieving', self.name
            return self.val
    
        def __set__(self, obj, val):
            print 'Updating' , self.name
            self.val = val
    
    >>> class MyClass(object):
        x = RevealAccess(10, 'var "x"')
        y = 5
    
    >>> m = MyClass()
    >>> m.x
    Retrieving var "x"
    10
    >>> m.x = 20
    Updating var "x"
    >>> m.x
    Retrieving var "x"
    20
    >>> m.y
    5

The protocol is simple and offers exciting possibilities. Several use cases are so common that they have been packaged into individual function calls. Properties, bound and unbound methods, static methods, and class methods are all based on the descriptor protocol.

String formatting old and new format compared here

For python string formatting visit \[link\]([https://pyformat.info/](https://pyformat.info/ "https://pyformat.info/"))

Comprehensions

    >>> {i**2 for i in range(5)}
    set([0, 1, 4, 16, 9])
    >>> {i: i**2 for i in range(5)}
    {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
    >>> [i**2 for i in range(5)]
    [0, 1, 4, 9, 16]

Ternary operator

    
    >>> y = 1
    >>> x = 3 if (y == 1) else 2
    >>> x
    3
    >>>
    >>> x = 3 if (y == 1) else 2 if (y == -1) else 1
    >>> x
    3

This functionality will be usefull in webapp to accept get/post true/false param

    >>> import json
    >>> json.loads('true')
    True
    >>> json.loads('false')
    False
    

Debug a function from console

    >>> import pdb
    >>> import mymodule
    >>> pdb.run('mymodule.test()')
    

List of list flattening

    >>> l = [[1, 2, 3], [4, 5], [6], [7, 8, 9]]
    >>> sum(l, [])
    [1, 2, 3, 4, 5, 6, 7, 8, 9]
    

Quick benchmark your code

    python -m timeit 'r = range(0, 1000)' 'for i in r: pass'

You can send messages to yield !!

    # returns 4 until you tell generator to 'stop' !!
    >>> def looper():
    ...     while True:
    ...             inp = (yield 4)
    ...             if inp == 'stop':
    ...                     raise GeneratorExit()
    ...
    >>> gen = looper()
    >>> gen.next()
    4
    >>> gen.next()
    4
    >>> gen.send('sdf')
    4
    >>> gen.send('stop')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "<stdin>", line 5, in looper
    GeneratorExit
    >>>