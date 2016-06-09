# New Features

### PEP 492 - Coroutines with async and await syntax
PEP 492 greatly improves support for asynchronous programming in Python by adding awaitable objects, coroutine functions, asynchronous iteration, and asynchronous context managers.

Coroutine functions are declared using the new async def syntax:

```
>>>
>>> async def coro():
...     return 'spam'
```

Inside a coroutine function, the new await expression can be used to suspend coroutine execution until the result is available. Any object can be awaited, as long as it implements the awaitable protocol by defining the __await__() method.

PEP 492 also adds async for statement for convenient iteration over asynchronous iterables.

An example of a rudimentary HTTP client written using the new syntax:

```python
import asyncio

async def http_get(domain):
    reader, writer = await asyncio.open_connection(domain, 80)

    writer.write(b'\r\n'.join([
        b'GET / HTTP/1.1',
        b'Host: %b' % domain.encode('latin-1'),
        b'Connection: close',
        b'', b''
    ]))

    async for line in reader:
        print('>>>', line)

    writer.close()

loop = asyncio.get_event_loop()
try:
    loop.run_until_complete(http_get('example.com'))
finally:
    loop.close()
```

Similarly to asynchronous iteration, there is a new syntax for asynchronous context managers. The following script:

```python
import asyncio

async def coro(name, lock):
    print('coro {}: waiting for lock'.format(name))
    async with lock:
        print('coro {}: holding the lock'.format(name))
        await asyncio.sleep(1)
        print('coro {}: releasing the lock'.format(name))

loop = asyncio.get_event_loop()
lock = asyncio.Lock()
coros = asyncio.gather(coro(1, lock), coro(2, lock))
try:
    loop.run_until_complete(coros)
finally:
    loop.close()
```

will output:

```python
coro 2: waiting for lock
coro 2: holding the lock
coro 1: waiting for lock
coro 2: releasing the lock
coro 1: holding the lock
coro 1: releasing the lock
```

Note that both async for and async with can only be used inside a coroutine function declared with async def.

Coroutine functions are intended to be run inside a compatible event loop, such as the asyncio loop.

> See also  
PEP 492 – Coroutines with async and await syntax  
&emsp;PEP written and implemented by Yury Selivanov.

### PEP 465 - A dedicated infix operator for matrix multiplication

PEP 465 adds the @ infix operator for matrix multiplication. Currently, no builtin Python types implement the new operator, however, it can be implemented by defining __matmul__(), __rmatmul__(), and __imatmul__() for regular, reflected, and in-place matrix multiplication. The semantics of these methods is similar to that of methods defining other infix arithmetic operators.

Matrix multiplication is a notably common operation in many fields of mathematics, science, engineering, and the addition of @ allows writing cleaner code:

`S = (H @ beta - r).T @ inv(H @ V @ H.T) @ (H @ beta - r)`

instead of:

```python
S = dot((dot(H, beta) - r).T,
        dot(inv(dot(dot(H, V), H.T)), dot(H, beta) - r))
        
```

NumPy 1.10 has support for the new operator:

```shell
>>>
>>> import numpy

>>> x = numpy.ones(3)
>>> x
array([ 1., 1., 1.])

>>> m = numpy.eye(3)
>>> m
array([[ 1., 0., 0.],
       [ 0., 1., 0.],
       [ 0., 0., 1.]])

>>> x @ m
array([ 1., 1., 1.])
```

> See also  
PEP 465 – A dedicated infix operator for matrix multiplication  
&emsp;PEP written by Nathaniel J. Smith; implemented by Benjamin Peterson.

### PEP 448 - Additional Unpacking Generalizations
PEP 448 extends the allowed uses of the * iterable unpacking operator and ** dictionary unpacking operator. It is now possible to use an arbitrary number of unpackings in function calls:

```shell
>>>
>>> print(*[1], *[2], 3, *[4, 5])
1 2 3 4 5

>>> def fn(a, b, c, d):
...     print(a, b, c, d)
...

>>> fn(**{'a': 1, 'c': 3}, **{'b': 2, 'd': 4})
1 2 3 4
```

Similarly, tuple, list, set, and dictionary displays allow multiple unpackings:

```shell
>>>
>>> *range(4), 4
(0, 1, 2, 3, 4)

>>> [*range(4), 4]
[0, 1, 2, 3, 4]

>>> {*range(4), 4, *(5, 6, 7)}
{0, 1, 2, 3, 4, 5, 6, 7}

>>> {'x': 1, **{'y': 2}}
{'x': 1, 'y': 2}
```

> See also  
PEP 448 – Additional Unpacking Generalizations  
&emsp;PEP written by Joshua Landau; implemented by Neil Girdhar, Thomas Wouters, and Joshua Landau.

### PEP 461 - % formatting support for bytes and bytearray
PEP 461 adds support for the % interpolation operator to bytes and bytearray.

While interpolation is usually thought of as a string operation, there are cases where interpolation on bytes or bytearrays makes sense, and the work needed to make up for this missing functionality detracts from the overall readability of the code. This issue is particularly important when dealing with wire format protocols, which are often a mixture of binary and ASCII compatible text.

Examples:

```shell
>>>
>>> b'Hello %b!' % b'World'
b'Hello World!'

>>> b'x=%i y=%f' % (1, 2.5)
b'x=1 y=2.500000'
```

Unicode is not allowed for `%b`, but it is accepted by `%a` (equivalent of `repr(obj).encode('ascii', 'backslashreplace')`):

```shell
>>>
>>> b'Hello %b!' % 'World'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: %b requires bytes, or an object that implements __bytes__, not 'str'

>>> b'price: %a' % '10€'
b"price: '10\\u20ac'"
```

Note that `%s` and `%r` conversion types, although supported, should only be used in codebases that need compatibility with Python 2.

> See also  
PEP 461 – Adding % formatting to bytes and bytearray  
&emsp;PEP written by Ethan Furman; implemented by Neil Schemenauer and Ethan Furman.

### PEP 484 - Type Hints
Function annotation syntax has been a Python feature since version 3.0 (PEP 3107), however the semantics of annotations has been left undefined.

Experience has shown that the majority of function annotation uses were to provide type hints to function parameters and return values. It became evident that it would be beneficial for Python users, if the standard library included the base definitions and tools for type annotations.

PEP 484 introduces a provisional module to provide these standard definitions and tools, along with some conventions for situations where annotations are not available.

For example, here is a simple function whose argument and return type are declared in the annotations:

```python
def greeting(name: str) -> str:
    return 'Hello ' + name
```

While these annotations are available at runtime through the usual `__annotations__` attribute, *no automatic type checking happens at runtime*. Instead, it is assumed that a separate off-line type checker (e.g. [mypy](http://mypy-lang.org/)) will be used for on-demand source code analysis.

The type system supports unions, generic types, and a special type named Any which is consistent with (i.e. assignable to and from) all types.

> See also  
* typing module documentation  
* PEP 484 – Type Hints  
&emsp;&emsp;PEP written by Guido van Rossum, Jukka Lehtosalo, and Łukasz Langa; implemented by Guido van Rossum.
* PEP 483 – The Theory of Type Hints  
&emsp;&emsp;PEP written by Guido van Rossum

### PEP 471 - os.scandir() function – a better and faster directory iterator
PEP 471 adds a new directory iteration function, os.scandir(), to the standard library. Additionally, os.walk() is now implemented using scandir, which makes it 3 to 5 times faster on POSIX systems and 7 to 20 times faster on Windows systems. This is largely achieved by greatly reducing the number of calls to os.stat() required to walk a directory tree.

Additionally, scandir returns an iterator, as opposed to returning a list of file names, which improves memory efficiency when iterating over very large directories.

The following example shows a simple use of os.scandir() to display all the files (excluding directories) in the given path that don’t start with '.'. The entry.is_file() call will generally not make an additional system call:

```python
for entry in os.scandir(path):
    if not entry.name.startswith('.') and entry.is_file():
        print(entry.name)
```

> See also  
PEP 471 – os.scandir() function – a better and faster directory iterator  
&emsp;PEP written and implemented by Ben Hoyt with the help of Victor Stinner.  

### PEP 475: Retry system calls failing with EINTR
An errno.EINTR error code is returned whenever a system call, that is waiting for I/O, is interrupted by a signal. Previously, Python would raise InterruptedError in such cases. This meant that, when writing a Python application, the developer had two choices:

1. Ignore the `InterruptedError`.  
2. Handle the `InterruptedError` and attempt to restart the interrupted system call at every call site.

The first option makes an application fail intermittently. The second option adds a large amount of boilerplate that makes the code nearly unreadable. Compare:

`print("Hello World")`  

and:

```python
while True:
    try:
        print("Hello World")
        break
    except InterruptedError:
        continue
```
PEP 475 implements automatic retry of system calls on EINTR. This removes the burden of dealing with EINTR or InterruptedError in user code in most situations and makes Python programs, including the standard library, more robust. Note that the system call is only retried if the signal handler does not raise an exception.

Below is a list of functions which are now retried when interrupted by a signal:

* open() and io.open();
* functions of the faulthandler module;
* os functions: fchdir(), fchmod(), fchown(), fdatasync(), fstat(), fstatvfs(), fsync(), ftruncate(), mkfifo(), mknod(), open(), posix_fadvise(), posix_fallocate(), pread(), pwrite(), read(), readv(), sendfile(), wait3(), wait4(), wait(), waitid(), waitpid(), write(), writev();
* special cases: os.close() and os.dup2() now ignore EINTR errors; the syscall is not retried (see the PEP for the rationale);
* select functions: devpoll.poll(), epoll.poll(), kqueue.control(), poll.poll(), select();
* methods of the socket class: accept(), connect() (except for non-blocking sockets), recv(), recvfrom(), recvmsg(), send(), sendall(), sendmsg(), sendto();
* signal.sigtimedwait() and signal.sigwaitinfo();
* time.sleep().

> See also  
PEP 475 – Retry system calls failing with EINTR  
&emsp;PEP and implementation written by Charles-François Natali and Victor Stinner, with the help of Antoine Pitrou (the French connection).

### PEP 479: Change StopIteration handling inside generators
The interaction of generators and StopIteration in Python 3.4 and earlier was sometimes surprising, and could conceal obscure bugs. Previously, StopIteration raised accidentally inside a generator function was interpreted as the end of the iteration by the loop construct driving the generator.

PEP 479 changes the behavior of generators: when a `StopIteration` exception is raised inside a generator, it is replaced with a RuntimeError before it exits the generator frame. The main goal of this change is to ease debugging in the situation where an unguarded next() call raises `StopIteration` and causes the iteration controlled by the generator to terminate silently. This is particularly pernicious in combination with the `yield from` construct.

This is a backwards incompatible change, so to enable the new behavior, a __future__ import is necessary:

```shell
>>>
>>> from __future__ import generator_stop

>>> def gen():
...     next(iter([]))
...     yield
...
>>> next(gen())
Traceback (most recent call last):
  File "<stdin>", line 2, in gen
StopIteration

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
RuntimeError: generator raised StopIteration
```

Without a `__future__` import, a PendingDeprecationWarning will be raised whenever a StopIteration exception is raised inside a generator.

> See also  
PEP 479 – Change StopIteration handling inside generators  
&emsp;PEP written by Chris Angelico and Guido van Rossum. Implemented by Chris Angelico, Yury Selivanov and Nick Coghlan.

### PEP 485: A function for testing approximate equality
PEP 485 adds the math.isclose() and cmath.isclose() functions which tell whether two values are approximately equal or “close” to each other. Whether or not two values are considered close is determined according to given absolute and relative tolerances. Relative tolerance is the maximum allowed difference between isclose arguments, relative to the larger absolute value:

```shell
>>>
>>> import math
>>> a = 5.0
>>> b = 4.99998
>>> math.isclose(a, b, rel_tol=1e-5)
True
>>> math.isclose(a, b, rel_tol=1e-6)
False
```
It is also possible to compare two values using absolute tolerance, which must be a non-negative value:

```shell
>>>
>>> import math
>>> a = 5.0
>>> b = 4.99998
>>> math.isclose(a, b, abs_tol=0.00003)
True
>>> math.isclose(a, b, abs_tol=0.00001)
False
```

> See also  
PEP 485 – A function for testing approximate equality
&emsp;PEP written by Christopher Barker; implemented by Chris Barker and Tal Einat.

### PEP 486: Make the Python Launcher aware of virtual environments
PEP 486 makes the Windows launcher (see PEP 397) aware of an active virtual environment. When the default interpreter would be used and the VIRTUAL_ENV environment variable is set, the interpreter in the virtual environment will be used.

> See also  
PEP 486 – Make the Python Launcher aware of virtual environments  
&emsp;PEP written and implemented by Paul Moore.

### PEP 488: Elimination of PYO files
PEP 488 does away with the concept of .pyo files. This means that .pyc files represent both unoptimized and optimized bytecode. To prevent the need to constantly regenerate bytecode files, .pyc files now have an optional opt- tag in their name when the bytecode is optimized. This has the side-effect of no more bytecode file name clashes when running under either -O or -OO. Consequently, bytecode files generated from -O, and -OO may now exist simultaneously. importlib.util.cache_from_source() has an updated API to help with this change.

> See also  
PEP 488 – Elimination of PYO files  
&emsp;PEP written and implemented by Brett Cannon.

### PEP 489: Multi-phase extension module initialization
PEP 489 updates extension module initialization to take advantage of the two step module loading mechanism introduced by PEP 451 in Python 3.4.

This change brings the import semantics of extension modules that opt-in to using the new mechanism much closer to those of Python source and bytecode modules, including the ability to use any valid identifier as a module name, rather than being restricted to ASCII.

> See also  
PEP 489 – Multi-phase extension module initialization  
&emsp;PEP written by Petr Viktorin, Stefan Behnel, and Nick Coghlan; implemented by Petr Viktorin.
