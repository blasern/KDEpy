[![DOI](https://zenodo.org/badge/118142261.svg)](https://zenodo.org/badge/latestdoi/118142261) [![Build Status](https://travis-ci.org/tommyod/KDEpy.svg?branch=master)](https://travis-ci.org/tommyod/KDEpy) [![Build status](https://ci.appveyor.com/api/projects/status/2esjgx50mf6x1g67?svg=true)](https://ci.appveyor.com/project/tommyod/kdepy) [![Documentation Status](https://readthedocs.org/projects/kdepy/badge/?version=latest)](http://kdepy.readthedocs.io/en/latest/?badge=latest) [![PyPI version](https://badge.fury.io/py/KDEpy.svg)](https://badge.fury.io/py/KDEpy) [![Downloads](https://pepy.tech/badge/kdepy)](https://pepy.tech/project/kdepy)
---------

# [KDEpy](https://kdepy.readthedocs.io/en/latest/)

## About

This Python 3.5+ package implements various kernel density estimators (KDE).
Three algorithms are implemented through the same API: [`NaiveKDE`](https://kdepy.readthedocs.io/en/latest/API.html#naivekde), [`TreeKDE`](https://kdepy.readthedocs.io/en/latest/API.html#treekde) and [`FFTKDE`](https://kdepy.readthedocs.io/en/latest/API.html#fftkde).
The class [`FFTKDE`](https://kdepy.readthedocs.io/en/latest/API.html#fftkde) outperforms other popular implementations, see the [comparison page](https://kdepy.readthedocs.io/en/latest/comparison.html).

![Plot](./docs/source/_static/img/showcase.png)

*The code generating the above graph is found in [examples.py](https://github.com/tommyod/KDEpy/blob/master/docs/source/examples.py).*

## Installation

KDEpy is available through [PyPI](https://pypi.org/project/KDEpy/), and may be installed using `pip`:

```bash
pip install KDEpy
```

If you have [trouble on Ubuntu](https://github.com/tommyod/KDEpy/issues/11), try running `sudo apt install libpython3.X-dev`, where `3.X` is your Python version. 

## Example code and documentation

Below is an example using NumPy as `np` and `scipy.stats.norm` to plot a density estimate.
From the code below, it should be clear how to set the *kernel*, *bandwidth* (variance of the kernel) and *weights*.
See the [documentation](https://kdepy.readthedocs.io/en/latest/examples.html) for more examples.

```python
from KDEpy import FFTKDE
data = norm(loc=0, scale=1).rvs(2**3)
estimator = FFTKDE(kernel='gaussian', bw='silverman')
x, y = estimator.fit(data, weights=None).evaluate()
plt.plot(x, y, label='KDE estimate')
```
![Plot](./docs/source/_static/img/mwe.png)

The package consists of three algorithms. Here's a brief explanation:
- [`NaiveKDE`](https://kdepy.readthedocs.io/en/latest/API.html#naivekde) - A naive computation. Supports d-dimensional data, variable bandwidth, weighted data and many kernel functions. Very slow on large data sets.
- [`TreeKDE`](https://kdepy.readthedocs.io/en/latest/API.html#treekde) - A tree-based computation. Supports the same features as the naive algorithm, but is faster at the expense of small inaccuracy when using a kernel without finite support. Good for evaluation on non-uniform, arbitrary grids.
- [`FFTKDE`](https://kdepy.readthedocs.io/en/latest/API.html#fftkde) - A very fast convolution-based computation. Supports weighted d-dimensional data and many kernels, but not variable bandwidth. Must be evaluated on an equidistant grid, the finer the grid the higher the accuracy. Data points may not be outside of the grid.

## Issues and contributing

### Issues

If you are having trouble using the package, please let me know by creating an [Issue on GitHub](https://github.com/tommyod/KDEpy/issues) and I'll get back to you.

### Contributing

Whatever your mathematical and Python background is, you are very welcome to contribute to KDEpy.
To contribute, fork the project, create a branch and submit and Pull Request.
Please follow these guidelines:
- Import as few external dependencies as possible.
- Use test driven development, have tests and docs for every method.
- Cite literature and implement recent methods.
- Unless it's a bottleneck computation, readability trumps speed.
- Employ object orientation, but resist the temptation to implement many methods -- stick to the basics.
- Follow PEP8.
