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

# Libraries & Linking

## <mark style="color:red;">**Introduction**</mark>

* Libraries are crucial to ensure reusability and modularity of code.
* They contain one or more object files, which consist of object code.
* C functions beneficial to multiple applications should be compartmentalized into libraries.
* Libraries can reference functions in other libraries, e.g., standard C or math libraries.

## <mark style="color:red;">**Interface and Implementation**</mark>

* **Interface:** Defined in a header file (`.h`).
* **Implementation:** Defined in a `.c` file.

## <mark style="color:red;">**Linking**</mark>

* When a C program is compiled, the compiler generates object code (`.o` or `.obj`).
* After generating object code, the compiler invokes the linker.
* Linking combines multiple object files to create a single executable file.
* Main goal: Make the library functions available to your program.
  * Can copy the library function code to object code.
  * Alternatively, it can ensure the code is available at runtime.
* **Static Linking:** Library functions are copied to the executable file.
* **Dynamic Linking:** Only the name of the library is in the binary file; the actual linking occurs when the program runs.

## <mark style="color:red;">**Static vs. Dynamic Linking**</mark>

#### <mark style="color:purple;">**Static Linking:**</mark>

* Libraries are linked at compile-time.
* Advantages: No runtime symbol resolution; once bundled, you're certain the correct library version is in use.
* Disadvantages: Creates larger binaries; no way to update the library without rebuilding the whole program.

#### <mark style="color:purple;">**Dynamic Linking:**</mark>

* Libraries are linked at runtime.
* Advantages: Saves on disk and memory; all programs linked to a library share a single copy of that library in memory.
* Disadvantages: Small runtime penalty due to symbol resolution at runtime.

## <mark style="color:red;">**Library Types**</mark>

#### <mark style="color:purple;">**Static Libraries (.a or .lib):**</mark>&#x20;

Uses static linking; each process has its own copy of code and data.

#### <mark style="color:purple;">**Dynamic Libraries (.so or .dll):**</mark>&#x20;

Linked at runtime; code is shared but data is specific to each process.

## <mark style="color:red;">**Creating Libraries**</mark>

#### <mark style="color:purple;">**Static Libraries:**</mark>&#x20;

Created using `ar` (archive) utility. Typical naming convention: `lib<name>.a`.

#### <mark style="color:purple;">**Dynamic Libraries:**</mark>&#x20;

Created using the link editor (`ld`). Typical naming convention: `lib<name>.so` or `.dll`.

## <mark style="color:red;">**Library Loading**</mark>&#x20;

### <mark style="color:yellow;">**Introduction**</mark>&#x20;

When a program uses a library, the library's contents must be made available to the program either at compile-time or runtime.&#x20;

This process is called "loading". Depending on when and how the library's content is made available, loading can be categorized into different types.

### <mark style="color:yellow;">**Types of Library Loading**</mark>

#### <mark style="color:purple;">**Static Loading (or Load-time Linking):**</mark>

* Occurs at compile time.
* The code from the library is incorporated into the final executable during the linking phase.
* The resulting binary becomes larger because it contains the code it needs.
* If the library updates, the program must be re-linked and possibly recompiled to benefit from the changes.

#### <mark style="color:purple;">**Dynamic Loading (or Run-time Linking):**</mark>

* Occurs at runtime.
* The library's code isn't included in the executable but is accessed as needed while the program runs.
* Requires that the library is present on the system during execution.
* The program can benefit from updates to a library without recompilation, just a restart.

### <mark style="color:yellow;">**Static vs Dynamic Loading**</mark>

#### <mark style="color:purple;">**Advantages of Static Loading:**</mark>

* No dependencies required at runtime: Since all the code is bundled within the executable, there's no need for external libraries at runtime.
* Execution speed: Directly contains all the necessary library code, which might make it slightly faster at startup.

#### <mark style="color:purple;">**Advantages of Dynamic Loading:**</mark>

* Smaller executables: Since library code isn't bundled, the executable size is smaller.
* Shared libraries: Multiple programs can use a single library copy, conserving memory.
* Updates: Programs can benefit from library updates without recompilation.

### <mark style="color:yellow;">**Dynamic Loading Mechanics**</mark>&#x20;

#### <mark style="color:purple;">When a program requires a function from a dynamically loaded library:</mark>

* The loader checks if the library is already loaded into memory.
* If not, it finds the library on disk (using system paths and environment variables like `LD_LIBRARY_PATH`).
* The loader allocates memory and loads the library.
* The program can then access and execute the library's functions.

### <mark style="color:yellow;">**Dynamic Loading API**</mark>&#x20;

The dynamic loading API allows explicit control over the loading and unloading of shared libraries at runtime.

* **dlopen()**: Opens a dynamic library and returns a handle.
  * `RTLD_LAZY`: Resolves symbols as needed.
  * `RTLD_NOW`: Resolves all symbols immediately.
* **dlsym()**: Retrieves the address of a function or variable from the library using the handle.
* **dlclose()**: Closes an opened library.
* **dlerror()**: Fetches a human-readable error string for the most recent dynamic loading API error.

### <mark style="color:yellow;">**Dynamic Loading Practical Uses**</mark>

* **Plugins**: Software can be extended without modifying the core executable.
* **Modular Programs**: Only load parts of the software that are needed, conserving resources.
* **Optional Features**: If a library isn't present, the software can still run with reduced functionality.

## <mark style="color:red;">**Dynamic Loading Code Examples**</mark>

### <mark style="color:yellow;">**Loading a Dynamic Library**</mark>

```c
#include <stdio.h>
#include <dlfcn.h>

int main() {
    // Load the math library
    void *handle = dlopen("libm.so.6", RTLD_LAZY);
    
    if (!handle) {
        fprintf(stderr, "Error loading library: %s\n", dlerror());
        return 1;
    }

    printf("Library loaded successfully.\n");

    // Close the library
    dlclose(handle);
    return 0;
}
```

### <mark style="color:yellow;">**Using a Function from a Dynamically Loaded Library**</mark>

#### <mark style="color:purple;">This example will dynamically load the math library (</mark><mark style="color:purple;">`libm`</mark><mark style="color:purple;">) and utilize the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`cos`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">function:</mark>

```c
#include <stdio.h>
#include <dlfcn.h>

int main() {
    void *handle = dlopen("libm.so.6", RTLD_LAZY);
    if (!handle) {
        fprintf(stderr, "Error loading library: %s\n", dlerror());
        return 1;
    }

    // Get the cosine function
    double (*cosine)(double) = dlsym(handle, "cos");
    if (dlerror() != NULL) {
        fprintf(stderr, "Error loading function: %s\n", dlerror());
        dlclose(handle);
        return 1;
    }

    // Use the cosine function
    double result = cosine(1.0);
    printf("cos(1) = %f\n", result);

    dlclose(handle);
    return 0;
}
```

### <mark style="color:yellow;">**Dynamically Loading Optional Features**</mark>

Suppose we have a feature in our software that relies on a specific library. If the library is present, we use the feature; otherwise, the software runs with reduced functionality.

```c
#include <stdio.h>
#include <dlfcn.h>

int main() {
    void *handle = dlopen("libOptionalFeature.so", RTLD_LAZY);
    if (!handle) {
        printf("Running software without optional feature.\n");
    } else {
        void (*optionalFeature)(void) = dlsym(handle, "runFeature");
        if (dlerror() != NULL) {
            fprintf(stderr, "Error loading function: %s\n", dlerror());
            dlclose(handle);
            return 1;
        }

        // Use the optional feature
        optionalFeature();

        dlclose(handle);
    }

    printf("Software is running.\n");
    return 0;
}
```

#### <mark style="color:purple;">Remember, to compile any of these programs, you would typically use:</mark>

```bash
gcc program_name.c -ldl -o output_name
```

{% hint style="info" %}
The `-ldl` flag is necessary to link against the dynamic loading functions provided by `dlfcn.h`.
{% endhint %}

### <mark style="color:yellow;">**Conclusion**</mark>

While static loading offers simplicity and directness, dynamic loading brings versatility and efficiency, especially for large-scale or modular software.&#x20;

The choice depends on the application requirements, distribution considerations, and the intended user experience.
