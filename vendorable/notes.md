# Vendorable

This project is inspired by some handy Python modules that may feel a bit small to
add a runtime dependency for. In this case, the developer needs to decide whether to
encourage vendoring or, more recently, making itself available as a build-time
dependency.

As a pair of examples, [versioneer](https://github.com/python-versioneer/python-versioneer/)
and [setuptools\_scm](https://github.com/pypa/setuptools_scm) illustrate this nicely.
Versioneer predates PEP-518 `build-requires` dependencies, so until recently needed to
be vendored as a tool that sat alongside `setup.py`.
setuptools\_scm is a build-time dependency that also has a `python -m setuptools_scm` mode.

Vendorable would be a library for a tool that wants to make itself importable,
vendorable as a command-line option, or as a build-time requirement that vendors itself
when the package is built.

## API

`pkg/__init__.py`:

```python
try:
    from . import _vendorable  # Vendorable has been vendored
except ImportError:
    import vendorable as _vendorable  # Runtime dependency

# Make current package vendorable with `python -m pkg`
__main__ = _vendorable.main_module

# Create hooks for installation backends
_vendorable.install_hooks(__package__, "vendorable_module.py")
```
