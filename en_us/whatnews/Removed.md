## Removed
### API and Feature Removals
The following obsolete and previously deprecated APIs and features have been removed:

The `__version__` attribute has been dropped from the email package. The email code hasn’t been shipped separately from the stdlib for a long time, and the `__version__` string was not updated in the last few releases.
The internal `Netrc` class in the ftplib module was deprecated in 3.4, and has now been removed. (Contributed by Matt Chaput in [issue 6623](https://bugs.python.org/issue6623).)
The concept of `.pyo` files has been removed.
The JoinableQueue class in the provisional asyncio module was deprecated in 3.4.4 and is now removed. (Contributed by A. Jesse Jiryu Davis in [issue 23464](https://bugs.python.org/issue23464).)
