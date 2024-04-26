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

# Fundamentals for Linking

This guide delves into the core concepts of linking in C/C++, equipping you with the knowledge to build robust and portable applications. We'll explore object files, libraries, compilation, linking, symbol resolution, and more!

## Object Files and Libraries

### **Object Files**

Imagine a program as a house. Source code (`.c` or `.cpp` files) are like blueprints. Compiling these files translates them into object files (`.o` or `.obj`), which contain machine code instructions the CPU understands. These object files are the individual building blocks of your program.

### **Libraries**

Libraries are collections of pre-compiled object files, acting like pre-built components for your house.&#x20;

#### They come in two main flavors:

* **Static Libraries (`.a` on Linux/macOS, `.lib` on Windows):** These libraries archive multiple object files into a single unit. When you link your program, all the object files from the static library are copied into your final executable, making it self-contained but potentially larger.
* **Shared Libraries (`.so` on Linux, `.dll` on Windows):** Shared libraries reside on the system and are loaded by your program at runtime. This saves disk space (one copy of the library for multiple programs) and memory (loaded only when needed). However, it introduces a dependency – your program relies on the shared library being available on the system.

#### **Library Naming Convention:**

* The typical naming convention for libraries is `lib<name>.so` for shared libraries (e.g., `libmath.so` for the math library) and `lib<name>.a` for static libraries (e.g., `libmyfunctions.a` for a custom library).
* The `lib` prefix tells the linker it's dealing with a library, and the `.so` or `.a` extension indicates the library type (shared or static).

#### **Manually Linking with ldd (Linux):**

The `ldd` (List Dynamic Dependencies) command is a helpful tool for manually checking library dependencies and demonstrating the linking process.&#x20;

For windows `.dll` is used, check the equivalent to `ldd` in windows: [https://stackoverflow.com/questions/1993673/what-is-the-equivalent-of-linuxs-ldd-on-windows](https://stackoverflow.com/questions/1993673/what-is-the-equivalent-of-linuxs-ldd-on-windows)

#### Here's how to use it:

1. **Identify the Library:** Let's say you want to link your program with the math library (`libmath.so`).
2.  **Run ldd:** Execute the following command, replacing `<your_program>` with the actual name of your program:

    ```bash
    ldd <your_program> # .exe or elf executable
    ```



    This will display a list of libraries your program requires, including their paths. Look for an entry similar to `libmath.so => /lib/x86_64-linux-gnu/libmath.so.X.Y (0x00007ffff7a9b000)`

    Here, `libmath.so` is the library name, `/lib/x86_64-linux-gnu/libmath.so.X.Y` is its path on the system, and the address information indicates it's loaded.
3.  **Linking Manually (Not recommended for production):**

    While not recommended for everyday use due to potential issues with library search paths, you can manually specify libraries during linking using the linker flag `-l<library>`.&#x20;



    ```bash
    gcc -o myprogram myprogram.cpp -lmath
    ```



    The `-lmath` flag tells the linker to search for `libmath.so` and link it with your program `myprogram.cpp`.

{% hint style="warning" %}
**Important Note:**

Manually specifying libraries with linker flags is generally discouraged for production builds. It's better to rely on your build system (like CMake) to manage library dependencies automatically. This ensures the linker searches for libraries in the correct locations and avoids potential issues with missing libraries or version conflicts.
{% endhint %}

## Compilation and Linking

#### Building an executable from source code involves two crucial stages:

* **Compilation:** The compiler translates each source file into its corresponding object file. It checks for syntax errors and ensures the code follows the language rules.
* **Linking:** The linker takes the object files (and libraries) and combines them into a single executable program. It resolves references between functions and variables defined in different object files and libraries.

{% hint style="info" %}
**Imagine compilation as assembling the building blocks (object files) and linking as putting them together to form the final structure (executable).**
{% endhint %}

## Symbol Resolution

Programs rely on functions and variables defined throughout the codebase. During compilation, the compiler encounters references to these symbols (function names, variable names) but may not have their definitions yet (if they're in separate files or libraries).

To resolve these references, we use `extern` declarations. The `extern` keyword tells the compiler that a symbol is defined elsewhere (typically in a library) and instructs it to search for it during linking.

Linking then matches symbol references in your object files with their corresponding definitions in libraries, ensuring everything connects smoothly.

## Linker Flags and Options

#### Linkers offer various flags and options to control the linking process. Here are some common examples:

* `-l<library>`: This flag tells the linker to search for a specific library named `<library>`. (e.g., `-lm` for math library)
* `-L<path>`: This flag specifies a directory where the linker should look for libraries.
* `-Wl,<option>`: This flag allows passing platform-specific options to the linker (consult your linker documentation for details).

These flags provide flexibility in specifying libraries and controlling the linking behavior.
