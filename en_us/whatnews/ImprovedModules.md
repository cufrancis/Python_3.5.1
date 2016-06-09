## Improved Modules
### argparse
The ArgumentParser class now allows disabling abbreviated usage of long options by setting allow_abbrev to `False`. (Contributed by Jonathan Paugh, Steven Bethard, paul j3 and Daniel Eriksson in [issue 14910](https://bugs.python.org/issue14910).)

### asyncio
Since the asyncio module is provisional, all changes introduced in Python 3.5 have also been backported to Python 3.4.x.

Notable changes in the asyncio module since Python 3.4.0:

* New debugging APIs: loop.set_debug() and loop.get_debug() methods. (Contributed by Victor Stinner.)  
* The proactor event loop now supports SSL. (Contributed by Antoine Pitrou and Victor Stinner in [issue 22560](https://bugs.python.org/issue22560).)  
* A new loop.is_closed() method to check if the event loop is closed. (Contributed by Victor Stinner in [issue 21326](https://bugs.python.org/issue21326).)  
* A new loop.create_task() to conveniently create and schedule a new Task for a coroutine. The `create_task` method is also * used by all asyncio functions that wrap coroutines into tasks, such as asyncio.wait(), asyncio.gather(), etc. (Contributed by Victor Stinner.)  
* A new transport.get_write_buffer_limits() method to inquire for high- and low- water limits of the flow control. (Contributed by Victor Stinner.)  
* The async() function is deprecated in favor of ensure_future(). (Contributed by Yury Selivanov.)  
* New loop.set_task_factory() and loop.set_task_factory() methods to customize the task factory that loop.create_task() method uses. (Contributed by Yury Selivanov.)  
* New Queue.join() and Queue.task_done() queue methods. (Contributed by Victor Stinner.)  
* The `JoinableQueue` class was removed, in favor of the asyncio.Queue class. (Contributed by Victor Stinner.)  

Updates in 3.5.1:

* The ensure_future() function and all functions that use it, such as loop.run_until_complete(), now accept all kinds of awaitable objects. (Contributed by Yury Selivanov.)  
* New run_coroutine_threadsafe() function to submit coroutines to event loops from other threads. (Contributed by Vincent Michel.)  
* New Transport.is_closing() method to check if the transport is closing or closed. (Contributed by Yury Selivanov.)  
* The loop.create_server() method can now accept a list of hosts. (Contributed by Yann Sionneau.)  

Updates in 3.5.2:

* New loop.create_future() method to create Future objects. This allows alternative event loop implementations, such as uvloop, to provide a faster asyncio.Future implementation. (Contributed by Yury Selivanov.)  
* New loop.get_exception_handler() method to get the current exception handler. (Contributed by Yury Selivanov.)  
* New StreamReader.readuntil() method to read data from the stream until a separator bytes sequence appears. (Contributed by Mark Korenberg.)  
* The loop.create_connection() and loop.create_server() methods are optimized to avoid calling the system getaddrinfo function if the address is already resolved. (Contributed by A. Jesse Jiryu Davis.)  
* The loop.sock_connect(sock, address) no longer requires the address to be resolved prior to the call. (Contributed by A. Jesse Jiryu Davis.)  

### bz2
The BZ2Decompressor.decompress method now accepts an optional max_length argument to limit the maximum size of decompressed data. (Contributed by Nikolaus Rath in [issue 15955](https://bugs.python.org/issue15955).)

### cgi
The FieldStorage class now supports the context manager protocol. (Contributed by Berker Peksag in [issue 20289](https://bugs.python.org/issue20289).)

### cmath
A new function isclose() provides a way to test for approximate equality. (Contributed by Chris Barker and Tal Einat in [issue 24270](https://bugs.python.org/issue24270).)

### code
The InteractiveInterpreter.showtraceback() method now prints the full chained traceback, just like the interactive interpreter. (Contributed by Claudiu Popa in [issue 17442](https://bugs.python.org/issue17442).)

### collections
The OrderedDict class is now implemented in C, which makes it 4 to 100 times faster. (Contributed by Eric Snow in [issue 16991](https://bugs.python.org/issue16991).)

OrderedDict.items(), OrderedDict.keys(), OrderedDict.values() views now support reversed() iteration. (Contributed by Serhiy Storchaka in [issue 19505](https://bugs.python.org/issue19505).)

The deque class now defines index(), insert(), and copy(), and supports the + and * operators. This allows deques to be recognized as a MutableSequence and improves their substitutability for lists. (Contributed by Raymond Hettinger in [issue 23704](https://bugs.python.org/issue23704).)

Docstrings produced by namedtuple() can now be updated:

```python
Point = namedtuple('Point', ['x', 'y'])
Point.__doc__ += ': Cartesian coodinate'
Point.x.__doc__ = 'abscissa'
Point.y.__doc__ = 'ordinate'
```
(Contributed by Berker Peksag in [issue 24064](https://bugs.python.org/issue24064).)

The UserString class now implements the \__getnewargs\__(), \__rmod\__(), casefold(), format_map(), isprintable(), and maketrans() methods to match the corresponding methods of str. (Contributed by Joe Jevnik in [issue 22189](https://bugs.python.org/issue22189).)

### collections.abc
The Sequence.index() method now accepts start and stop arguments to match the corresponding methods of tuple, list, etc. (Contributed by Devin Jeanpierre in [issue 23086](https://bugs.python.org/issue23086).)

A new Generator abstract base class. (Contributed by Stefan Behnel in [issue 24018](https://bugs.python.org/issue24018).)

New Awaitable, Coroutine, AsyncIterator, and AsyncIterable abstract base classes. (Contributed by Yury Selivanov in [issue 24184](https://bugs.python.org/issue24184).)

For earlier Python versions, a backport of the new ABCs is available in an external PyPI package.

### compileall
A new compileall option,` -j *N*`, allows running *N* workers simultaneously to perform parallel bytecode compilation. The compile_dir() function has a corresponding `workers` parameter. (Contributed by Claudiu Popa in [issue 16104](https://bugs.python.org/issue16104).)

Another new option, `-r`, allows controlling the maximum recursion level for subdirectories. (Contributed by Claudiu Popa in [issue 19628](https://bugs.python.org/issue19628).)

The `-q` command line option can now be specified more than once, in which case all output, including errors, will be suppressed. The corresponding `quiet` parameter in compile_dir(), compile_file(), and compile_path() can now accept an integer value indicating the level of output suppression. (Contributed by Thomas Kluyver in [issue 21338](https://bugs.python.org/issue21338).)

### concurrent.futures
The Executor.map() method now accepts a chunksize argument to allow batching of tasks to improve performance when ProcessPoolExecutor() is used. (Contributed by Dan O’Reilly in [issue 11271](https://bugs.python.org/issue11271).)

The number of workers in the ThreadPoolExecutor constructor is optional now. The default value is 5 times the number of CPUs. (Contributed by Claudiu Popa in [issue 21527](https://bugs.python.org/issue21527).)

### configparser
configparser now provides a way to customize the conversion of values by specifying a dictionary of converters in the ConfigParser constructor, or by defining them as methods in `ConfigParser` subclasses. Converters defined in a parser instance are inherited by its section proxies.

Example:

```python
>>>
>>> import configparser
>>> conv = {}
>>> conv['list'] = lambda v: [e.strip() for e in v.split() if e.strip()]
>>> cfg = configparser.ConfigParser(converters=conv)
>>> cfg.read_string("""
... [s]
... list = a b c d e f g
... """)
>>> cfg.get('s', 'list')
'a b c d e f g'
>>> cfg.getlist('s', 'list')
['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> section = cfg['s']
>>> section.getlist('list')
['a', 'b', 'c', 'd', 'e', 'f', 'g']
```
(Contributed by Łukasz Langa in [issue 18159](https://bugs.python.org/issue18159).)

### contextlib
The new redirect_stderr() context manager (similar to redirect_stdout()) makes it easier for utility scripts to handle inflexible APIs that write their output to sys.stderr and don’t provide any options to redirect it:

```python
>>>
>>> import contextlib, io, logging
>>> f = io.StringIO()
>>> with contextlib.redirect_stderr(f):
...     logging.warning('warning')
...
>>> f.getvalue()
'WARNING:root:warning\n'
```
(Contributed by Berker Peksag in [issue 22389](https://bugs.python.org/issue22389).)

### csv
The writerow() method now supports arbitrary iterables, not just sequences. (Contributed by Serhiy Storchaka in [issue 23171](https://bugs.python.org/issue23171).)

### curses
The new update_lines_cols() function updates the LINES and COLS environment variables. This is useful for detecting manual screen resizing. (Contributed by Arnon Yaari in [issue 4254](https://bugs.python.org/issue4254).)

### dbm
dumb.open always creates a new database when the flag has the value "n". (Contributed by Claudiu Popa in [issue 18039](https://bugs.python.org/issue18039).)

### difflib
The charset of HTML documents generated by HtmlDiff.make_file() can now be customized by using a new charset keyword-only argument. The default charset of HTML document changed from `"ISO-8859-1"` to `"utf-8"`. (Contributed by Berker Peksag in [issue 2052](https://bugs.python.org/issue2052).)

The diff_bytes() function can now compare lists of byte strings. This fixes a regression from Python 2. (Contributed by Terry J. Reedy and Greg Ward in [issue 17445](https://bugs.python.org/issue17445).)

### distutils
Both the `build` and `build_ext` commands now accept a `-j` option to enable parallel building of extension modules. (Contributed by Antoine Pitrou in [issue 5309](https://bugs.python.org/issue5309).)

The distutils module now supports xz compression, and can be enabled by passing `xztar` as an argument to `bdist --format.` (Contributed by Serhiy Storchaka in [issue 16314](https://bugs.python.org/issue16314).)

### doctest
The DocTestSuite() function returns an empty unittest.TestSuite if module contains no docstrings, instead of raising ValueError. (Contributed by Glenn Jones in [issue 15916](https://bugs.python.org/issue15916).)

### email
A new policy option Policy.mangle_from_ controls whether or not lines that start with `"From "` in email bodies are prefixed with a `">"` character by generators. The default is `True` for compat32 and `False` for all other policies. (Contributed by Milan Oberkirch in [issue 20098](https://bugs.python.org/issue20098).)

A new Message.get_content_disposition() method provides easy access to a canonical value for the Content-Disposition header. (Contributed by Abhilash Raj in [issue 21083](https://bugs.python.org/issue21083).)

A new policy option EmailPolicy.utf8 can be set to True to encode email headers using the UTF-8 charset instead of using encoded words. This allows Messages to be formatted according to [RFC 6532](https://tools.ietf.org/html/rfc6532.html) and used with an SMTP server that supports the [RFC 6531](https://tools.ietf.org/html/rfc6531.html) `SMTPUTF8` extension. (Contributed by R. David Murray in [issue 24211](https://bugs.python.org/issue24211).)

The mime.text.MIMEText constructor now accepts a charset.Charset instance. (Contributed by Claude Paroz and Berker Peksag in [issue 16324](https://bugs.python.org/issue24211).)

### enum
The Enum callable has a new parameter start to specify the initial number of enum values if only names are provided:

```python
>>>
>>> Animal = enum.Enum('Animal', 'cat dog', start=10)
>>> Animal.cat
<Animal.cat: 10>
>>> Animal.dog
<Animal.dog: 11>
```
(Contributed by Ethan Furman in [issue 21706](https://bugs.python.org/issue21706).)

### faulthandler
The enable(), register(), dump_traceback() and dump_traceback_later() functions now accept file descriptors in addition to file-like objects. (Contributed by Wei Wu in [issue 23566](https://bugs.python.org/issue23566).)

### functools
Most of the lru_cache() machinery is now implemented in C, making it significantly faster. (Contributed by Matt Joiner, Alexey Kachayev, and Serhiy Storchaka in [issue 14373](https://bugs.python.org/issue14373).)

### glob
The iglob() and glob() functions now support recursive search in subdirectories, using the `"**"` pattern. (Contributed by Serhiy Storchaka in [issue 13968](https://bugs.python.org/issue13968).)

### gzip
The mode argument of the GzipFile constructor now accepts "x" to request exclusive creation. (Contributed by Tim Heaney in [issue 19222](https://bugs.python.org/issue19222))

### heapq
Element comparison in merge() can now be customized by passing a key function in a new optional key keyword argument, and a new optional reverse keyword argument can be used to reverse element comparison:

```python
>>>
>>> import heapq
>>> a = ['9', '777', '55555']
>>> b = ['88', '6666']
>>> list(heapq.merge(a, b, key=len))
['9', '88', '777', '6666', '55555']
>>> list(heapq.merge(reversed(a), reversed(b), key=len, reverse=True))
['55555', '6666', '777', '88', '9']
```
(Contributed by Raymond Hettinger in [issue 13742](https://bugs.python.org/issue13742).)

### http
A new HTTPStatus enum that defines a set of HTTP status codes, reason phrases and long descriptions written in English. (Contributed by Demian Brecht in [issue 21793](https://bugs.python.org/issue21793).)

### http.client
HTTPConnection.getresponse() now raises a RemoteDisconnected exception when a remote server connection is closed unexpectedly. Additionally, if a ConnectionError (of which `RemoteDisconnected` is a subclass) is raised, the client socket is now closed automatically, and will reconnect on the next request:

```python
import http.client
conn = http.client.HTTPConnection('www.python.org')
for retries in range(3):
    try:
        conn.request('GET', '/')
        resp = conn.getresponse()
    except http.client.RemoteDisconnected:
        pass
```
(Contributed by Martin Panter in [issue 3566](https://bugs.python.org/issue3566).)

### idlelib and IDLE
Since idlelib implements the IDLE shell and editor and is not intended for import by other programs, it gets improvements with every release. See `Lib/idlelib/NEWS.txt` for a cumulative list of changes since 3.4.0, as well as changes made in future 3.5.x releases. This file is also available from the IDLE Help ‣ About IDLE dialog.

### imaplib
The IMAP4 class now supports the context manager protocol. When used in a with statement, the IMAP4 `LOGOUT` command will be called automatically at the end of the block. (Contributed by Tarek Ziadé and Serhiy Storchaka in [issue 4972](https://bugs.python.org/issue4972).)

The imaplib module now supports [RFC 5161](https://tools.ietf.org/html/rfc5161.html) (ENABLE Extension) and [RFC 6855](https://tools.ietf.org/html/rfc6855.html) (UTF-8 Support) via the IMAP4.enable() method. A new IMAP4.utf8_enabled attribute tracks whether or not [RFC 6855](https://tools.ietf.org/html/rfc6855.html) support is enabled. (Contributed by Milan Oberkirch, R. David Murray, and Maciej Szulik in [issue 21800](https://bugs.python.org/issue21800).)

The imaplib module now automatically encodes non-ASCII string usernames and passwords using UTF-8, as recommended by the RFCs. (Contributed by Milan Oberkirch in [issue 21800](https://bugs.python.org/issue21800).)

### imghdr
The what() function now recognizes the OpenEXR format (contributed by Martin Vignali and Claudiu Popa in [issue 20295](https://bugs.python.org/issue20295)), and the WebP format (contributed by Fabrice Aneche and Claudiu Popa in [issue 20197](https://bugs.python.org/issue20197).)

### importlib
The util.LazyLoader class allows for lazy loading of modules in applications where startup time is important. (Contributed by Brett Cannon in [issue 17621](https://bugs.python.org/issue17621).)

The abc.InspectLoader.source_to_code() method is now a static method. This makes it easier to initialize a module object with code compiled from a string by running `exec(code, module.__dict__)`. (Contributed by Brett Cannon in [issue 21156](https://bugs.python.org/issue21156).)

The new util.module_from_spec() function is now the preferred way to create a new module. As opposed to creating a types.ModuleType instance directly, this new function will set the various import-controlled attributes based on the passed-in spec object. (Contributed by Brett Cannon in [issue 20383](https://bugs.python.org/issue20383).)

### inspect
Both the Signature and Parameter classes are now picklable and hashable. (Contributed by Yury Selivanov in [issue 20726](https://bugs.python.org/issue20726) and [issue 20334](https://bugs.python.org/issue20334).)

A new BoundArguments.apply_defaults() method provides a way to set default values for missing arguments:

```python
>>>
>>> def foo(a, b='ham', *args): pass
>>> ba = inspect.signature(foo).bind('spam')
>>> ba.apply_defaults()
>>> ba.arguments
OrderedDict([('a', 'spam'), ('b', 'ham'), ('args', ())])
(Contributed by Yury Selivanov in [issue 24190](https://bugs.python.org/issue24190).)
```

A new class method Signature.from_callable() makes subclassing of Signature easier. (Contributed by Yury Selivanov and Eric Snow in [issue 17373](https://bugs.python.org/issue17373).)

The signature() function now accepts a follow_wrapped optional keyword argument, which, when set to `False`, disables automatic following of `__wrapped__` links. (Contributed by Yury Selivanov in [issue 20691](https://bugs.python.org/issue20691).)

A set of new functions to inspect coroutine functions and coroutine objects has been added: iscoroutine(), iscoroutinefunction(), isawaitable(), getcoroutinelocals(), and getcoroutinestate(). (Contributed by Yury Selivanov in [issue 24017 and issue 24400](https://bugs.python.org/issue24017 and issue 24400).)

The stack(), trace(), getouterframes(), and getinnerframes() functions now return a list of named tuples. (Contributed by Daniel Shahaf in [issue 16808](https://bugs.python.org/issue16808).)

### io
A new BufferedIOBase.readinto1() method, that uses at most one call to the underlying raw stream’s RawIOBase.read() or RawIOBase.readinto() methods. (Contributed by Nikolaus Rath in [issue 20578](https://bugs.python.org/issue20578).)

### ipaddress
Both the IPv4Network and IPv6Network classes now accept an (address, netmask) tuple argument, so as to easily construct network objects from existing addresses:

```python
>>>
>>> import ipaddress
>>> ipaddress.IPv4Network(('127.0.0.0', 8))
IPv4Network('127.0.0.0/8')
>>> ipaddress.IPv4Network(('127.0.0.0', '255.0.0.0'))
IPv4Network('127.0.0.0/8')
```

(Contributed by Peter Moody and Antoine Pitrou in [issue 16531](https://bugs.python.org/issue16531).)

A new reverse_pointer attribute for the IPv4Network and IPv6Network classes returns the name of the reverse DNS PTR record:

```python
>>>
>>> import ipaddress
>>> addr = ipaddress.IPv4Address('127.0.0.1')
>>> addr.reverse_pointer
'1.0.0.127.in-addr.arpa'
>>> addr6 = ipaddress.IPv6Address('::1')
>>> addr6.reverse_pointer
'1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa'
```

(Contributed by Leon Weber in [issue 20480](https://bugs.python.org/issue20480).)

### json
The json.tool command line interface now preserves the order of keys in JSON objects passed in input. The new `--sort-keys` option can be used to sort the keys alphabetically. (Contributed by Berker Peksag in [issue 21650](https://bugs.python.org/issue21650).)

JSON decoder now raises JSONDecodeError instead of ValueError to provide better context information about the error. (Contributed by Serhiy Storchaka in [issue 19361](https://bugs.python.org/issue19361).)

### linecache
A new lazycache() function can be used to capture information about a non-file-based module to permit getting its lines later via getline(). This avoids doing I/O until a line is actually needed, without having to carry the module globals around indefinitely. (Contributed by Robert Collins in [issue 17911](https://bugs.python.org/issue17911).)

### locale
A new delocalize() function can be used to convert a string into a normalized number string, taking the `LC_NUMERIC` settings into account:

```python
>>>
>>> import locale
>>> locale.setlocale(locale.LC_NUMERIC, 'de_DE.UTF-8')
'de_DE.UTF-8'
>>> locale.delocalize('1.234,56')
'1234.56'
>>> locale.setlocale(locale.LC_NUMERIC, 'en_US.UTF-8')
'en_US.UTF-8'
>>> locale.delocalize('1,234.56')
'1234.56'
```

(Contributed by Cédric Krier in [issue 13918](https://bugs.python.org/issue13918).)

### logging
All logging methods (Logger log(), exception(), critical(), debug(), etc.), now accept exception instances as an exc_info argument, in addition to boolean values and exception tuples:

```python
>>>
>>> import logging
>>> try:
...     1/0
... except ZeroDivisionError as ex:
...     logging.error('exception', exc_info=ex)
ERROR:root:exception
```

(Contributed by Yury Selivanov in [issue 20537](https://bugs.python.org/issue20537).)

The handlers.HTTPHandler class now accepts an optional ssl.SSLContext instance to configure SSL settings used in an HTTP connection. (Contributed by Alex Gaynor in [issue 22788](https://bugs.python.org/issue22788).)

The handlers.QueueListener class now takes a respect_handler_level keyword argument which, if set to True, will pass messages to handlers taking handler levels into account. (Contributed by Vinay Sajip.)

### lzma
The LZMADecompressor.decompress() method now accepts an optional max_length argument to limit the maximum size of decompressed data. (Contributed by Martin Panter in [issue 15955](https://bugs.python.org/issue15955).)

### math
Two new constants have been added to the math module: inf and nan. (Contributed by Mark Dickinson in [issue 23185](https://bugs.python.org/issue23185).)

A new function isclose() provides a way to test for approximate equality. (Contributed by Chris Barker and Tal Einat in [issue 24270](https://bugs.python.org/issue24270).)

A new gcd() function has been added. The fractions.gcd() function is now deprecated. (Contributed by Mark Dickinson and Serhiy Storchaka in [issue 22486](https://bugs.python.org/issue22486).)

### multiprocessing
sharedctypes.synchronized() objects now support the context manager protocol. (Contributed by Charles-François Natali in [issue 21565](https://bugs.python.org/issue21565).)

### operator
attrgetter(), itemgetter(), and methodcaller() objects now support pickling. (Contributed by Josh Rosenberg and Serhiy Storchaka in [issue 22955](https://bugs.python.org/issue22955).)

New matmul() and imatmul() functions to perform matrix multiplication. (Contributed by Benjamin Peterson in [issue 21176](https://bugs.python.org/issue21176).)

### os
The new scandir() function returning an iterator of DirEntry objects has been added. If possible, scandir() extracts file attributes while scanning a directory, removing the need to perform subsequent system calls to determine file type or attributes, which may significantly improve performance. (Contributed by Ben Hoyt with the help of Victor Stinner in [issue 22524](https://bugs.python.org/issue22524).)

On Windows, a new stat_result.st_file_attributes attribute is now available. It corresponds to the `dwFileAttributes` member of the `BY_HANDLE_FILE_INFORMATION` structure returned by `GetFileInformationByHandle().` (Contributed by Ben Hoyt in [issue 21719](https://bugs.python.org/issue21719).)

The urandom() function now uses the `getrandom()` syscall on Linux 3.17 or newer, and `getentropy()` on OpenBSD 5.6 and newer, removing the need to use `/dev/urandom` and avoiding failures due to potential file descriptor exhaustion. (Contributed by Victor Stinner in [issue 22181](https://bugs.python.org/issue22181).)

New get_blocking() and set_blocking() functions allow getting and setting a file descriptor’s blocking mode (O_NONBLOCK.) (Contributed by Victor Stinner in [issue 22054](https://bugs.python.org/issue22054).)

The truncate() and ftruncate() functions are now supported on Windows. (Contributed by Steve Dower in [issue 23668](https://bugs.python.org/issue23668).)

There is a new os.path.commonpath() function returning the longest common sub-path of each passed pathname. Unlike the os.path.commonprefix() function, it always returns a valid path:

```python
>>>
>>> os.path.commonprefix(['/usr/lib', '/usr/local/lib'])
'/usr/l'

>>> os.path.commonpath(['/usr/lib', '/usr/local/lib'])
'/usr'
```

(Contributed by Rafik Draoui and Serhiy Storchaka in [issue 10395](https://bugs.python.org/issue10395).)

### pathlib
The new Path.samefile() method can be used to check whether the path points to the same file as another path, which can be either another Path object, or a string:

```python
>>>
>>> import pathlib
>>> p1 = pathlib.Path('/etc/hosts')
>>> p2 = pathlib.Path('/etc/../etc/hosts')
>>> p1.samefile(p2)
True
```
(Contributed by Vajrasky Kok and Antoine Pitrou in [issue 19775](https://bugs.python.org/issue19775).)

The Path.mkdir() method now accepts a new optional exist_ok argument to match `mkdir -p` and os.makedirs() functionality. (Contributed by Berker Peksag in [issue 21539](https://bugs.python.org/issue21539).)

There is a new Path.expanduser() method to expand `~` and `~user` prefixes. (Contributed by Serhiy Storchaka and Claudiu Popa in [issue 19776](https://bugs.python.org/issue19776).)

A new Path.home() class method can be used to get a Path instance representing the user’s home directory. (Contributed by Victor Salgado and Mayank Tripathi in [issue 19777](https://bugs.python.org/issue19777).)

New Path.write_text(), Path.read_text(), Path.write_bytes(), Path.read_bytes() methods to simplify read/write operations on files.

The following code snippet will create or rewrite existing file `~/spam42`:

```python
>>>
>>> import pathlib
>>> p = pathlib.Path('~/spam42')
>>> p.expanduser().write_text('ham')
3
```
(Contributed by Christopher Welborn in [issue 20218](https://bugs.python.org/issue20218).)

### pickle
Nested objects, such as unbound methods or nested classes, can now be pickled using pickle protocols older than protocol version 4. Protocol version 4 already supports these cases. (Contributed by Serhiy Storchaka in [issue 23611](https://bugs.python.org/issue23611).)

### poplib
A new POP3.utf8() command enables [RFC 6856](https://tools.ietf.org/html/rfc6856.html) (Internationalized Email) support, if a POP server supports it. (Contributed by Milan OberKirch in [issue 21804](https://bugs.python.org/issue21804).)

### re
References and conditional references to groups with fixed length are now allowed in lookbehind assertions:

```python
>>>
>>> import re
>>> pat = re.compile(r'(a|b).(?<=\1)c')
>>> pat.match('aac')
<_sre.SRE_Match object; span=(0, 3), match='aac'>
>>> pat.match('bbc')
<_sre.SRE_Match object; span=(0, 3), match='bbc'>
```
(Contributed by Serhiy Storchaka in [issue 9179](https://bugs.python.org/issue9179).)

The number of capturing groups in regular expressions is no longer limited to 100. (Contributed by Serhiy Storchaka in [issue 22437](https://bugs.python.org/issue22437).)

The sub() and subn() functions now replace unmatched groups with empty strings instead of raising an exception. (Contributed by Serhiy Storchaka in [issue 1519638](https://bugs.python.org/issue1519638).)

The re.error exceptions have new attributes, msg, pattern, pos, lineno, and colno, that provide better context information about the error:

```python
>>>
>>> re.compile("""
...     (?x)
...     .++
... """)
Traceback (most recent call last):
   ...
sre_constants.error: multiple repeat at position 16 (line 3, column 7)
```
(Contributed by Serhiy Storchaka in [issue 22578](https://bugs.python.org/issue22578).)

### readline
A new append_history_file() function can be used to append the specified number of trailing elements in history to the given file. (Contributed by Bruno Cauet in [issue 22940](https://bugs.python.org/issue22940).)

### selectors
The new DevpollSelector supports efficient `/dev/poll` polling on Solaris. (Contributed by Giampaolo Rodola’ in [issue 18931](https://bugs.python.org/issue18931).)

### shutil
The move() function now accepts a copy_function argument, allowing, for example, the copy() function to be used instead of the default copy2() if there is a need to ignore file metadata when moving. (Contributed by Claudiu Popa in [issue 19840](https://bugs.python.org/issue19840).)

The make_archive() function now supports the xztar format. (Contributed by Serhiy Storchaka in [issue 5411](https://bugs.python.org/issue5411).)

### signal
On Windows, the set_wakeup_fd() function now also supports socket handles. (Contributed by Victor Stinner in [issue 22018](https://bugs.python.org/issue22018).)

Various `SIG*` constants in the signal module have been converted into Enums. This allows meaningful names to be printed during debugging, instead of integer “magic numbers”. (Contributed by Giampaolo Rodola’ in [issue 21076](https://bugs.python.org/issue21076).)

### smtpd
Both the SMTPServer and SMTPChannel classes now accept a decode_data keyword argument to determine if the `DATA` portion of the SMTP transaction is decoded using the `"utf-8"` codec or is instead provided to the SMTPServer.process_message() method as a byte string. The default is `True` for backward compatibility reasons, but will change to False in Python 3.6. If decode_data is set to `False`, the process_message method must be prepared to accept keyword arguments. (Contributed by Maciej Szulik in [issue 19662](https://bugs.python.org/issue19662).)

The SMTPServer class now advertises the 8BITMIME extension ([RFC 6152](https://tools.ietf.org/html/rfc6152.html)) if decode_data has been set True. If the client specifies BODY=8BITMIME on the MAIL command, it is passed to SMTPServer.process_message() via the mail_options keyword. (Contributed by Milan Oberkirch and R. David Murray in [issue 21795](https://bugs.python.org/issue21795).)

The SMTPServer class now also supports the SMTPUTF8 extension ([RFC 6531](https://tools.ietf.org/html/rfc6531.html): Internationalized Email). If the client specified SMTPUTF8 BODY=8BITMIME on the MAIL command, they are passed to SMTPServer.process_message() via the mail_options keyword. It is the responsibility of the process_message method to correctly handle the SMTPUTF8 data. (Contributed by Milan Oberkirch in [issue 21725](https://bugs.python.org/issue21725).)

It is now possible to provide, directly or via name resolution, IPv6 addresses in the SMTPServer constructor, and have it successfully connect. (Contributed by Milan Oberkirch in [issue 14758](https://bugs.python.org/issue14758).)

### smtplib
A new SMTP.auth() method provides a convenient way to implement custom authentication mechanisms. (Contributed by Milan Oberkirch in [issue 15014](https://bugs.python.org/issue15014).)

The SMTP.set_debuglevel() method now accepts an additional debuglevel (2), which enables timestamps in debug messages. (Contributed by Gavin Chappell and Maciej Szulik in [issue 16914](https://bugs.python.org/issue16914).)

Both the SMTP.sendmail() and SMTP.send_message() methods now support [RFC 6531](https://tools.ietf.org/html/rfc6531.html) (SMTPUTF8). (Contributed by Milan Oberkirch and R. David Murray in [issue 22027](https://bugs.python.org/issue22027).)

### sndhdr
The what() and whathdr() functions now return a namedtuple(). (Contributed by Claudiu Popa in [issue 18615](https://bugs.python.org/issue18615).)

### socket
Functions with timeouts now use a monotonic clock, instead of a system clock. (Contributed by Victor Stinner in [issue 22043](https://bugs.python.org/issue22043).)

A new socket.sendfile() method allows sending a file over a socket by using the high-performance os.sendfile() function on UNIX, resulting in uploads being from 2 to 3 times faster than when using plain socket.send(). (Contributed by Giampaolo Rodola’ in [issue 17552](https://bugs.python.org/issue17552).)

The socket.sendall() method no longer resets the socket timeout every time bytes are received or sent. The socket timeout is now the maximum total duration to send all data. (Contributed by Victor Stinner in [issue 23853](https://bugs.python.org/issue23853).)

The backlog argument of the socket.listen() method is now optional. By default it is set to SOMAXCONN or to 128, whichever is less. (Contributed by Charles-François Natali in [issue 21455](https://bugs.python.org/issue21455).)

### ssl
### Memory BIO Support
(Contributed by Geert Jansen in [issue 21965](https://bugs.python.org/issue21965).)

The new SSLObject class has been added to provide SSL protocol support for cases when the network I/O capabilities of SSLSocket are not necessary or are suboptimal. `SSLObject` represents an SSL protocol instance, but does not implement any network I/O methods, and instead provides a memory buffer interface. The new MemoryBIO class can be used to pass data between Python and an SSL protocol instance.

The memory BIO SSL support is primarily intended to be used in frameworks implementing asynchronous I/O for which SSLSocket‘s readiness model (“select/poll”) is inefficient.

A new SSLContext.wrap_bio() method can be used to create a new SSLObject instance.

### Application-Layer Protocol Negotiation Support
(Contributed by Benjamin Peterson in [issue 20188](https://bugs.python.org/issue20188).)

Where OpenSSL support is present, the ssl module now implements the Application-Layer Protocol Negotiation TLS extension as described in [RFC 7301](https://tools.ietf.org/html/rfc7301.html).

The new SSLContext.set_alpn_protocols() can be used to specify which protocols a socket should advertise during the TLS handshake.

The new SSLSocket.selected_alpn_protocol() returns the protocol that was selected during the TLS handshake. The HAS_ALPN flag indicates whether ALPN support is present.

### Other Changes
There is a new SSLSocket.version() method to query the actual protocol version in use. (Contributed by Antoine Pitrou in [issue 20421](https://bugs.python.org/issue20421).)

The SSLSocket class now implements a SSLSocket.sendfile() method. (Contributed by Giampaolo Rodola’ in [issue 17552](https://bugs.python.org/issue17552).)

The SSLSocket.send() method now raises either the ssl.SSLWantReadError or ssl.SSLWantWriteError exception on a non-blocking socket if the operation would block. Previously, it would return 0. (Contributed by Nikolaus Rath in [issue 20951](https://bugs.python.org/issue20951).)

The cert_time_to_seconds() function now interprets the input time as UTC and not as local time, per [RFC 5280](https://tools.ietf.org/html/rfc5280.html). Additionally, the return value is always an int. (Contributed by Akira Li in [issue 19940](https://bugs.python.org/issue19940).)

New SSLObject.shared_ciphers() and SSLSocket.shared_ciphers() methods return the list of ciphers sent by the client during the handshake. (Contributed by Benjamin Peterson in [issue 23186](https://bugs.python.org/issue23186).)

The SSLSocket.do_handshake(), SSLSocket.read(), SSLSocket.shutdown(), and SSLSocket.write() methods of the SSLSocket class no longer reset the socket timeout every time bytes are received or sent. The socket timeout is now the maximum total duration of the method. (Contributed by Victor Stinner in [issue 23853](https://bugs.python.org/issue23853).)

The match_hostname() function now supports matching of IP addresses. (Contributed by Antoine Pitrou in [issue 23239](https://bugs.python.org/issue23239).)

### sqlite3
The Row class now fully supports the sequence protocol, in particular reversed() iteration and slice indexing. (Contributed by Claudiu Popa in [issue 10203](https://bugs.python.org/issue10203); by Lucas Sinclair, Jessica McKellar, and Serhiy Storchaka in [issue 13583](https://bugs.python.org/issue13583).)

### subprocess
The new run() function has been added. It runs the specified command and returns a CompletedProcess object, which describes a finished process. The new API is more consistent and is the recommended approach to invoking subprocesses in Python code that does not need to maintain compatibility with earlier Python versions. (Contributed by Thomas Kluyver in [issue 23342](https://bugs.python.org/issue23342).)

Examples:

```python
>>>
>>> subprocess.run(["ls", "-l"])  # doesn't capture output
CompletedProcess(args=['ls', '-l'], returncode=0)

>>> subprocess.run("exit 1", shell=True, check=True)
Traceback (most recent call last):
  ...
subprocess.CalledProcessError: Command 'exit 1' returned non-zero exit status 1

>>> subprocess.run(["ls", "-l", "/dev/null"], stdout=subprocess.PIPE)
CompletedProcess(args=['ls', '-l', '/dev/null'], returncode=0,
stdout=b'crw-rw-rw- 1 root root 1, 3 Jan 23 16:23 /dev/null\n')
```

### sys
A new set_coroutine_wrapper() function allows setting a global hook that will be called whenever a coroutine object is created by an async def function. A corresponding get_coroutine_wrapper() can be used to obtain a currently set wrapper. Both functions are provisional, and are intended for debugging purposes only. (Contributed by Yury Selivanov in [issue 24017](https://bugs.python.org/issue24017).)

A new is_finalizing() function can be used to check if the Python interpreter is shutting down. (Contributed by Antoine Pitrou in[issue 22696](https://bugs.python.org/issue22696).)

### sysconfig
The name of the user scripts directory on Windows now includes the first two components of the Python version. (Contributed by Paul Moore in[issue 23437](https://bugs.python.org/issue23437).)

### tarfile
The mode argument of the open() function now accepts `"x"` to request exclusive creation. (Contributed by Berker Peksag in[issue 21717](https://bugs.python.org/issue21717).)

The TarFile.extractall() and TarFile.extract() methods now take a keyword argument numeric_only. If set to `True`, the extracted files and directories will be owned by the numeric uid and gid from the tarfile. If set to `False` (the default, and the behavior in versions prior to 3.5), they will be owned by the named user and group in the tarfile. (Contributed by Michael Vogt and Eric Smith in [issue 23193](https://bugs.python.org/issue23193).)

The TarFile.list() now accepts an optional members keyword argument that can be set to a subset of the list returned by TarFile.getmembers(). (Contributed by Serhiy Storchaka in [issue 21549](https://bugs.python.org/issue21549).)

### threading
Both the Lock.acquire() and RLock.acquire() methods now use a monotonic clock for timeout management. (Contributed by Victor Stinner in [issue 22043](https://bugs.python.org/issue22043).)

### time
The monotonic() function is now always available. (Contributed by Victor Stinner in [issue 22043](https://bugs.python.org/issue22043).)

### timeit
A new command line option `-u` or `--unit=U` can be used to specify the time unit for the timer output. Supported options are `usec`, `msec`, or `sec`. (Contributed by Julian Gindi in [issue 18983](https://bugs.python.org/issue18983).)

The timeit() function has a new globals parameter for specifying the namespace in which the code will be running. (Contributed by Ben Roberts in [issue 2527](https://bugs.python.org/issue2527).)

### tkinter
The `tkinter._fix` module used for setting up the Tcl/Tk environment on Windows has been replaced by a private function in the `_tkinter` module which makes no permanent changes to environment variables. (Contributed by Zachary Ware in [issue 20035](https://bugs.python.org/issue20035).)

### traceback
New walk_stack() and walk_tb() functions to conveniently traverse frame and traceback objects. (Contributed by Robert Collins in [issue 17911](https://bugs.python.org/issue17911).)

New lightweight classes: TracebackException, StackSummary, and FrameSummary. (Contributed by Robert Collins in [issue 17911](https://bugs.python.org/issue17911).)

Both the print_tb() and print_stack() functions now support negative values for the limit argument. (Contributed by Dmitry Kazakov in [issue 22619](https://bugs.python.org/issue22619).)

### types
A new coroutine() function to transform generator and generator-like objects into awaitables. (Contributed by Yury Selivanov in [issue 24017](https://bugs.python.org/issue24017).)

A new type called CoroutineType, which is used for coroutine objects created by async def functions. (Contributed by Yury Selivanov in [issue 24400](https://bugs.python.org/issue24400).)

### unicodedata
The unicodedata module now uses data from Unicode 8.0.0.

### unittest
The TestLoader.loadTestsFromModule() method now accepts a keyword-only argument pattern which is passed to load_tests as the third argument. Found packages are now checked for load_tests regardless of whether their path matches pattern, because it is impossible for a package name to match the default pattern. (Contributed by Robert Collins and Barry A. Warsaw in [issue 16662](https://bugs.python.org/issue16662).)

Unittest discovery errors now are exposed in the TestLoader.errors attribute of the TestLoader instance. (Contributed by Robert Collins in [issue 19746](https://bugs.python.org/issue19746).)

A new command line option --locals to show local variables in tracebacks. (Contributed by Robert Collins in [issue 22936](https://bugs.python.org/issue22936).)

### unittest.mock
The Mock class has the following improvements:

The class constructor has a new unsafe parameter, which causes mock objects to raise AttributeError on attribute names starting with "assert". (Contributed by Kushal Das in [issue 21238](https://bugs.python.org/issue21238).)
A new Mock.assert_not_called() method to check if the mock object was called. (Contributed by Kushal Das in [issue 21262](https://bugs.python.org/issue21262).)
The MagicMock class now supports __truediv__(), __divmod__() and __matmul__() operators. (Contributed by Johannes Baiter in [issue 20968, and Håkan Lövdahl in issue 23581 and issue 23568](https://bugs.python.org/issue20968, and Håkan Lövdahl in issue 23581 and issue 23568).)

It is no longer necessary to explicitly pass create=True to the patch() function when patching builtin names. (Contributed by Kushal Das in [issue 17660](https://bugs.python.org/issue17660).)

### urllib
A new request.HTTPPasswordMgrWithPriorAuth class allows HTTP Basic Authentication credentials to be managed so as to eliminate unnecessary 401 response handling, or to unconditionally send credentials on the first request in order to communicate with servers that return a 404 response instead of a 401 if the Authorization header is not sent. (Contributed by Matej Cepl in [issue 19494 and Akshit Khurana in issue 7159](https://bugs.python.org/issue19494 and Akshit Khurana in issue 7159).)

A new quote_via argument for the parse.urlencode() function provides a way to control the encoding of query parts if needed. (Contributed by Samwyse and Arnon Yaari in [issue 13866](https://bugs.python.org/issue13866).)

The request.urlopen() function accepts an ssl.SSLContext object as a context argument, which will be used for the HTTPS connection. (Contributed by Alex Gaynor in [issue 22366](https://bugs.python.org/issue22366).)

The parse.urljoin() was updated to use the [RFC 3986](https://tools.ietf.org/html/rfc3986.html) semantics for the resolution of relative URLs, rather than [RFC 1808](https://tools.ietf.org/html/rfc1808.html) and [RFC 2396](https://tools.ietf.org/html/rfc2396.html). (Contributed by Demian Brecht and Senthil Kumaran in [issue 22118](https://bugs.python.org/issue22118).)

### wsgiref
The headers argument of the headers.Headers class constructor is now optional. (Contributed by Pablo Torres Navarrete and SilentGhost in [issue 5800](https://bugs.python.org/issue5800).)

### xmlrpc
The client.ServerProxy class now supports the context manager protocol. (Contributed by Claudiu Popa in [issue 20627](https://bugs.python.org/issue20627).)

The client.ServerProxy constructor now accepts an optional ssl.SSLContext instance. (Contributed by Alex Gaynor in [issue 22960](https://bugs.python.org/issue22960).)

### xml.sax
SAX parsers now support a character stream of the xmlreader.InputSource object. (Contributed by Serhiy Storchaka in [issue 2175](https://bugs.python.org/issue2175).)

parseString() now accepts a str instance. (Contributed by Serhiy Storchaka in [issue 10590](https://bugs.python.org/issue10590).)

### zipfile
ZIP output can now be written to unseekable streams. (Contributed by Serhiy Storchaka in [issue 23252](https://bugs.python.org/issue23252).)

The mode argument of ZipFile.open() method now accepts "x" to request exclusive creation. (Contributed by Serhiy Storchaka in [issue 21717](https://bugs.python.org/issue21717).)
