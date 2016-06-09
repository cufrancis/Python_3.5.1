## Other Language Changes
Some smaller changes made to the core Python language are:

* Added the `"namereplace"` error handlers. The `"backslashreplace"` error handlers now work with decoding and translating. (Contributed by Serhiy Storchaka in issue 19676 and issue 22286.)
* The -b option now affects comparisons of bytes with int. (Contributed by Serhiy Storchaka in issue 23681.)
* New Kazakh `kz1048` and Tajik `koi8_t` codecs. (Contributed by Serhiy Storchaka in issue 22682 and issue 22681.)
* Property docstrings are now writable. This is especially useful for collections.namedtuple() docstrings. (Contributed by Berker Peksag in issue 24064.)
* Circular imports involving relative imports are now supported. (Contributed by Brett Cannon and Antoine Pitrou in issue 17636.)
