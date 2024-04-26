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

# Shared Libraries (Dynamic Linking)

## The Efficiency Advantage

Imagine multiple programs needing the same functionality, like mathematical operations. Traditionally, each program would have a static copy of the math library code compiled into its executable. This leads to wasted space – the same code gets stored repeatedly.

Shared libraries come to the rescue! They reside in a single location on the system (e.g., `/usr/lib` on Linux). When a program needs the functionality, the operating system loads the shared library at runtime and makes it available to the program. This way, multiple programs can share a single copy of the code, saving precious disk space.

## Code Reusability

Shared libraries promote code reusability. You can create custom libraries containing well-tested functions and data structures.&#x20;

These libraries can then be linked with different programs, reducing code duplication and improving maintainability. Updates to the shared library can be applied once, benefiting all linked programs.

## Advantages and Potential Drawbacks

### **Advantages**

* **Reduced disk space:** Shared libraries avoid redundant code in executables.
* **Improved code reusability:** Libraries encapsulate functionality for reuse across projects.
* **Simplified updates:** Updates to a shared library affect all linked programs.

### **Drawbacks**

* R**untime dependencies:** Programs rely on the shared library being available at runtime. Missing libraries can cause errors.
* **Increased complexity**: Managing library versions and dependencies requires extra attention.
* **Potential for conflicts:** Multiple programs using different versions of the same library might lead to issues.

## Shared Library Mechanics

### **Creating Shared Libraries**

Compilers provide options (e.g., `-shared` for GCC) to generate shared libraries from source code. The linker creates a single file with the code and information for the dynamic linker to resolve references at runtime.

### **Loading at Runtime**

When a program starts and needs a shared library, the operating system's dynamic linker locates the library based on search paths (system-wide or environment variables) and loads it into memory.

### **Symbol Resolution**

The dynamic linker identifies unresolved symbols (functions and variables) in the program and matches them with corresponding definitions in the loaded shared library. This ensures everything works seamlessly.

## Library Naming Conventions

#### Shared libraries have platform-specific naming conventions:

* Linux: `.so` (e.g., `libmath.so`)
* Windows: `.dll` (e.g., `math.dll`)
* macOS: `.dylib` (e.g., `libmath.dylib`)

## Dependency Management

Managing dependencies for shared libraries is crucial. Tools like `ldd` (Linux) or Dependency Walker (Windows) can help analyze a program's shared library dependencies. Package managers on various platforms often handle library installation and versioning.

CMake's `find_package` helps locate and link with shared libraries during the build process.

## Versioning and Compatibility

Shared libraries have versions (e.g., `libmath.so.3.2`) to ensure compatibility with applications. Major version changes might introduce breaking changes, requiring program updates to work with the new library. Tools like `ldconfig` (Linux) update the dynamic linker cache to reflect library versions available on the system.

**Remember:**

* Shared libraries are powerful for code reuse and efficiency, but they introduce runtime dependencies and versioning complexities.
* Carefully consider the trade-offs before using shared libraries in your projects.
