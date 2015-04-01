---
layout: post
title: "Compiling F77 Sources with F90"
description: ""
category: 
tags: []
---
{% include JB/setup %}

There are multiple considerations to make:

## Interfacing

- Fortran 77 has made certain assumptions about the memory lay-out of arrays, primarily, that arrays are column oriented and contiguous. In many of the matrix oriented packages you need to pass the maximum leading dimension. As far as the routines were concerned, all arrays were one-dimensional. If given the maximum leading dimension and current matrix dimension the routine could determine where the array data was stored. This is called an assumed-size specification. 

- Fortran 90 frees the program of much of these burdens by packaging the array dimensions into the passed array reference. All the routine needs to know is the array shape (i.e. 1,2,3, or more dimensions). This is the assumed-shape specification. 

The solution to resolve this difference is to provide an INTERFACE block, which gives the Fortran 90 compiler more information regarding the Fortran 77 routine. 

An example can be found [here](http://owen.sj.ca.us/~rk/howto/slides/f90model/slides/f77int.html).

Another example, providing interfaces in a modle like [this](https://github.com/certik/fortran-utils/blob/master/src/amos.f90).

## Upper/Lower-cases

Both f77 and f90 treat all source lines as if they were lowercase (except in quoted character strings. However, unlike f77, f90 has no -U option to force the compiler to be sensitive to both upper and lower case. This may present a problem when mixing f77 and f90 compiled routines. Since a routine compiled by f90 will treat CALL XyZ the same as CALL XYz, and treat them both as if they were CALL xyz, care must be taken to rearrange the way these calls are made. A similar situation will exist when trying to define entry points in f90 compiled routines that are differentiated by case. The clue to potential problems would be the need to use -U with f77.

[Reference](http://docs.oracle.com/cd/E19957-01/805-4941/6j4m2sobm/index.html).

## General Solution

Compiling the FORTRAN 77 and Fortran 90/59/2003/2008 source with the same compiler should produce compatible object modules. 

It is convenient to compile the FORTRAN 77 first, into an object file, and then use the fortran command that compiles the Fortran 90 to link everything.

Usually we have to compile the two language versions separately since different compiler options will probably be necessary, e.g., for fixed and free-form source layout. With the interfaces in your Fortan 90/95/2003/2008 code, the compiler will use compatible calling conventions.

To mix f77 and f90 object binaries, link with f90 and the f77 compatibility library (for ifort, it is libf77compat).

[Reference](http://stackoverflow.com/questions/16342259/how-to-use-fortran-77-subroutines-in-fortran-90-95).
