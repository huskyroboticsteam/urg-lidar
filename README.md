# URG Lidar

This is a driver library for the Hokuyo URG Lidar sensor. Original code was
developed by Hokuyo; this repository contains a version modified by Husky
Robotics. Primarily, most changes are to clean up the repository and add CMake
build configuration.

The original README file can be viewed in `orig_readme.txt`. Note that the install
instructions there will no longer work.

## Installation instructions

These instructions will require you to have a C/C++ compiler, Git, and
CMake. On Ubuntu, do `sudo apt install build-essential cmake git`. On
Mac, install [Homebrew](https://brew.sh) and do `brew install cmake`.

1. Enter a directory where you'd like to place the source code; we'll use the home directory (`~`):
```bash
cd ~
```
2. Clone the repository:
```bash
git clone https://github.com/huskyroboticsteam/urg-lidar
```
3. Enter the repository directory:
```bash
cd urg-lidar
```
4. Create and enter a directory to hold the build files:
```bash
mkdir -p build && cd build
```
5. Configure CMake:
```bash
cmake ..
```
6. Build and install the library:
```bash
sudo cmake --build . --target install
```

## Including in a project

Once you've done the installation instructions, you can use the library by adding the following line to the top of your `CMakeLists.txt`:
```cmake
find_package(URGLidar REQUIRED)
```
and then add one of the following lines _after_ your `add_executable`/`add_library` commands:
```cmake
## NOTE: Only use ONE of these, depending on whether you want to 
##  use the C or the C++ library!
# For C
target_link_libraries(your_target_name URGLidar::urg_c)
# For C++
target_link_libraries(your_target_name URGLidar::urg_cpp)
```
