## Introduction

This is a quick and dirty way to assess performance of multiple methods to do some task across 
multiple versions of Python. The task shown here is character-by-character string concatenation, 
but the same code could be used to assess other tasks.

## Setup

There are a couple of system-dependent warts (`sed` in `iter_versions` and `sysctl` in `graphs`) 
that you might need to adjust. This was written on a mac book.

Whatever platform you're on, you'll need:

- pyenv
- pyenv install (whatever versions you want to test - min 2.6)
- R
- ggplot2 for R

## Usage

To run through all of the methods for your current version of Python, generating `times.csv`:

    ./trial

To run `trial` through all of your installed pyenv versions

    ./iter_versions

To generate `Rplots.pdf` from `times.csv`:

    ./graphs

## Results (concatenation)

- `StringIO` is very slow for Python 2.x through 3.0, but is greatly improved in 3.3+
- `cStringIO` in 2.x is faster than `StringIO` but is still fairly slow.
- `BytesIO` in 3.x is somewhat fast; the performance of `StringIO` becomes nearly equivalent to 
  `BytesIO` in 3.3+
- New versions of Python are not guaranteed to speed up memory operations. Unfortunately, the 
  opposite is often true. Simple string concatenation is far slower in 3.3+ than 2.x. 
  Concatenation of `bytes` has been particularly worsened in 3.x.
- Some methods (`join`, `StringIO`) have a fairly linear input complexity; that is, after some 
  initial cost they approximate O(n). Other methods (`reduce`, and `bytes+` in 3.x) scale very 
  poorly.

This is not an attempt to deter people from using 3.x. I would love critique on my methods (gc?) 
and findings.