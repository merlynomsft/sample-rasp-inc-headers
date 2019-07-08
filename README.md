# sample-rasp-inc-headers
Sample project demonstrating how to reference custom include and library reference paths for Raspberry Pi

Setup notes: using Raspbian OS

## Build Sqllite using vcpkg

```
sudo apt-get install ninja-build
wget https://github.com/microsoft/CMake/releases/download/v3.14.2542820/cmake-3.14.2542820-MSVC_2-Linux-arm.sh
chmod +777 cmake-3.14.2542820-MSVC_2-Linux-arm.sh
./cmake-3.14.2542820-MSVC_2-Linux-arm.sh

export VCPKG_FORCE_SYSTEM_BINARIES=1
# limit parallel builds
export CMAKE_BUILD_PARALLEL_LEVEL=2
export PATH="$HOME/cmake-3.14.2542820-MSVC_2-Linux-arm/bin:$PATH"

mkdir /home/pi/projects/vcpkg
cd /home/pi/projects/vcpkg

git clone https://github.com/microsoft/vcpkg.git 
./bootstrap-vcpkg.sh -useSystemBinaries
./vcpkg install sqllite2tool
```

## Visual Studio
```
Install Visual Studio 2019.  
Ensure 'Linux development with C++' workload is installed

```
### To use the sample
```
Open Blink5.sln
Tools -> Options -> Cross Platform
Click Add and enter host information for Raspberry PI
Right click Blink5 and configure 'Remote Build Machine' to configured connection

```
### To Re-create the sample
```
New Project
Search for 'Raspberry PI' blink template
Follow Getting Started steps for configuring remote connection
Add #include <sqlite3.h> and 'printf("%s\n", sqlite3_libversion());' to main()
Properties -> C/C++ -> General
Update 'Additional Include Directories' to: /home/pi/projects/vcpkg/packages/sqlite3_x64-linux/include;%(AdditionalIncludeDirectories)
Properties -> C/C++ -> Linker -> General
Update 'Additional Library Directories' to /home/pi/projects/vcpkg/packages/sqlite3_x64-linux/debug/lib;%(AdditionalLibraryDirectories)
Properties -> C/C++ -> Linker -> Input
Update 'Library Dependencies' to: 'wiringPi;sqlite3;pthread;dl'

