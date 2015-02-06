---
layout: post
title: "A Bug with GFORTRAN(<4.6)"
description: ""
category: 
tags: []
---
{% include JB/setup %}

In GCC up to 4.6, segmentation fault occurs on the DEALLOCATE statement.
Using ifort resolves the issue. Or letting the compiler do deallocation (commenting out deallocate() commands) resolve the issue.

This is actually [A KNOWN BUG](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=55867) on Bugzilla.
