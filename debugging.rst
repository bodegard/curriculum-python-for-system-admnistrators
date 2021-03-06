*************************
Debugging Python Programs
*************************


Techniques
==========


The ``print`` statement
-----------------------

Sometimes the quickest/easiest solution is just to throw a few ``print``
statements into your code.  Not a good idea for complex problems.  Some very
good programmers disdain this technique, calling it sloppy.  However others
really very good programmers, such as Rob Pike (early contributor to Unix_,
father of `Plan 9`_, UTF-8_, and Go_) is said to approve of this method.

.. _`Plan 9`: http://en.wikipedia.org/wiki/Plan_9_from_Bell_Labs
.. _UTF-8: http://en.wikipedia.org/wiki/UTF-8
.. _Unix: http://en.wikipedia.org/wiki/Unix
.. _Go: http://en.wikipedia.org/wiki/Go_(programming_language)


IPython
-------

IPython_ is an interactive interpreter shell with more features than the default
Python REPL shell.

While not used for *debugging* per se, an interactive session can be a good way
to understand and explore small bits of code.

.. _IPython: http://ipython.org

.. code-block:: console

   (venv)$ pip install ipython


Debugger
--------

A debugger is a computer program that lets you run your program, line by line
and examine the values of variables or look at values passed into functions and
let you figure out why it isn't running the way you expected it to. [1]_


Logging
-------

While running code in a debugger offers the maximum visibility into its 
operation, it may be difficult or impossible to correctly simulate a 
production environment (including number of connections, network topography,
load, etc) while a program is running inside a debugger.

Logging statements offer an alternative way to gain insight into your code's
operation.  Verbose logging can be especially helpful for understanding bugs
in highly concurrent code, where it would be difficult to inspect each
thread/process in an ordinary debugger.

Logging is an essential part of `12-Factor methodology`_ for building modern
applications.

.. _`12-Factor methodology`: http://www.12factor.net/



Debuggers
=========


Eclipse
-------

Eclipse offers a graphical debugger, making it easy to explore your code and 
set breakpoints while running in the debugger.

**Note:** Eclipse's PyDev debugger is powerful, but *notoriously* flakey. 
*Expect a few hiccups!*


``pdb``
-------

.. todo::

   Brief description of ``pdb``, maybe a simple example.



Common Species of Bug
=====================


.. todo::

   Add an example for each (?) species of bug.
   
   http://stackoverflow.com/questions/1011431/common-pitfalls-in-python
   
   


Indentation
-----------

Usually results from mixture of tabs & spaces, causing the actual scope of some 
lines to be different than it appears on the programmer's screen.


Wrong Number of Arguments
-------------------------

A function is called with the wrong number (too few or too many) arguments, 
causing an exception to be thrown.


Wrong Package
-------------

You type ``import foobar``, and the module imported is ``foobar.py`` in the 
local folder, not the ``foobar`` module in the standard library.


Catchign Multiple Exceptions
----------------------------

Be careful catching multiple exception types: [2]_

::

   try:
       raise KeyError("hmm bug")
   except KeyError, TypeError:
       print TypeError
       
This prints "hmm bug", though it is not a bug; it looks like we are catching
exceptions of both types, but instead we are catching KeyError only as variable
TypeError.

`PythonTutor can help us visualize what is happening.`__

__ http://www.pythontutor.com/visualize.html#code=try%3A%0A++++raise+KeyError(%22hmm+bug%22)%0Aexcept+KeyError,+TypeError%3A%0A++++print+TypeError&mode=display&cumulative=false&py=2&curInstr=0


The correct way to catch multiple exceptions is to put them in parentheses:


::
   
   try:
       raise KeyError("hmm bug")
   except (KeyError, TypeError):
       print TypeError


`Visualize the correct program flow.`__
 
__ http://www.pythontutor.com/visualize.html#code=try%3A%0A++++raise+KeyError(%22hmm+bug%22)%0Aexcept+(KeyError,+TypeError)%3A%0A++++print+TypeError%0A&mode=display&cumulative=false&py=2&curInstr=0




Unqualified ``except:`` block
-----------------------------

Do you really want to catch *all* exceptions?  Can your ``except`` block
*really* recover from all the exceptions it catches?


Populating Arrays
-----------------

When you need a population of arrays you might be tempted to type something like this: [3]_

::

   >>> a=[[1,2,3,4,5]]*4

And sure enough it will give you what you expect when you look at it

::

   >>> from pprint import pprint
   >>> pprint(a)
   
   [[1, 2, 3, 4, 5],
    [1, 2, 3, 4, 5],
    [1, 2, 3, 4, 5],
    [1, 2, 3, 4, 5]]

But don't expect the elements of your population to be seperate objects:

::

   >>> a[0][0] = 2
   >>> pprint(a)
   
   [[2, 2, 3, 4, 5],
    [2, 2, 3, 4, 5],
    [2, 2, 3, 4, 5],
    [2, 2, 3, 4, 5]]

Unless this is what you need...

`Visualize what is happening.`__

__ http://www.pythontutor.com/visualize.html#code=a+%3D+%5B%5B1,2,3,4,5%5D%5D*4%0A%0Aprint+a%0A%0Aa%5B0%5D%5B0%5D+%3D+2%0A%0Aprint+a&mode=display&cumulative=false&py=2&curInstr=0

It is worth mentioning a workaround:

::

   a = [[1,2,3,4,5] for i in range(4)]



.. rubric:: Footnotes

.. [1] http://cplus.about.com/od/glossar1/g/debugdefinition.htm
.. [2] http://stackoverflow.com/q/1011431/164308
.. [3] http://stackoverflow.com/a/1025447/164308
