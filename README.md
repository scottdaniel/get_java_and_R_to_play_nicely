# get_java_and_R_to_play_nicely
Guide to get Java and R to play nicely on Mac OS X El Capitan

Versions:
R version 3.5.2 (2018-12-20)

Platform: x86_64-apple-darwin15.6.0 (64-bit)

Running under: macOS Mojave 10.14.2

and Rstudio.Version():
```$mode
[1] "desktop"

$version
[1] ‘1.1.463’
```
# Preliminary
This took me the better half of a day to figure out, so I thought a guide would be useful to avoid future frustration. Of course, YMMV.

This is approximately based off of: http://www.owsiak.org/r-3-4-rjava-macos-and-even-more-mess/

## 1
Download and extract the latest clang for Mac, I got mine from here: http://releases.llvm.org/download.html#7.0.0

I placed it like so:
```
mkdir ~/opt
cd ~/opt
mv ~/Downloads/clang+llvm-7.0.0-x86_64-apple-darwin.tar.xz ./
tar xf clang+llvm-7.0.0-x86_64-apple-darwin.tar.xz
```

## 2
Download and install gFortran, this provides some nice Mac'esque installers:
https://github.com/fxcoudert/gfortran-for-macOS/releases

## 3
Now for the tricky part, you need the Java Runtime Environment (JRE) but if you update/install Java SE for Mac, it just installs as a Safari plugin!
Even worse (for me), installing the latest Java Development Kit (JDK v. 11.0.2) didn't install a JRE!
This is what (FINALLY) worked for me: https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
I downloaded and installed Java SE Development Kit 8u202
