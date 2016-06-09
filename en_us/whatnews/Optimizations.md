## Optimizations

The os.walk() function has been sped up by 3 to 5 times on POSIX systems, and by 7 to 20 times on Windows. This was done using the new os.scandir() function, which exposes file information from the underlying readdir or FindFirstFile/FindNextFile system calls. (Contributed by Ben Hoyt with help from Victor Stinner in [issue 23605](https://bugs.python.org/issue23605).)

Construction of bytes(int) (filled by zero bytes) is faster and uses less memory for large objects. calloc() is used instead of malloc() to allocate memory for these objects. (Contributed by Victor Stinner in [issue 21233](https://bugs.python.org/issue21233).)

Some operations on ipaddress IPv4Network and IPv6Network have been massively sped up, such as subnets(), supernet(), summarize_address_range(), collapse_addresses(). The speed up can range from 3 to 15 times. (Contributed by Antoine Pitrou, Michel Albert, and Markus in [issue 21486](https://bugs.python.org/issue21486), [issue 21487](https://bugs.python.org/issue21487), [issue 20826](https://bugs.python.org/issue20826), [issue 23266](https://bugs.python.org/issue23266).)

Pickling of ipaddress objects was optimized to produce significantly smaller output. (Contributed by Serhiy Storchaka in [issue 23133](https://bugs.python.org/issue23133).)

Many operations on io.BytesIO are now 50% to 100% faster. (Contributed by Serhiy Storchaka in [issue 15381](https://bugs.python.org/issue15381) and David Wilson in [issue 22003](https://bugs.python.org/issue22003).)

The marshal.dumps() function is now faster: 65-85% with versions 3 and 4, 20-25% with versions 0 to 2 on typical data, and up to 5 times in best cases. (Contributed by Serhiy Storchaka in [issue 20416](https://bugs.python.org/issue20416) and [issue 23344](https://bugs.python.org/issue23344).)

The UTF-32 encoder is now 3 to 7 times faster. (Contributed by Serhiy Storchaka in [issue 15027](https://bugs.python.org/issue15027).)

Regular expressions are now parsed up to 10% faster. (Contributed by Serhiy Storchaka in [issue 19380](https://bugs.python.org/issue19380).)

The json.dumps() function was optimized to run with ensure_ascii=False as fast as with ensure_ascii=True. (Contributed by Naoki Inada in [issue 23206](https://bugs.python.org/issue23206).)

The PyObject_IsInstance() and PyObject_IsSubclass() functions have been sped up in the common case that the second argument has type as its metaclass. (Contributed Georg Brandl by in [issue 22540](https://bugs.python.org/issue22540).)

Method caching was slightly improved, yielding up to 5% performance improvement in some benchmarks. (Contributed by Antoine Pitrou in [issue 22847](https://bugs.python.org/issue22847).)

Objects from the random module now use 50% less memory on 64-bit builds. (Contributed by Serhiy Storchaka in [issue 23488](https://bugs.python.org/issue23488).)

The property() getter calls are up to 25% faster. (Contributed by Joe Jevnik in [issue 23910](https://bugs.python.org/issue23910).)

Instantiation of fractions.Fraction is now up to 30% faster. (Contributed by Stefan Behnel in [issue 22464](https://bugs.python.org/issue22464).)

String methods find(), rfind(), split(), partition() and the in string operator are now significantly faster for searching 1-character substrings. (Contributed by Serhiy Storchaka in [issue 23573](https://bugs.python.org/issue23573).)
