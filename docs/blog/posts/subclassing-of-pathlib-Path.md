---
date: 2025-10-13
categories:
  - Python
---

# Subclassing of `pathlib.Path`

In my case, I wanted to enhance the functionality of `pathlib.Path` to include additional methods for file or directory operations specific to my project. 

For example, I wanted to add a method that would allow me to easily zip a directory or a file directly from the `Path` object:

```python
from pathlib import Path

class EnhancePath(Path):
    def zip(self):
        print('zip file')

EnhancePath.cwd()
```

This code works normally on python `3.13`, but it raises an error on python `3.11` and below (I don't test on `3.12`):

> AttributeError: type object 'EnhancePath' has no attribute '_flavour'

<!-- more -->

## Solution

Read the source code of `pathlib.Path` on python `3.11`:

```python title="python3.11 pathlib" hl_lines="3 4 15"
class Path(PurePath):
    def __new__(cls, *args, **kwargs):
        if cls is Path:
            cls = WindowsPath if os.name == 'nt' else PosixPath
        self = cls._from_parts(args)
        if not self._flavour.is_supported:
            raise NotImplementedError("cannot instantiate %r on your system"
                                      % (cls.__name__,))
        return self

class PosixPath(Path, PurePosixPath):
    ...

class PurePosixPath(PurePath):
    _flavour = _posix_flavour
```

`__new__` implements factory function functionality, which means that when you use `Path`, 
it will return either a `PosixPath` or a `WindowsPath` object depending on the operating system.

When we subclass `Path`, the `__new__` method is inherited, and since `cls` is not `pathlib.Path`,
In the previous example, `cls` is `__main__.EnhancePath`, so it will not change `cls` to `PosixPath` or `WindowsPath`.

So, we need to optimize our code like this:

```python
import sys
from pathlib import Path

class EnhancePath(Path if sys.version_info >= (3, 13) else type(Path())):
    def zip(self):
        print('zip file')

EnhancePath.cwd()
```

`type(Path())` will return `PosixPath` or `WindowsPath` depending on the operating system,
