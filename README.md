```
/*
Copyright (c) by respective owners including Yahoo!, Microsoft, and
individual contributors. All rights reserved.  Released under a BSD (revised)
license as described in the file LICENSE.
 */
```

[![Build Status](https://travis-ci.org/JohnLangford/vowpal_wabbit.png)](https://travis-ci.org/JohnLangford/vowpal_wabbit)

This is the *vowpal wabbit* fast online learning code.  For Windows, look at README.windows.txt

## Prerequisite software

These prerequisites are usually pre-installed on many platforms. However, you may need to consult your favorite package
manager (*yum*, *apt*, *MacPorts*, *brew*, ...) to install missing software.

- [Boost](http://www.boost.org) library, with the `Boost::Program_Options` library option enabled.
- GNU *autotools*: *autoconf*, *automake*, *libtool*, *autoheader*, et. al. This is not a strict prereq. On many systems (notably Ubuntu with `libboost-dev` installed), the provided `Makefile` works fine.
- (optional) [git](http://git-scm.com) if you want to check out the latest version of *vowpal wabbit*,
  work on the code, or even contribute code to the main project.

## Getting the code

You can download the latest version from [here](https://github.com/JohnLangford/vowpal_wabbit/wiki/Download).
The very latest version is always available via 'github' by invoking one of the following:

```
## For the traditional ssh-based Git interaction:
$ git clone git://github.com/JohnLangford/vowpal_wabbit.git

## For HTTP-based Git interaction
$ git clone https://github.com/JohnLangford/vowpal_wabbit.git
```

## Compiling

You should be able to build the *vowpal wabbit* on most systems with:
```
$ make
$ make test    # (optional)
```

If that fails, try:
```
$ ./autogen.sh
$ make
$ make test    # (optional)
$ make install
```

Note that `./autogen.sh` requires *automake* (see the prerequisites, above.)

`./autogen.sh`'s command line arguments are passed directly to `configure` as
if they were `configure` arguments and flags.

Note that `./autogen.sh` will overwrite the supplied `Makefile`, so
keeping a copy of `Makefile` may be a good idea before running `autogen.sh`.

Be sure to read the wiki: https://github.com/JohnLangford/vowpal_wabbit/wiki
for the tutorial, command line options, etc.

The 'cluster' directory has it's own documentation for cluster
parallel use, and the examples at the end of test/Runtests give some
example flags.

## C++ Optimization

The default C++ compiler optimization flags are very aggressive. If you should run into a problem, consider running `configure` with the `--enable-debug` option, e.g.:

```
$ ./configure --enable-debug
```

or passing your own compiler flags via the `CXXOPTIMIZE` make variable:

```
$ make CXXOPTIMIZE="-O0 -g"
```

## Ubuntu/Debian specific info

On Ubuntu/Debian/Mint and similar the following sequence should work
for building the latest from github:

```
apt-get install libboost-dev
git clone git://github.com/JohnLangford/vowpal_wabbit.git
cd vowpal_wabbit
make
make test       # (optional)
make install
```

### Ubuntu advanced build options (clang and static)

If you prefer building with `clang` instead of `gcc` (much faster build
and slighly faster executable), install `clang` and change the `make`
step slightly:

```
apt-get install clang

make CXX=clang++
```

A statically linked `vw` executable that is not sensitive to boost
version upgrades and can be safely copied between different Linux
versions (e.g. even from Ubuntu the Red Hat) can be built and tested with:

```
make CXX='clang++ -static' clean vw test     # you may ignore warnings
```

## Mac OS X-specific info

OSX requires _glibtools_, which is available via the [brew](http://brew.sh) or
[MacPorts](https://www.macports.org) package managers.

### Complete brew install of 7.10
```
brew install vowpal-wabbit
```
[The homebrew formula for VW is located on github](https://github.com/Homebrew/homebrew/blob/master/Library/Formula/vowpal-wabbit.rb).

### brew install dependencies + manual install of vowpal wabbit
```
brew install libtool
brew install boost --with-python
```

### MacPorts
```
## Install glibtool and other GNU autotool friends:
$ port install libtool autoconf automake

## Build Boost for Mac OS X 10.8 and below
$ port install boost +no_single +no_static +openmpi +python27 configure.cxx_stdlib=libc++ configure.cxx=clang++

## Build Boost for Mac OS X 10.9 and above
$ port install boost +no_single +no_static +openmpi +python27
```

*Mac OS X 10.8 and below*: ``configure.cxx_stdlib=libc++`` and ``configure.cxx=clang++`` ensure that ``clang++`` uses
the correct C++11 functionality while building Boost. Ordinarily, ``clang++`` relies on the older GNU ``g++`` 4.2 series
header files and ``stdc++`` library; ``libc++`` is the ``clang`` replacement that provides newer C++11 functionality. If
these flags aren't present, you will likely encounter compilation errors when compiling _vowpalrabbit/cbify.cc_. These
error messages generally contain complaints about ``std::to_string`` and ``std::unique_ptr`` types missing.

To compile:
```
$ sh autogen.sh --enable-libc++
$ make
$ make test    # (optional)
```

