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

## 4
Now for some ~/.bash_profile stuff:
```
> /usr/libexec/java_home -V
Matching Java Virtual Machines (1):
    1.8.0_202, x86_64:	"Java SE 8"	/Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Home

/Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Home
```
From that, I put the following lines in my ~/.bash_profile (or it may be your .bashrc / .profile):
```
export JAVA_HOME=$(/usr/libexec/java_home -v 1.8.0_202)
```
Then, I restarted terminal and was able to see:
```
> echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Home
```

## 5
Edit a plist:
```
> sudo vim /Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Info.plist
```
Go to the line
```
<string>CommandLine</string>
```
and add <string>JNI</string> immediately after so it looks like this:
```
<string>CommandLine</string>
<string>JNI</string>
```

## 6
Now we tell R to find Java:
```
> JAVA_HOME=${JAVA_HOME}/jre
> sudo R CMD javareconf \
JAVA_HOME=${JAVA_HOME} \
JAVA=${JAVA_HOME}/../bin/java \
JAVAC=${JAVA_HOME}/../bin/javac \
JAVAH=${JAVA_HOME}/../bin/javah \
JAR=${JAVA_HOME}/../bin/jar \
JAVA_LIBS="-L${JAVA_HOME}/lib/server -ljvm" \
JAVA_CPPFLAGS="-I${JAVA_HOME}/../include -I${JAVA_HOME}/../include/darwin"
```

