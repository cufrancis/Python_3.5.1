## Deprecated
### New Keywords
`async` and `await` are not recommended to be used as variable, class, function or module names. Introduced by [PEP 492](https://www.python.org/dev/peps/pep-0492) in Python 3.5, they will become proper keywords in Python 3.7.

### Deprecated Python Behavior
Raising the StopIteration exception inside a generator will now generate a silent PendingDeprecationWarning, which will become a non-silent deprecation warning in Python 3.6 and will trigger a RuntimeError in Python 3.7. See [PEP 479](https://www.python.org/dev/peps/pep-0479): Change StopIteration handling inside generators for details.

### Unsupported Operating Systems
Windows XP is no longer supported by Microsoft, thus, per [PEP 11](https://www.python.org/dev/peps/pep-0011), CPython 3.5 is no longer officially supported on this OS.

### Deprecated Python modules, functions and methods
The formatter module has now graduated to full deprecation and is still slated for removal in Python 3.6.

The asyncio.async() function is deprecated in favor of ensure_future().

The smtpd module has in the past always decoded the DATA portion of email messages using the utf-8 codec. This can now be controlled by the new decode_data keyword to SMTPServer. The default value is True, but this default is deprecated. Specify the decode_data keyword with an appropriate value to avoid the deprecation warning.

Directly assigning values to the key, value and coded_value of http.cookies.Morsel objects is deprecated. Use the set() method instead. In addition, the undocumented LegalChars parameter of set() is deprecated, and is now ignored.

Passing a format string as keyword argument format_string to the format() method of the string.Formatter class has been deprecated. (Contributed by Serhiy Storchaka in [issue 23671](https://bugs.python.org/issue23671).)

The platform.dist() and platform.linux_distribution() functions are now deprecated. Linux distributions use too many different ways of describing themselves, so the functionality is left to a package. (Contributed by Vajrasky Kok and Berker Peksag in [issue 1322](https://bugs.python.org/issue1322).)

The previously undocumented from_function and from_builtin methods of inspect.Signature are deprecated. Use the new Signature.from_callable() method instead. (Contributed by Yury Selivanov in [issue 24248](https://bugs.python.org/issue24248).)

The inspect.getargspec() function is deprecated and scheduled to be removed in Python 3.6. (See [issue 20438](https://bugs.python.org/issue20438) for details.)

The inspect getfullargspec(), getargvalues(), getcallargs(), getargvalues(), formatargspec(), and formatargvalues() functions are deprecated in favor of the inspect.signature() API. (Contributed by Yury Selivanov in [issue 20438](https://bugs.python.org/issue20438).)

Use of re.LOCALE flag with str patterns or re.ASCII is now deprecated. (Contributed by Serhiy Storchaka in [issue 22407](https://bugs.python.org/issue22407).)

Use of unrecognized special sequences consisting of '\' and an ASCII letter in regular expression patterns and replacement patterns now raises a deprecation warning and will be forbidden in Python 3.6. (Contributed by Serhiy Storchaka in [issue 23622](https://bugs.python.org/issue23622).)

The undocumented and unofficial use_load_tests default argument of the unittest.TestLoader.loadTestsFromModule() method now is deprecated and ignored. (Contributed by Robert Collins and Barry A. Warsaw in [issue 16662](https://bugs.python.org/issue16662).)
