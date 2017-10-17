# `PyGMOS`
An automated `PyRAF` data reduction pipeline for `GMOS` spectroscopic data


## Installation

Download the latest (stable) version from this repository. Then run:

    python setup.py install [--user]

where the `--user` flag is recommended so as not to install as root. To check whether the `pygmos` executable is in your `PATH`, type

    which pygmos

If you see nothing (or a 'not found' message), it means the location of the executable is not part of your `PATH`. If this is the case, look through the installation messages for the place where the executable was copied to. In my case:

    Installing pygmos script to /u/sifon/.local/bin

You should therefore add the equivalent of `/u/sifon/.local/bin` to your `PATH`. Add the following line to your `~/.bashrc` or `~/.bash_profile`:

    export PATH=/u/sifon/.local/bin:$PATH

or to the `~/.cshrc`, etc, if you're not using `bash`:

    setenv PATH /u/sifon/.local/bin:$PATH

and restart the console.

## Contains:

#### Main code

  * `pygmos`               -- the code itself
 
#### Auxiliary
  
  * `align.cl`              -- align spectra by wavelength
  * `check_gswave`          -- compares the corrected arc lines with the given ones to estimate dispersion
  * `CuAr-GMOS.dat`         -- set of CuAr lamp emission lines for wavelength calibration
  * `inventory.py`          -- identifies the fits files corresponding to each object
  * `lacos_spec.cl`         -- cosmic ray removal software written by Pieter van Dokkum (distributed with permission)
  * `README.md`             -- this file

## To run, type:

    pygmos <object> [options]

For further help, go [here](http://www.astro.princeton.edu/~sifon/pygmos/help.html)

## Python requirements:
    numpy, http://numpy.scipy.org/
    PyFITS, http://www.stsci.edu/resources/software_hardware/pyfits
    PyRAF, http://www.stsci.edu/resources/software_hardware/pyraf

All of these are best installed through [Astroconda](http://astroconda.readthedocs.io/en/latest/).

----

**How it works:**

The pipeline takes the object name given in the command line and finds
all data associated with that object. It bias-subtracts all images and
calibrates the science image with the flat field. It then does the
wavelength calibration, removes cosmic rays using L.A.Cosmic (van
Dokkum, 2001, PASP, 113, 1420) and sky subtracts the spectra. After
this, the individual exposures are added. Finally, the 1d spectra are
extracted.

**Data format taken by pygmos:**

Usual GMOS FITS files, which means two exposures per object, each of
which is composed of a science image, a flat field and a calibration
arc.

**When running the pipeline, keep in mind that:**

  * As of now, the pipeline reduces both MOS and longslit GMOS spectra
 but flux calibration is not implemented.
  * Being an automated process, some things could go
 wrong. Most task parameters have to be modified by digging into the
 code, although some of the most important are easy to find, as they
 are in the definition of the functions. Others can be easily added
 in the usual IRAF way.
  * This code has only been tested (and is recommended) for redshift
 measurements.

## Features:

  * Automatic identification of all relevant files given the object name.
  * Incorporates the Lagrangian Cosmic Ray Removal "L.A.Cosmic" code
 implemented by P. van Dokkum (distributed with permission).
  * Can be executed either in automatic or interactive mode, which allows
 for a more thorough analysis, without the need to run each PyRAF task
 separately.
  * Has the option of automatically cutting the spectra and copying them
 to a separate folder. This is useful if, for instance, the spectra
 will be cross-correlated using RVSAO, which takes only single spectra
 (as opposed to multi-slit images) as input.
  * Has the option of aligning the 2d images, which is particularly
 useful for visualizing galaxy cluster data.
 
## Current specific limitations:

  * Flux calibration is not implemented.
  * There is no automated search for the bias file(s). The (master)
 bias file needs to be given in the command (see help page). If not
 given, the pipeline will ask for one.
  * The inventory only finds one Flat and one Arc per observation.
  * When run automatically, the pipeline only extracts one aperture from
 each slit, while some slits might contain more than one object.
  * The interactive feature runs all tasks interactively, without being
 able to switch individual tasks as automatic and others as
 interactive. 
 
 
 ---
 
 (c) Cristóbal Sifón
 
 Last updated March 2012.