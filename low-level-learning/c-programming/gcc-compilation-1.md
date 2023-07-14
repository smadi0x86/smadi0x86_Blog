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

# GCC Compilation

#### <mark style="color:purple;">GCC (upper case) refers to the GNU Compiler Collection.</mark>

GCC is an open source compiler suite which include compilers for C, C++, Objective C, Fortran, Ada, Go and Java.&#x20;

#### <mark style="color:purple;">gcc (lower case) is the C compiler in the GNU Compiler Collection.</mark>

The GCC project has detailed documentation at [https://gcc.gnu.org](https://gcc.gnu.org) which document installation, general usage, and every command line option. Please refer to the official GCC documentation on any question not answered here.

## <mark style="color:red;">Compilation steps</mark>

### <mark style="color:yellow;">1) Preprocessing</mark>

#### <mark style="color:purple;">The preprocessor obeys commands that begin with # (known as directives) by:</mark>

* removing comments
* expanding macros
* expanding included files

{% hint style="info" %}
If you included a header file such as #include \<stdio.h>, it will look for the stdio.h file and copy the header file into the source code file.
{% endhint %}

The preprocessor also generates macro code and replaces symbolic constants defined using #define with their values.

### <mark style="color:yellow;">2)</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**Compiling**</mark>

It takes the output of the preprocessor and generates assembly language, an intermediate human readable language, specific to the target processor.

### <mark style="color:yellow;">3) Assembly</mark>

The assembler will convert the assembly code into pure binary code or machine code (0s & 1s). This code is also known as object code.

### <mark style="color:yellow;">4) Linking</mark>

The linker merges all the object code from multiple modules into a single one.&#x20;

If we are using a function from libraries, linker will link our code with that library function code.

#### <mark style="color:purple;">In static linking, the linker makes a copy of all used library functions to the executable file.</mark>&#x20;

#### <mark style="color:purple;">In dynamic linking, the code is not copied, it is done by just placing the name of the library in the binary file.</mark>

## <mark style="color:red;">Basic GCC syntax</mark>

Here is a basic usage of gcc for compiling a non-threaded C code with no external linking and libraries (except for standard C libraries) on linux systems:

```c
gcc app.c -o app
```

#### <mark style="color:purple;">This will do all the steps discussed above and generate an executable (ELF) file named app.</mark>

## <mark style="color:red;">Useful GCC options</mark>

These are some of the most used gcc compilation options that you will usually use, for more info and options refer to [gcc official documentation](https://gcc.gnu.org/onlinedocs/).

```bash
-Wall → enables all warnings.

-E → shows the code that preprocessors use.

-S → only assembly code.

-save-temp → save all the .o files (produce everything).

-l → link to a file.

m → for math library while using -l (link to math library).

-fPIC → create a shared library ( then use -shared -o prog.so prog.o).

-g → generates debugging info.

-v → verbose on every step.

-ansi → make sure thing are compatible with the ansi standard.

-funsigned-char →  char type is threated as unsigned type.

-fsigned-char → the aposite of the above options.

-DMY_MACRO → define MY_MACRO.

-Werror → all warnings will be threated as errors.
```

## <mark style="color:red;">Using a config file</mark>

To avoid specifying the options each time you want to compile a file, you can specify a configuration file including all the options like this:

```bash
@ → use with a file name containing gcc options for example a file with this content:

opt_file:

-Wall -g 

use it like this:

gcc test.c @opt_file → use the options in opt_file  
```

## <mark style="color:red;">C and disassembly</mark>

#### <mark style="color:purple;">​</mark><mark style="color:purple;">**The gcc option -O enables different levels of optimization.**</mark>&#x20;

Use **-O0** to disable them and use **-S** to output assembly.&#x20;

**-O3** is the highest level of optimization.

Starting with **gcc 4.8** the optimization level **-Og** is available.&#x20;

It enables optimizations that do not interfere with debugging and is the recommended default for the standard edit-compile-debug cycle.

#### <mark style="color:purple;">**To change the dialect of the assembly to either intel or att use -masm=intel or -masm=att.**</mark>

You can also enable certain optimizations manually with **-fname**.

## <mark style="color:red;">​Optimization Flags</mark>

#### <mark style="color:purple;">Use gcc with -Q --help=optimizers to find the exact set of optimizations at each level:</mark>

{% code overflow="wrap" %}
```bash
-O → tries to reduce code size and execution time without performing any optimizations that make a great deal of compilation time.

-O1 → takes more time and a lot more memory.

-O2 → optimize even more. performs nearlly all supported optimizations that do not involve a spade-speed tradeoff.

-O3 → turns on all optimizations.

-Ofast → turns on everything including the optimizations that are not valid for all standard-compilant programs.

-Og → optimize debugging experience.
```
{% endcode %}

## <mark style="color:red;">Strip and reduce binary size</mark>

{% code overflow="wrap" %}
```bash
-Os -> this flag enables size optimization, focusing on reducing the size of the resulting binary. It applies various optimization techniques to achieve smaller code size.

-s -> this flag tells GCC to strip symbols from the compiled binary, reducing its size. This makes it harder to analyze the binary and extract meaningful information from it.

-flto -> this flag enables link-time optimization, Link-time optimization can help reduce code size by eliminating redundant code and optimizing function calls.

-ffunction-sections and -fdata-sections -> these flags enable the generation of individual sections for each function and data object. 

--gc-sections -> removal of unused functions or data sections during the linking phase.

-Wl and --gc-sections -> this linker flag removes unused sections from the final binary, further reducing its size by eliminating unused code and data sections.
```
{% endcode %}

## <mark style="color:red;">GCC Environment Variables</mark>

1. **PATH** **→** for searching the executables and run-time shared libraries (.dll, .so).
2. **CPATH** **→** for searching the include-paths for headers searched after paths specified in -l options .
3. **C\_INCLUDE\_PATH** **->** for specifying C headers if the particular language was indicated in pre-processing.
4. **LIBRARY\_PATH** **→** for searching library-paths for link libraries. it is searching after paths specified in -L options.
