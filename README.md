# qmakeandroidclang
An attempt at setting up an android-clang mkspec for Qmake as it only provides android-g++ by default

Still loads to tweak and discover, but this is already a good start. 
So far I've found some workarounds needed to make this work but it does give me full APKs straight from
Qt Creator. 

The reason behind this is that currently NDK ships with GCC 4.9 and I wanted to play around with C++14 (which afaik
is GCC 5+ only). 

My hope is that I eventually bring this to a more robust state. I have made this under Linux and only tested it there for 
incompatibilities so I wouldn't be too surprised if this exploded in other systems

##workarounds

I've had issues where the /opt folder must contain an "android" folder, with "sdk/" and "ndk/" folders inside it,
symbolic links to the actual locations worked fine.

The android-clang folder must be copied to your qt installation folder (e.g: "/opt/qt/5.4/android_armv5/mkspecs"). 
This folder contains the configuration files to allow Qt to use the clang compiler supplied with LLVM. The file is
still in a crude state and, as such, things are hard-coded for llvm 3.6 atm.

When configuring the QT kit, you must MANUALLY specify qmake mkspec as "android-clang", otherwise
it defaults to either unix-clang or android-g++

First time compilation may fail complaining about some missing unspecified JSON. If this happens,
disable the "generate apk" build step in QT creator and build the application once, re-enabling it afterward.

In their mangling of the NDK, google at some point did away with some necessary headers for libc++.
A copy of these headers is found in the android-ndk folder in the project, with the full path where they
must be copied to inside the NDK.

In QT Creator when setting up your project, it may be necessary to alter the build environment. Set
ANDROID_NDK_PLATFORM to android-21 and ANDROID_TOOLCHAIN_VERSION to 4.9 as these values are read
by qmake when generating the makefiles for the project.
