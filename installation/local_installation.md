---
layout: default
title: Local Installation (Linux)
parent: Installation Instructions
nav_order: 2
---

## Download

Elmer/Ice can be retrived along with the entire Elmer package from [Github](https://github.com/elmercsc/elmerfem).
Execute the command:

```bash
git clone git://www.github.com/ElmerCSC/elmerfem -b elmerice elmerice
```
in a directory where you can store a local copy of the elemer source code (i.e. `/home/user/bin`).
Note the `-b elmerice` command specifies the Elmer/Ice branch of the source repo.
This is necessary to make sure there Elmer/Ice development doesn’t interfere with the main Elmer/Ice developments.
Also note the second `elmerice` specifies the directory to pull the repo, this can be changed if you so feel.


## Instal Dependancies

The core requirements for building Elmer (and therefore Elmer/Ice) are:  
  - a Fortran 90 compiler
  - (GNU) make
  - cmake  

additional requirements are specified on the [Github](https://github.com/elmercsc/elmerfem) and are included below.

On Ubuntu run:
```bash
sudo apt install cmake build-essential gfortran libopenmpi-dev libblas-dev liblapack-dev
```

## Building Elmer/Ice


The following script is from the guidline to compile Elmer with Elmer/Ice from the [Elmer/Ice Wiki](http://elmerfem.org/elmerice/wiki/doku.php?id=compilation:compilationcmake):


```bash
#!/bin/bash
CMAKE=cmake
# Installation directory (set these!)
TIMESTAMP=$(date +"%m-%d-%y")
ELMER_REV="Elmer_devel_${TIMESTAMP}"
ELMERSRC="/path/to/Elmer/Source/elmerice"
BUILDDIR="$ELMERSRC/builddir"
IDIR="/usr/local/$ELMER_REV"


echo "Building Elmer from within " ${BUILDDIR}
echo "installation into " ${IDIR}
cd ${BUILDDIR}
pwd
ls -ltr

echo $CMAKE $ELMERSRC

$CMAKE $ELMERSRC \
      -DCMAKE_INSTALL_PREFIX=$IDIR \
      -DCMAKE_C_COMPILER=/path/to/compiler/executable \
      -DCMAKE_Fortran_COMPILER=/path/to/compiler/executable \
      -DWITH_MPI:BOOL=TRUE \
      -DWITH_Mumps:BOOL=FALSE \
      -DWITH_Hypre:BOOL=FALSE \
      -DWITH_Trilinos:BOOL=FALSE \
      -DWITH_ELMERGUI:BOOL=FALSE \
      -DWITH_ElmerIce:BOOL=TRUE

# where -j4 is the number of available cores on your system
make -j4 && sudo make install

# this automatically links /usr/local/Elmer-devel to your new built
sudo rm /usr/local/Elmer-devel
sudo ln -s $IDIR /usr/local/Elmer-devel
###############################################################################    
```

To find out which `C` and `Fortran` compilers are on you system run:

```bash
dpkg --list | grep -E 'Fortran.*compiler|C.*compiler'
```
and then to find where they are located run:
```bash
which C_COMPILER
```
or
```bash
which Fortran_COMPILER
```
but on a standard Linux system the `C` and `fortran` compilers (and paths to them) will be `/usr/bin/gcc` and
`usr/bin/gfortran` respectively.  


Finally, to find out how many cores you have available on your system run:
```bash
lscpu | grep -E '^Thread|^Core|^Socket|^CPU\('  
```

Once the above script is edited to your system configurations make it exectuable through `chmod +x script_name`
and run it.
Add the path to your .bashrc as a final step.
```bash
export PATH="/usr/local/Elmer-devel/bin:$PATH"
```


## Test the build

After building you instalation of Elmer/Ice, you can run the tests:
```bash
ctest -L elmerice #run all Elmer/Ice tests (VERY long)
ctest -L elmerice-fast #set of fast Elmer/Ice tests
ctest -L elmerice-long #set of slow Elmer/Ice tests
ctest -L netcdf #set of test using NetCDF library
```
or to only run specific test (that match the given regular expression)

```bash
ctest -R <expression>
```
