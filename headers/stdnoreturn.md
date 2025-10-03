# **Complete Reference: stdnoreturn.h Header in C**

## **Overview**

The **`<stdnoreturn.h>`** header was introduced in **C11** and provides a convenience macro for specifying functions that do not return to their caller. **This header is deprecated in C23** and replaced by the `[[noreturn]]` attribute.

## **Header Status**

- **Introduced:** C11 (2011)
- **Current Status:** Deprecated in C23 (2024)
- **Replacement:** `[[noreturn]]` attribute in C23

## **Contents**

### **Macros**

#### **`noreturn`**

- **Type:** Convenience macro
- **Expands to:** `_Noreturn`
- **Purpose:** Provides a more readable way to specify non-returning functions
- **Usage:** Applied as a function specifier before function declarations
- **Return Type:** N/A (macro expansion)
- **Introduced:** C11
- **Status:** Deprecated in C23

The `<stdnoreturn.h>` header defines **exactly one macro**:

```c
#include <stdnoreturn.h>
#define noreturn _Noreturn
// Equivalent to: _Noreturn void terminate_program(void);
```

This is the complete content of the header. The macro serves as a convenience wrapper around the `_Noreturn` keyword.

### **Keywords Referenced**

#### **`_Noreturn`**

- **Type:** Function specifier (keyword)
- **Purpose:** Indicates that a function does not return by executing a return statement or reaching the end of function body
- **Usage:** Can appear before or after return type in function declarations
- **Return Type:** Functions marked with `_Noreturn` should have `void` return type
- **Introduced:** C11
- **Status:** Deprecated in C23 (replaced by `[[noreturn]]` attribute)

**Syntax:**

```c
_Noreturn void function_name(parameters);
// or
void _Noreturn function_name(parameters);
```

## **Complete Usage Examples**

### **Example 1: Basic Usage with noreturn Macro**

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>

noreturn void terminate_program(void) {
    printf("Terminating program...\n");
    exit(1);
}

int main(void) {
    printf("Program started\n");
    terminate_program();
    printf("This line will never be reached\n"); // Unreachable code
    return 0;
}
```

**Output:**

```txt
Program started
Terminating program...
```

### **Example 2: Using _Noreturn Keyword Directly**

```c
#include <stdio.h>
#include <stdlib.h>

_Noreturn void fatal_error(const char* message) {
    fprintf(stderr, "Fatal error: %s\n", message);
    abort();
}

int main(void) {
    printf("Starting application\n");
    fatal_error("Something went wrong");
    return 0; // Never reached
}
```

**Output:**

```txt
Starting application
Fatal error: Something went wrong
```

***Program terminates abnormally***

### **Example 3: Multiple _Noreturn Specifiers (Valid)**

```c
#include <stdlib.h>

// Multiple _Noreturn specifiers are allowed
_Noreturn _Noreturn void exit_function(void) {
    exit(0);
}
// Equivalent to single _Noreturn
```

### **Example 4: Invalid Usage (Undefined Behavior)**

```c
#include <stdio.h>
#include <stdnoreturn.h>

// WARNING: This is INVALID - causes undefined behavior
// because the function CAN return if i <= 0
noreturn void bad_function(int i) {
    if (i > 0) {
        exit(1);
    }
    // If i <= 0, function returns normally - undefined behavior!
}
```

### **Example 5: Infinite Loop (Valid noreturn)**

```c
#include <stdio.h>
#include <stdnoreturn.h>

noreturn void infinite_loop(void) {
    while (1) {
        printf("Running forever...\n");
    }
    // Function never returns due to infinite loop
}
```

### **Example 6: C23 Replacement with [[noreturn]]**

```c
// C23 version using [[noreturn]] attribute
#include <stdio.h>
#include <stdlib.h>

[[noreturn]] void terminate_c23(void) {
    printf("Using C23 noreturn attribute\n");
    exit(1);
}

int main(void) {
    terminate_c23();
    return 0;
}
```

**Output:**

```txt
Using C23 noreturn attribute
```

## **Standard Library Functions with noreturn**

The following standard library functions are inherently `noreturn`:

1. `abort()` - from `<stdlib.h>`
2. `exit()` - from `<stdlib.h>`
3. `_Exit()` - from `<stdlib.h>`
4. `quick_exit()` - from `<stdlib.h>` (C11)
5. `thrd_exit()` - from `<threads.h>` (C11)

## **Compiler Behavior and Optimizations**

### **Benefits of Using noreturn**

1. **Code Optimization:** Compiler can optimize without considering return paths
2. **Warning Suppression:** Eliminates "uninitialized variable" warnings in unreachable code
3. **Static Analysis:** Helps static analysis tools understand control flow
4. **Documentation:** Makes programmer intent clear

### **Compiler Diagnostics**

- Compilers should produce diagnostic messages if a `_Noreturn` function appears capable of returning
- If a `_Noreturn` function actually returns, behavior is undefined

## **Important Rules and Constraints**

### **Valid Usage**

- Function must never return via `return` statement
- Function must never reach end of function body
- Function can exit via `exit()`, `abort()`, `longjmp()`, infinite loops, etc.
- Multiple `_Noreturn` specifiers in same declaration are allowed
- Should only be used with `void` return type

### **Invalid Usage (Undefined Behavior)**

- Function marked `_Noreturn` that can return normally
- Using explicit `return` statement in `_Noreturn` function
- Reaching end of function body in `_Noreturn` function

## **Migration from C11/C17 to C23**

### **C11/C17 Code**

```c
#include <stdnoreturn.h>

noreturn void old_style(void) {
    exit(1);
}
```

### **C23 Equivalent**

```c
[[noreturn]] void new_style(void) {
    exit(1);
}
```

### **Compatibility Considerations**

- `<stdnoreturn.h>` header becomes obsolescent in C23
- `_Noreturn` keyword deprecated but still available
- `noreturn` macro deprecated
- New `[[noreturn]]` attribute is preferred

## **Compiler Support and Portability**

### **Feature Detection**

```c
#ifdef __STDC_VERSION__
    #if __STDC_VERSION__ >= 202311L
        // C23: Use [[noreturn]]
        [[noreturn]] void func(void);
    #elif __STDC_VERSION__ >= 201112L
        // C11/C17: Use _Noreturn or noreturn
        #include <stdnoreturn.h>
        noreturn void func(void);
    #endif
#endif
```

### **GCC Compatibility**

GCC provides `__attribute__((noreturn))` which serves similar functionality. However, including `<stdnoreturn.h>` can cause conflicts because the `noreturn` macro interferes with the attribute syntax.

### **Microsoft Visual C++**

MSVC provides `__declspec(noreturn)` for similar functionality. The C11 `_Noreturn` keyword and `noreturn` macro are supported in newer versions.

### **Cross-Compiler Compatibility**

```c
#ifndef __has_c_attribute
    #define __has_c_attribute(x) 0
#endif

#if __has_c_attribute(__noreturn__)
    #define NORETURN [[__noreturn__]]
#elif defined(__STDC_VERSION__) && __STDC_VERSION__ >= 201112L
    #define NORETURN _Noreturn
#elif defined(__GNUC__)
    #define NORETURN __attribute__((noreturn))
#elif defined(_MSC_VER)
    #define NORETURN __declspec(noreturn)
#else
    #define NORETURN
#endif

NORETURN void portable_exit(void) {
    exit(1);
}
```

## **Summary**

The `<stdnoreturn.h>` header provides minimal but crucial functionality for indicating non-returning functions in C11 and C17. It contains exactly **one macro** (`noreturn` expanding to `_Noreturn`) and **no functions or additional keywords**. While deprecated in C23 in favor of the `[[noreturn]]` attribute, understanding its usage remains essential for maintaining legacy code and ensuring compatibility across different C standard versions. The header represents one of the smallest standard library headers, serving as a simple convenience wrapper around the `_Noreturn` function specifier.
