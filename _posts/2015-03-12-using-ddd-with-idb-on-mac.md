---
layout: post
title: "Using DDD with IDB on Mac"
description: ""
category: 
tags: []
---
{% include JB/setup %}

- This solution came from [Intel Developer Zone](https://software.intel.com/en-us/forums/topic/269052). -

With the Intel Debugger (IDB) for Mac OS X only providing a command-line interface and Xcode only capable of debugging simple structured Fortran programs with no support of MODULEs, GUI-based debugging of Fortran on Mac OS X seemed impossible, until now.

An option exists for GUI-based debugging of Fortran on Mac OS X using the GNU DataDisplayDebugger (DDD) (http://www.gnu.org/software/ddd/) with the Intel Debugger (IDB).

To obtain DDD 3.3.11 for Mac OS X, first install Fink 0.9.0 and FinkCommander (GUI interface to Fink) for Mac OS X available from http://www.finkproject.org/ (download versions are available for various Mac OS X releases).

I found FinkCommander very easy to use. There are numerous software packages available for installation, so under the FinkCommander window scroll through the package names or use the search facility to locate ddd. Once found, click on the +.h toolbar icon or right-click on the ddd package name and select Source -> Install. This will download, build, and install ddd and the required lesstif-shlibs packages.

DDD installs under: /sw/bin/ddd

Once installed, open a terminal window and launch DDD via the command-line with Intel Debugger (IDB) as follows:

ddd --debugger "/usr/bin/idb"

We welcome user feedback about using DDD as a GUI front-end to the Intel Debugger (IDB) for Mac OS X.

DDD is also usable with the command-line interface to the Intel Debugger (IDB) for Linux. Please refer to the GNU DataDisplayDebugger (DDD) page (http://www.gnu.org/software/ddd/) for information about obtaining DDD source and/or binary downloads for Linux.
