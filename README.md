# Work in Progess. Don't try using this yet as it's a mess in this state.

# OpenMW for Android

## Building

There are two steps for building OpenMW for Android. The first step is building C/C++ libraries. The second step is building the Java launcher.

### Prerequisites

You will need some standard tools installed that you probably already have (bash, gcc, g++, sha256sum, unzip, python3, python3-devel, curl, cmake).

Run the following on your machine to install them:

rpm-based: sudo dnf install gcc g++ unzip python3 python3-devel python-is-python3 cmake

### debian-based: 

#sudo add-apt-repository ppa:openmw/openmw

#sudo apt-get update

#sudo apt-get install git build-essential cmake curl bash gcc g++ python3 python3-dev python-is-python3 libopenal-dev libopenscenegraph-dev libbullet-dev libsdl2-dev lib32z1 libmygui-dev libunshield-dev liblz4-dev libtinyxml-dev libqt5opengl5-dev libboost-filesystem-dev libboost-program-options-dev libboost-iostreams-dev libavcodec-dev libavformat-dev libavutil-dev libswscale-dev libswresample-dev librecast-dev libluajit-5.1-dev libsqlite3-dev libyaml-cpp-dev libcollada-dom-dev clang libc6:i386 libncurses5:i386 libstdc++6:i386 libbz2-1.0:i386

CMake 3.6.0 or newer is **required**, you can download the latest version [here](https://cmake.org/download/) (and place in your `PATH`) if your distro ships with an outdated version.

Additionally, to build the launcher you will need Android SDK installed, it is suggested that you use Android Studio which can set it up for you (see step 2).

### Step 1: Build the libraries

Go into the `buildscripts` directory and run `./build.sh`. The script will automatically download the Android native toolchain and all dependencies, and will compile and install them.

### Step 2: Build the Java launcher

To get an APK file you can install, open the `openmw-android` directory in Android Studio and run the project.

Alternatively, if you do not have Android Studio installed or would rather not use it, run `./gradlew assembleDebug` from the root directory of this repository. The resulting APK, located at `./app/build/outputs/apk/debug/app-debug.apk`, can be transferred to the device and installed.

## Notes for developers

### Debugging native code

You can debug native code with `ndk-gdb`. To use it, once you've built both libraries and the apk and installed the apk, run the application and let it stay on the main menu. Then `cd` to `app/src/main` and run `./gdb.sh [arch]`. The `arch` variable has to match the library your device will be using (one of `arm`, `arm64`, `x86_64`, `x86`; `arm` is the default).

This also automatically enables gdb to use unstripped libraries, so you get proper symbols, source code references, etc.

### Running Address Sanitizer

To compile everything with ASAN:

```
# Clean previous build
./clean.sh
# Build with ASAN enabled & debug symbols
./build.sh --ccache --asan --debug
# Or: ./build.sh --ccache --asan --debug --arch arm64
```

Then open Android Studio and compile and install the project.

To get symbolized output:

```
adb logcat | ./tool/asan_symbolize.py --demangle -s ./symbols/armeabi-v7a/
# Or: adb logcat | ./tool/asan_symbolize.py --demangle -s ./symbols/arm64-v8a/
```

## Credits

### Source code

Original Java code written by sandstranger. Build scripts originally written by sandstranger and bwhaines.
