# Toml

## Description

Python Library for Tom's Obvious, Minimal Language

## README

****
TOML
****

.. image:: https://img.shields.io/pypi/v/toml
    :target: https://pypi.org/project/toml/

.. image:: https://travis-ci.org/uiri/toml.svg?branch=master
    :target: https://travis-ci.org/uiri/toml

.. image:: https://img.shields.io/pypi/pyversions/toml.svg
    :target: https://pypi.org/project/toml/


A Python library for parsing and creating `TOML <https://en.wikipedia.org/wiki/TOML>`_.

The module passes `the TOML test suite <https://github.com/BurntSushi/toml-test>`_.

See also:

* `The TOML Standard <https://github.com/toml-lang/toml>`_
* `The currently supported TOML specification <https://github.com/toml-lang/toml/blob/v0.5.0/README.md>`_

Installation
============

To install the latest release on `PyPI <https://pypi.org/project/toml/>`_,
simply run:

::

  pip install toml

Or to install the latest development version, run:

::

  git clone https://github.com/uiri/toml.git
  cd toml
  python setup.py install

Quick Tutorial
==============

*toml.loads* takes in a string containing standard TOML-formatted data and
returns a dictionary containing the parsed data.

.. code:: pycon

  > > > import toml
  > > > toml_string = """
  ... # This is a TOML document.
  ...
  ... title = "TOML Example"
  ...
  ... [owner]
  ... name = "Tom Preston-Werner"
  ... dob = 1979-05-27T07:32:00-08:00 # First class dates
  ...
  ... [database]
  ... server = "192.168.1.1"
  ... ports = [ 8001, 8001, 8002 ]
  ... connection_max = 5000
  ... enabled = true
  ...
  ... [servers]
  ...
  ... # Indentation (tabs and/or spaces) is allowed but not required
  ... [servers.alpha]
  ... ip = "10.0.0.1"
  ... dc = "eqdc10"
  ...
  ... [servers.beta]
  ... ip = "10.0.0.2"
  ... dc = "eqdc10"
  ...
  ... [clients]
  ... data = [ ["gamma", "delta"], [1, 2] ]
  ...
  ... # Line breaks are OK when inside arrays
  ... hosts = [
  ... "alpha",
  ... "omega"
  ... ]
  ... """
  > > > parsed_toml = toml.loads(toml_string)


*toml.dumps* takes a dictionary and returns a string containing the
corresponding TOML-formatted data.

.. code:: pycon

  > > > new_toml_string = toml.dumps(parsed_toml)
  > > > print(new_toml_string)
  title = "TOML Example"
  [owner]
  name = "Tom Preston-Werner"
  dob = 1979-05-27T07:32:00Z
  [database]
  server = "192.168.1.1"
  ports = [ 8001, 8001, 8002,]
  connection_max = 5000
  enabled = true
  [clients]
  data = [ [ "gamma", "delta",], [ 1, 2,],]
  hosts = [ "alpha", "omega",]
  [servers.alpha]
  ip = "10.0.0.1"
  dc = "eqdc10"
  [servers.beta]
  ip = "10.0.0.2"
  dc = "eqdc10"

*toml.dump* takes a dictionary and a file descriptor and returns a string containing the
corresponding TOML-formatted data.

.. code:: pycon

  > > > with open('new_toml_file.toml', 'w') as f:
  ... new_toml_string = toml.dump(parsed_toml, f)
  > > > print(new_toml_string)
  title = "TOML Example"
  [owner]
  name = "Tom Preston-Werner"
  dob = 1979-05-27T07:32:00Z
  [database]
  server = "192.168.1.1"
  ports = [ 8001, 8001, 8002,]
  connection_max = 5000
  enabled = true
  [clients]
  data = [ [ "gamma", "delta",], [ 1, 2,],]
  hosts = [ "alpha", "omega",]
  [servers.alpha]
  ip = "10.0.0.1"
  dc = "eqdc10"
  [servers.beta]
  ip = "10.0.0.2"
  dc = "eqdc10"

For more functions, view the API Reference below.

Note
----

For Numpy users, by default the data types ``np.floatX`` will not be translated to floats by toml, but will instead be encoded as strings. To get around this, specify the ``TomlNumpyEncoder`` when saving your data.

.. code:: pycon

  > > > import toml
  > > > import numpy as np
  > > > a = np.arange(0, 10, dtype=np.double)
  > > > output = {'a': a}
  > > > toml.dumps(output)
  'a = [ "0.0", "1.0", "2.0", "3.0", "4.0", "5.0", "6.0", "7.0", "8.0", "9.0",]\n'
  > > > toml.dumps(output, encoder=toml.TomlNumpyEncoder())
  'a = [ 0.0, 1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0,]\n'

API Reference
=============

``toml.load(f, _dict=dict)``
  Parse a file or a list of files as TOML and return a dictionary.

  :Args:
    * ``f``: A path to a file, list of filepaths (to be read into single
      object) or a file descriptor
    * ``_dict``: The class of the dictionary object to be returned

  :Returns:
    A dictionary (or object ``_dict``) containing parsed TOML data

  :Raises:
    * ``TypeError``: When ``f`` is an invalid type or is a list containing
      invalid types
    * ``TomlDecodeError``: When an error occurs while decoding the file(s)

``toml.loads(s, _dict=dict)``
  Parse a TOML-formatted string to a dictionary.

  :Args:
    * ``s``: The TOML-formatted string to be parsed
    * ``_dict``: Specifies the class of the returned toml dictionary

  :Returns:
    A dictionary (or object ``_dict``) containing parsed TOML data

  :Raises:
    * ``TypeError``: When a non-string object is passed
    * ``TomlDecodeError``: When an error occurs while decoding the
      TOML-formatted string

``toml.dump(o, f, encoder=None)``
  Write a dictionary to a file containing TOML-formatted data

  :Args:
    * ``o``: An object to be converted into TOML
    * ``f``: A File descriptor where the TOML-formatted output should be stored
    * ``encoder``: An instance of ``TomlEncoder`` (or subclass) for encoding the object. If ``None``, will default to ``TomlEncoder``

  :Returns:
    A string containing the TOML-formatted data corresponding to object ``o``

  :Raises:
    * ``TypeError``: When anything other than file descriptor is passed

``toml.dumps(o, encoder=None)``
  Create a TOML-formatted string from an input object

  :Args:
    * ``o``: An object to be converted into TOML
    * ``encoder``: An instance of ``TomlEncoder`` (or subclass) for encoding the object. If ``None``, will default to ``TomlEncoder``

  :Returns:
    A string containing the TOML-formatted data corresponding to object ``o``



Licensing
=========

This project is released under the terms of the MIT Open Source License. View
*LICENSE.txt* for more information.
