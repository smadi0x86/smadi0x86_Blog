---
cover: https://cdn.pixabay.com/photo/2016/07/27/10/44/fireworks-1544999_1280.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# CMake for Building and Linking

CMake is a powerful cross-platform build system that simplifies building C/C++ projects, especially when it comes to managing libraries and linking.&#x20;

#### Here's a comprehensive look at using CMake for building and linking, complete with examples:

## **Project Setup**

* Create a directory for your project.
* Inside the project directory, create a file named `CMakeLists.txt`. This is the heart of your CMake configuration.
* Create a build directory
* Write your CMakeLists.txt
* Move to build directory
* Run `cmake ..`
* Run `make`

{% hint style="warning" %}
Refer to CMake documentation for specific library names and linking instructions.

Create a separate build directory and run `cmake` there.
{% endhint %}

{% embed url="https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html" %}

{% embed url="https://cppcheatsheet.com/notes/cmake_basic.html" %}

## **Building a Simple Static Library**

```cmake
# Minimum required CMake version
cmake_minimum_required(VERSION 3.0)

# Project name
project(MyStaticLib)

# Create a static library
add_library(MyStaticLib STATIC source1.cpp source2.cpp)
```

#### **Explanation:**

* `cmake_minimum_required(VERSION 3.0)`: This line specifies the minimum required version of CMake for this project.
* `project(MyStaticLib)`: This line defines the project name, which is used for generating build files.
* `add_library(MyStaticLib STATIC source1.cpp source2.cpp)`: This line creates a static library named `MyStaticLib` from the source files `source1.cpp` and `source2.cpp`.

## **Building an Executable and Linking with the Static Library**

```cmake
# Include the library directory
target_include_directories(MyProgram PRIVATE "${CMAKE_CURRENT_BINARY_DIR}")

# Create an executable
add_executable(MyProgram main.cpp)

# Link the executable with the static library
target_link_libraries(MyProgram PRIVATE MyStaticLib)
```

#### **Explanation:**

* `target_include_directories(MyProgram PRIVATE "${CMAKE_CURRENT_BINARY_DIR}")`: This line tells the compiler where to find header files. Since the static library might have headers, we include the current binary directory (where the library is built) in the search path.
* `add_executable(MyProgram main.cpp)`: This line creates an executable named `MyProgram` from the source file `main.cpp`.
* `target_link_libraries(MyProgram PRIVATE MyStaticLib)`: This line links the executable `MyProgram` with the previously created static library `MyStaticLib`. The `PRIVATE` keyword indicates that the library is private to this project and shouldn't be exposed to other projects linking against this executable.

## **Building a Shared Library**

```cmake
# Create a shared library
add_library(MySharedLib SHARED source1.cpp source2.cpp)
```

#### **Explanation:**

This line is similar to creating a static library, but instead of `STATIC`, we use `SHARED` to create a shared library named `MySharedLib`.

## **Linking with External Libraries using find\_package**

```cmake
# Find an external library (replace with the actual library)
find_package(Threads REQUIRED)

# Include the library directory
target_include_directories(MyProgram PRIVATE "${CMAKE_THREAD_INCLUDE_DIRS}")

# Link the executable with the external library
target_link_libraries(MyProgram PRIVATE Threads::Threads)
```

#### **Explanation:**

* `find_package(Threads REQUIRED)`: This line attempts to find a package named `Threads` (replace with the actual library name, like `OpenSSL` or `SDL2`). The `REQUIRED` keyword ensures CMake throws an error if the library is not found.
* `target_include_directories(MyProgram PRIVATE "${CMAKE_THREAD_INCLUDE_DIRS}")`: This line includes the library's header directory (`CMAKE_THREAD_INCLUDE_DIRS`) in the search path for `MyProgram`.
* `target_link_libraries(MyProgram PRIVATE Threads::Threads)`: This line links the executable with the external library's target (`Threads::Threads`).

## **Advanced Linking Options and Flags**

### **Visibility Control**

You can control which symbols from a library are visible to linked programs using compiler flags like `-fvisibility=hidden` (GCC) or `/DEF:{symbol}` (MSVC).&#x20;

#### CMake can help manage these flags through target properties:

```cmake
if(CMAKE_C_COMPILER_ID MATCHES "GNU")
  target_compile_properties(MyLib PROPERTIES CVISIBILITY_PRESET "hidden")
endif()
```

#### **Explanation:**

* The `if` statement checks the compiler ID.
* If it's GCC, `target_compile_properties` sets the `CVISIBILITY_PRESET` property to "hidden" for the target `MyLib`, effectively hiding symbols by default.
* **Custom Linker Flags:** You can pass custom linker flags using `target_link_libraries` with the `LINK_OPTIONS` clause:

{% code overflow="wrap" %}
```cmake
target_link_libraries(MyProgram PRIVATE MyLib LINK_OPTIONS "-Wl,-rpath,/path/to/additional/libraries")
```
{% endcode %}

#### **Explanation:**

* This line links the program `MyProgram` with `MyLib` and adds the linker flag `-Wl,-rpath,/path/to/additional/libraries`, specifying an additional search path for libraries at runtime.

## **Cross-Platform Considerations**

* **Toolchain Files:** CMake allows defining toolchain files that specify compiler and linker settings for different platforms. This enables consistent builds across operating systems.
* **Platform-Specific Flags:** Be mindful of platform-specific compiler and linker flags. CMake can handle some of these through built-in properties like `CMAKE_CXX_FLAGS` or `CMAKE_EXE_LINKER_FLAGS`, but you might need to add custom flags for specific needs.

#### Example of a cross platform build:

{% code overflow="wrap" %}
```cmake
cmake_minimum_required(VERSION 3.0)

project(MyMultithreadedApp)

# Find pthread library (replace with the actual find_package call for your target platform)
find_package(Threads REQUIRED)

# Include directories (adjust based on your header locations)
target_include_directories(MyProgram PRIVATE
  "${CMAKE_CURRENT_BINARY_DIR}"  # Include current directory for project headers
  "${CMAKE_THREAD_INCLUDE_DIRS}"  # Include directory from Threads package
)

# Source files (replace with your actual source files)
add_executable(MyProgram main.cpp worker.cpp)

# Link with pthread library
target_link_libraries(MyProgram PRIVATE Threads::Threads)

# Platform-specific flags (optional)
if(CMAKE_SYSTEM_NAME MATCHES "Linux")
  target_link_libraries(MyProgram PRIVATE "-lpthread")  # Link with pthread on Linux
endif()

if(CMAKE_SYSTEM_NAME MATCHES "Darwin")
  target_link_libraries(MyProgram PRIVATE "-framework pthread")  # Link with pthread on macOS
endif()
```
{% endcode %}

#### **Explanation:**

1. **CMake Minimum Version:** We specify the minimum required CMake version.
2. **Project Name:** We define the project name.
3. **Finding Threads Library:** We use `find_package(Threads REQUIRED)` to attempt finding the `pthread` library. This might require replacing `Threads` with the actual package name for your target platform (e.g., `PkgConfig` on some Linux distributions).
4. **Include Directories:** We set the include directories for our project headers and the headers from the `Threads` package.
5. **Source Files:** We define the source files for our application.
6. **Linking with Threads:** We link the executable `MyProgram` with the target `Threads::Threads` from the found library.
7. **Platform-Specific Flags (Optional):** We use conditional statements to add platform-specific linker flags. On Linux, we link with `-lpthread`, and on macOS, we link with the `-framework pthread` framework.
