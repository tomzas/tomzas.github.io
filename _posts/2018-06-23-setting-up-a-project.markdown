---
layout: post
title:  "Setting up a project"
date:   2018-06-23 23:16:00 +0200
categories: jekyll update
---
I wrote a small shell script to help me setup the bare minimum project. This will help me quickly
prepare a new learning example together with unit test framework. I am very convinced that learning
using Test Driven Development will definitely benefit me.

I created also a github repository for my scripts:

[https://github.com/tomzas/scripts](https://github.com/tomzas/scripts)

The script is called bootstrap.sh. It sets up a C++ project with CMake and GTest. GTest can be a
fresh clone from github and built locally or it can be cloned from local copy then built. Also
it's possible to point to already existing GTest installation, this will set the GTEST_ROOT.

I prefer to use ninja-build on my Linux machines, but on Mac the ninja-build cannot be install.
Maybe I am searching in wrong place. Does not matter. I prepared the script that by default it uses
make and with a command line argument it enables to use ninja-build.