# Build Statically-Linked QT from Sources

This is an instruction for creating a static linking (assembly) for QT 6.2.0, with the help of this instruction you will be able to make portable versions of your projects.

>[!Attention]
>All parameters are set according to the EXAMPLE, you may have a different value of the fields!

## Prerequisites

1. [Online installer](https://www.qt.io/download-qt-installer?hsCtaTracking=99d9dd4f-5681-48d2-b096-470725510d34%7C074ddad0-fdef-4e53-8aa8-5e8a876d6ab4)
2. [Visual Studio 2019](https://visualstudio.microsoft.com/ru/downloads/)
3. Qt source code
4. [Strawberry Perl 5+](https://strawberryperl.com/)
5. [Python 3.5+](https://www.python.org/)
6. CMake 3.25+ (https://cmake.org/download/)
7. Ninja (https://github.com/ninja-build/ninja/releases)

## Installation of the necessary components

- QT installation is acceptable with the parameters you require, QT(Sources) sources must be installed
- Installing Visual Studio 2019 is valid with the settings you require
- Installing Strawberry Perl is desirable with the default settings
- Python installation is desirable with default settings (Add to PATH checkbox, must be enabled)
- Installing CMake is desirable with default settings
- Ninja is downloaded as an archive and unpacked to a convenient place for you, for me it is "C:\ninja". After unpacking, there should only be the executable "ninja.exe"

## Checking the location of the required components
- Be sure to check in CMD running as administrator, in parentheses commands to be called! The following are mandatory for verification:

   1. Strawberry Perl (`where perl.exe`)
   2. Python (`where python.exe`)
   3. CMake (`where cmake.exe`)
   4. Ninja (`where ninja.exe)`

If it gives "INFORMATION: I can't find files according to the specified templates.", then you need to manually specify the path for the desired component in CMD

```
set "<COMPONENT NAME>_ROOT=<PATH TO COMPONENT EXECUTIVE FILES>"
set PATH=%<COMPONENT NAME>_ROOT%;%PATH%
Example:

set "CMAKE_ROOT=C:\Program Files\CMake\bin"
set PATH=%CMAKE_ROOT%;%PATH%
```

After the procedure is done, call the where command for the required component, the error should disappear!

## Installation and assembly procedure

1. Download and install the necessary components, checking them with the where command, in case of errors, do the procedures described above!
2. Download QT Online Installer and run it. Go through authorization and other stages until you get to the component selection window!
3. Select the Qt 6.2.0 version, expand the item and check the boxes:

      - [X] MSVC 2019 64-bit
      - [X] Sources
      - [X] Qt Quick 3D
      - [X] Qt 5 Compatibility Module
      - [X] Qt Shader Tools

4. Additional Libraries
      - [x] Qt Debug Information Files
      - [x] Qt Quick Timeline

5. Go down below where the Developer and Designer Tools are located, expand the item:
      - [X] Qt Creator X.X.X

 ## Qt Creator X.X.X CDB Debugger Support

Debugging Tools for Windows: Unchecking the CMake and Ninja checkboxes, as we installed them earlier. 

>[!Attention]
>I set such settings, you can remove the checkboxes or enable new ones! We are waiting for the installation to be completed!

Install Visual Studio (if it is not installed), the main thing here is to choose an SDK that is compatible for your version of QT and compilation tools!

Open x64 Native Tools Command Prompt for VS 2019 5.1 Go to the directory with QT installed (in my case, it is D:\QT\6.2.0

**If on the C drive:**
```
cd C:\QT\6.2.0\
```

**If on drive D:**
```
cd /d D:\QT\6.2.0\
```

## 5.2 

Create a folder to build
```
mkdir build
cd build
Run the configuration script
<Path to source code of Qt>\configure.bat -debug-and-release -static -static-runtime -opensource -confirm-license -platform win32-msvc -qt-zlib -qt-libpng -qt-libjpeg -nomake examples -nomake tests -no-opengl -skip qtscript -skip qtquick3d -prefix "<Output Directory>"
```

### Options

`skip <module>` - excludes a separate submodule from the assembly process
`nomake examples` - excludes sample programs from the build process
`nomake tests` - excludes tests from the build process
`platform <platform>` - defines the platform for which Qt will be built, in this case Windows with MSVC
`no-opengl` - Do not use OpenGL to render the interface
`qt-zlib` - use the libraries shipped with Qtqt-libpngqt-libjpegzliblibpnglibjpeg
`opensource` - use the open source version of Qt
`confirm-license` - automatically accept the Qt license
`debug-and-release` - assembly optionsreleasedebug
`static` - enable static Qt and runtime linkingstatic-runtime
`prefix <prefix>` - the path to the folder to which the compiled Qt files will be copied 6.1 Waiting for the end of the process! After that, the result will be written at the end, without errors it looks like this: Qt is now configured for building. Just run 

```
cmake --build . --parallel
```

### HERE IS THE BEGINNING 
***
"Once everything is built, you must run `ninja install` 
Qt will be installed into 'D:/QT/6.2.0/MSCV-Static'

To configure and build other Qt modules, you can use the following convenience script: D:/QT/6.2.0/MSCV-Static/bin/qt-configure-module.bat

If reconfiguration fails for some reason, try to remove 'CMakeCache.txt' from the build directory
```
-- Configuring done (306.1s) -- Generating done (22.6s) CMake Warning: Manually-specified variables were not used by the project:
```
### BUILD_qtscript

```
-- Build files have been written to: D:/QT/6.2.0/build"``
```
***
### THAT'S THE END


- QT is configured to build, run the

```
cmake --build . --parallel
```

- Wait for the end of the setup (it can take a long time, depends on the CPU and write speed), the console output should look like this:
```
[11790/11790] Linking CXX static library qtbase\qml\QtWebView\qtwebviewquickplugind.lib
```
After 7.1 is finished, run the
```
ninja install
```

After the assembly is finished, we can go to QT to set up the profile and kit!
