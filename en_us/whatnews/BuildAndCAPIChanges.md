## Build and C API Changes
New calloc functions were added:

* PyMem_RawCalloc(),
* PyMem_Calloc(),
* PyObject_Calloc(),
* \_PyObject_GC_Calloc().
(Contributed by Victor Stinner in [issue 21233](https://bugs.python.org/issue21233).)

New encoding/decoding helper functions:

* Py_DecodeLocale() (replaced `_Py_char2wchar()`),
* Py_EncodeLocale() (replaced `_Py_wchar2char()`).
(Contributed by Victor Stinner in [issue 18395](https://bugs.python.org/issue18395).)

A new PyCodec_NameReplaceErrors() function to replace the unicode encode error with `\N{...}` escapes. (Contributed by Serhiy Storchaka in [issue 19676](https://bugs.python.org/issue19676).)

A new PyErr_FormatV() function similar to PyErr_Format(), but accepts a `va_list` argument. (Contributed by Antoine Pitrou in [issue 18711](https://bugs.python.org/issue18711).)

A new PyExc_RecursionError exception. (Contributed by Georg Brandl in [issue 19235](https://bugs.python.org/issue19235).)

New PyModule_FromDefAndSpec(), PyModule_FromDefAndSpec2(), and PyModule_ExecDef() functions introduced by [PEP 489](https://www.python.org/dev/peps/pep-0489) – multi-phase extension module initialization. (Contributed by Petr Viktorin in [issue 24268](https://bugs.python.org/issue24268).)

New PyNumber_MatrixMultiply() and PyNumber_InPlaceMatrixMultiply() functions to perform matrix multiplication. (Contributed by Benjamin Peterson in [issue 21176](https://bugs.python.org/issue21176). See also [PEP 465](https://www.python.org/dev/peps/pep-0465) for details.)

The PyTypeObject.tp_finalize slot is now part of the stable ABI.

Windows builds now require Microsoft Visual C++ 14.0, which is available as part of [Visual Studio 2015](https://www.visualstudio.com/).

Extension modules now include a platform information tag in their filename on some platforms (the tag is optional, and CPython will import extensions without it, although if the tag is present and mismatched, the extension won’t be loaded):

* On Linux, extension module filenames end with `.cpython-<major><minor>m-<architecture>-<os>.pyd`:
  * `<major>` is the major number of the Python version; for Python 3.5 this is 3.
  * `<minor>` is the minor number of the Python version; for Python 3.5 this is 5.
  * `<architecture>` is the hardware architecture the extension module was built to run on. It’s most commonly either `i386` for 32-bit Intel platforms or `x86_64` for 64-bit Intel (and AMD) platforms.
  * `<os>` is always `linux-gnu`, except for extensions built to talk to the 32-bit ABI on 64-bit platforms, in which case it is `linux-gnu32` (and `<architecture>` will be x86_64).
* On Windows, extension module filenames end with `<debug>.cp<major><minor>-<platform>.pyd`:
  * `<major>` is the major number of the Python version; for Python 3.5 this is 3.
  * `<minor>` is the minor number of the Python version; for Python 3.5 this is 5.
  * `<platform>` is the platform the extension module was built for, either win32 for Win32, `win_amd64` for Win64, `win_ia64` for Windows Itanium 64, and `win_arm` for Windows on ARM.
  * If built in debug mode, `<debug>` will be `_d`, otherwise it will be blank.
* On OS X platforms, extension module filenames now end with `-darwin.so.`
* On all other platforms, extension module filenames are the same as they were with Python 3.4.
