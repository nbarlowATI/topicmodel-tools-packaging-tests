# topicmodel-tools-packaging-tests
Playing around with Stephen Hansen's text-mining-tutorial code, with the aim of getting the
non-user-facing bits into a PyPi package

## Building source and binary distributions.

Source distribution can be made by running
`python setup.py sdist`

This creates a tar.gz file in the dist/ directory that can then be uploaded to e.g. PyPi
(see below for more info on this).


Binary wheel distribution can be made by running
`python setup.py bdist_wheel --universal`
(universal flag indicates it can work in both python 2 and python 3).
This creates a .whl binary in the dist/ directory for upload to PyPi.  Wheels are platform dependent.

## Uploading to (Test)PyPi

* First need to register at https://pypi.python.org/pypi for the main PyPi repository,
or https://testpypi.python.org/pypi for TestPyPi (separate repository for testing the process).

* Install twine
`pip install twine`

* Create a .pypirc file in your home directory with content something like:

`[distutils]
index-servers=
    pypi
    testpypi


[pypi]
repository: https://upload.pypi.org/legacy/
username: PYPI_USERNAME
password: PYPI_PASSWD

[testpypi]
repository: https://test.pypi.org/legacy/
username: TESTPYPI_USERNAME
password: TESTPYPI_PASSWD
`
(substituting in the usernames/passwords you used when registering with (Test)PyPi).

* Then you can upload with the command:
`twine upload dist/topic-modelling-tools-0.1.dev0.tar.gz -r testpypi` (for TestPyPi)
`twine upload dist/topic-modelling-tools-0.1.dev0.tar.gz -r pypi` (for PyPi).

After this, people should be able to install your package, with e.g.
`pip install topic-modelling-tools` (from regular PyPi)
`pip install --index-url https://test.pypi.org/simple/topic-modelling-tools` from TestPyPi.

Note that if there are dependencies on other python packages, these packages might not be present in the TestPyPi
repository, in which case pip installing from there might fail.

## Unit tests

The basic functionality of the topicmodels library is tested by the suite of unit tests in topicmodel_tests.
These can be run with the command:
`python setup.py test`


## Current status of package and distributions.

Stephen's original text-mining-tutorial package made use of GSL for faster random number generation.  However,
I have not yet managed to get setup.py to handle this correctly even for OSX (and it will be even more difficult
to get it working on multiple platforms), so the current implementation just uses numpy.

A "wheel" for OSX, and a source dist for other platforms, have been uploaded to PyPi.  Installing via `pip install`
has been tested on a couple of different Macs, on an Ubuntu 16.04 VM, and on a Windows Server 2016 VM.  For
the Windows installation, it was necessary to install the Microsoft Visual Studio C++ Compiler (to build the
library from the Cython file).



