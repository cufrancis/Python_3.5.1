## New Modules
### typing
The new typing provisional module provides standard definitions and tools for function type annotations. See Type Hints for more information.

### zipapp
The new zipapp module (specified in PEP 441) provides an API and command line tool for creating executable Python Zip Applications, which were introduced in Python 2.6 in issue 1739468, but which were not well publicized, either at the time or since.

With the new module, bundling your application is as simple as putting all the files, including a `__main__.py `file, into a directory `myapp` and running:

```python
$ python -m zipapp myapp
$ python myapp.pyz
```

The module implementation has been contributed by Paul Moore in issue 23491.

> See also PEP 441 – Improving Python ZIP Application Support
