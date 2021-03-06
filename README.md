
[![Actions Status](https://github.com/space-physics/hwm93/workflows/ci_python/badge.svg)](https://github.com/space-physics/hwm93/actions)

[![PyPi version](https://img.shields.io/pypi/pyversions/hwm93.svg)](https://pypi.python.org/pypi/hwm93)
[![PyPi Download stats](http://pepy.tech/badge/hwm93)](http://pepy.tech/project/hwm93)


# HWM93 in Python

NASA Horizontal Wind Model HWM93 in Python &ge; 3.6


![image](tests/example.png)

Works with many Fortran compilers, including:

* Gfortran &ge; 5
* Intel `ifort`
* PGI `pgf90`
* Nvidia `flang`


## Install

A Fortran compiler such as `gfortran` is needed.
We use `f2py` (part of `numpy`) to seamlessly use Fortran libraries from Python.
If you don't have a Fortran compiler, here is how to install Gfortran:

* Linux: `apt install gfortran`
* Mac: `brew install gcc`
* [Windows](https://www.scivision.dev/windows-gcc-gfortran-cmake-make-install/)

    pip install -e .

test by

    pytest -sv

### Windows
If you get ImportError on Windows for the Fortran module, try from the `hwm93` directory:
```posh
del *.pyd
python setup.py build_ext --inplace --compiler=mingw32
```

## Usage

HWM93 can be used from the command line or as an imported Python module in other programs.
Matlab also can use HWM93.

### Command line

    python RunHWM93.py -h

Write data to NetCDF (HDF5) with `-o` option.

### import module

```python
import hwm93
from datetime import datetime

winds = hwm93.run(t=datetime(2017,11,12,8), altkm=150.,
                    glat=65., glon=-148., f107a=150, f107=150, ap=4)
```

`winds` is an [xarray.Dataset](http://xarray.pydata.org/en/stable/generated/xarray.Dataset.html)

```
 <xarray.Dataset>
Dimensions:     (alt_km: 1)
Coordinates:
  * alt_km      (alt_km) float64 150.0
Data variables:
    meridional  (alt_km) float64 4.827
    zonal       (alt_km) float64 -20.5
Attributes:
    time:     2017-11-12T08:00:00
    glat:     65.0
    glon:     -148.0
```

If you want just a single variable, say
```python
print(winds.zonal.values)
```

### Matlab

You can import this Python module from Matlab as in `hwm93.m`.

## Notes

### [Optional] Fortran-only use

Most users don't need this.
```sh
meson build

meson test -C build
````

or

```sh
f2py -c src/hwm93_sub.f  -m hwm93 only: gws5 :
```


### Reference

Original A. E. Hedin Fortran 77 HWM93 [code](ftp://hanna.ccmc.gsfc.nasa.gov/pub/modelweb/atmospheric/hwm93/)
