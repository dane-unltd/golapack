LAPACK package for the Go language
======

A collection of packages to provide LAPACK functionality for the Go programming
language (http://golang.org)

This is work in progress. Breaking changes are likely to happen.

## Installation 

```
  go get github.com/dane-unltd/blas
```


Install OpenBLAS:
```
  git clone https://github.com/xianyi/OpenBLAS
  cd OpenBLAS
  make
```

For Windows you can download binary packages for OpenBLAS at
http://sourceforge.net/projects/openblas/files/

generate lapack bindings
```
  cd $GOPATH/src/github.com/dane-unltd/lapack/clapack
  ./genLapack -L/path/to/OpenBLAS -lopenblas
```

If you want to use the Intel MKL and all of your paths are properly set
```
  cd $GOPATH/src/github.com/dane-unltd/lapack/clapack
  ./genLapack -lmkl_rt
```
should work.

## Packages

### lapack

Defines the LAPACK API based on http://www.netlib.org/lapack/lapacke.html

### lapack/clapack

Binding to a C implementation of the lapacke interface (e.g. OpenBLAS or intel MKL)

The linker flags (i.e. path to the BLAS library and library name) might have to be adapted.

The recommended (free) option for good performance on both linux and darwin is OpenBLAS.

### blas/dbw

Experimental wrapper for the float64 part of the lapack interface.

You have to register an implementation before you can use the LAPACK functions:

```
package main

import (
	"fmt"

	"github.com/gonum/blas/cblas"
	"github.com/gonum/blas/dbw"
	"github.com/gonum/lapack/clapack"
	"github.com/gonum/lapack/dla"
	"github.com/gonum/
)

func init() {
	dbw.Register(cblas.Blas{})
	dla.Register(clapack.Lapack{})
}

func main() {
	A := dbw.NewGeneral(3, 2, []float64{1, 2, 3, 4, 5, 6})
	B := dbw.NewGeneral(3, 2, []float64{1, 2, 1, 2, 1, 2})

	tau := dbw.Allocate(2)

	f := dla.QR(A, tau)

	f.Solve(B)

	fmt.Println(B.Data)
}
```

### blas/zbw

Experimental wrapper for the complex128 part of the lapack interface.

