# Complete stddef.h Reference for C Programming

## Overview

The **stddef.h** header is one of the foundational headers in the C standard library that defines several essential types and macros used throughout C programming. Despite its fundamental importance, it's often included implicitly by other headers, making it less frequently seen in direct includes.

## Types Defined in stddef.h

### **ptrdiff_t**

**Category:** Signed integer type
**Purpose:** Stores the result of subtracting two pointers
**Declaration:** `typedef signed long ptrdiff_t;` (implementation-defined)
**Standard:** C89
**Printf Format:** `%td`

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>

int main(void) {
    int array[^100];
    int *ptr1 = array + 20;
    int *ptr2 = array + 60;
    
    ptrdiff_t diff = ptr2 - ptr1;  // Result is 40
    printf("Pointer difference: %td\n", diff);
    
    return 0;
}
```

**Expected Output:**

```txt
Pointer difference: 40
```

The `ptrdiff_t` type is essential for portable pointer arithmetic and ensures your code works correctly across different architectures where pointer sizes may vary.

### **size_t**

**Category:** Unsigned integer type
**Purpose:** Result of sizeof operator and array indexing
**Declaration:** `typedef unsigned long size_t;` (implementation-defined)
**Standard:** C89
**Printf Format:** `%zu`

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>

int main(void) {
    size_t int_size = sizeof(int);
    size_t double_size = sizeof(double);
    size_t array_size = sizeof(int[^100]);
    
    printf("Size of int: %zu bytes\n", int_size);
    printf("Size of double: %zu bytes\n", double_size);
    printf("Size of int[^100]: %zu bytes\n", array_size);
    
    return 0;
}
```

**Expected Output:**

```txt
Size of int: 4 bytes
Size of double: 8 bytes
Size of int[^100]: 400 bytes
```

The `size_t` type is crucial for memory management and array operations, guaranteeing it can represent the size of any object in bytes.

### **wchar_t**

**Category:** Wide character type
**Purpose:** Store wide characters for internationalization
**Declaration:** `typedef int wchar_t;` (implementation-defined)
**Standard:** C89
**Printf Format:** `%lc` (character), `%ls` (string)

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>
#include <wchar.h>
#include <locale.h>

int main(void) {
    setlocale(LC_ALL, "");
    
    wchar_t wide_char = L'A';
    wchar_t wide_string[] = L"Hello, 世界!";
    
    printf("Size of wchar_t: %zu bytes\n", sizeof(wchar_t));
    printf("Wide character: %lc\n", wide_char);
    printf("Wide string: %ls\n", wide_string);
    
    return 0;
}
```

**Expected Output:**

```txt
Size of wchar_t: 4 bytes
Wide character: A
Wide string: Hello, 世界!
```

The `wchar_t` type enables programs to handle international characters and multi-byte character sets.

### **max_align_t** (C11)

**Category:** Maximum alignment type
**Purpose:** Type with the strictest alignment requirement
**Declaration:** `typedef struct { long long ll; long double ld; } max_align_t;` (implementation-defined)
**Standard:** C11
**Printf Format:** `%zu` (for alignof result)

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>
#include <stdalign.h>

int main(void) {
    printf("Alignment information:\n");
    printf("Alignment of char: %zu\n", alignof(char));
    printf("Alignment of int: %zu\n", alignof(int));
    printf("Alignment of double: %zu\n", alignof(double));
    printf("Alignment of max_align_t: %zu\n", alignof(max_align_t));
    printf("Maximum fundamental alignment: %zu\n", alignof(max_align_t));
    
    return 0;
}
```

**Expected Output:**

```txt
Alignment information:
Alignment of char: 1
Alignment of int: 4
Alignment of double: 8
Alignment of max_align_t: 16
Maximum fundamental alignment: 16
```

The `max_align_t` type is essential for custom memory allocators and low-level memory management.

### **nullptr_t** (C23)

**Category:** Null pointer type
**Purpose:** Type of nullptr literal introduced in C23
**Declaration:** `typedef typeof(nullptr) nullptr_t;`
**Standard:** C23
**Printf Format:** `%p`

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>

int main(void) {
    printf("Size of nullptr_t: %zu bytes\n", sizeof(nullptr_t));
    
    nullptr_t null_ptr = nullptr;
    int *ptr1 = nullptr;
    void *ptr2 = nullptr;
    
    printf("nullptr_t value: %p\n", (void*)null_ptr);
    printf("int* nullptr: %p\n", (void*)ptr1);
    
    if (ptr1 == nullptr) {
        printf("ptr1 is nullptr\n");
    }
    
    return 0;
}
```

**Expected Output:**

```txt
Size of nullptr_t: 8 bytes
nullptr_t value: (nil)
int* nullptr: (nil)
ptr1 is nullptr
```

The `nullptr_t` type provides type-safe null pointers, eliminating ambiguity issues that can occur with the traditional `NULL` macro.

## Macros Defined in stddef.h

### **NULL**

**Category:** Null pointer constant
**Purpose:** Represents null pointer value
**Declaration:** `#define NULL ((void*)0)` or `#define NULL 0`
**Standard:** C89
**Printf Format:** `%p`

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>
#include <stdlib.h>

int main(void) {
    int *ptr1 = NULL;
    char *ptr2 = NULL;
    
    printf("NULL pointer values:\n");
    printf("int* NULL: %p\n", (void*)ptr1);
    printf("char* NULL: %p\n", (void*)ptr2);
    
    if (ptr1 == NULL) {
        printf("ptr1 is NULL\n");
    }
    
    return 0;
}
```

**Expected Output:**

```txt
NULL pointer values:
int* NULL: (nil)
char* NULL: (nil)
ptr1 is NULL
```

The `NULL` macro is the traditional way to represent null pointers, though it may be defined differently on various platforms.

### **offsetof(type, member)**

**Category:** Structure member offset macro
**Purpose:** Get byte offset of member within a structure
**Declaration:** `#define offsetof(type, member) ((size_t)&(((type*)0)->member))`
**Standard:** C89
**Printf Format:** `%zu` (for result)

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>

struct Employee {
    int id;
    char name[^50];
    double salary;
    char department[^30];
};

int main(void) {
    printf("Employee structure layout:\n");
    printf("id offset: %zu bytes\n", offsetof(struct Employee, id));
    printf("name offset: %zu bytes\n", offsetof(struct Employee, name));
    printf("salary offset: %zu bytes\n", offsetof(struct Employee, salary));
    printf("department offset: %zu bytes\n", offsetof(struct Employee, department));
    printf("Total size: %zu bytes\n", sizeof(struct Employee));
    
    return 0;
}
```

**Expected Output:**

```txt
Employee structure layout:
id offset: 0 bytes
name offset: 4 bytes
salary offset: 56 bytes
department offset: 64 bytes
Total size: 94 bytes
```

The `offsetof` macro is crucial for low-level programming, serialization, and understanding memory layout.

### **unreachable()** (C23)

**Category:** Unreachable code marker
**Purpose:** Indicates unreachable code paths for optimization
**Declaration:** `#define unreachable() __builtin_unreachable()`
**Standard:** C23

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>

enum Color { RED, GREEN, BLUE };

const char* color_name(enum Color color) {
    switch (color) {
        case RED: return "Red";
        case GREEN: return "Green"; 
        case BLUE: return "Blue";
    }
    unreachable();  // All enum values handled
}

int main(void) {
    printf("Color RED: %s\n", color_name(RED));
    printf("Color GREEN: %s\n", color_name(GREEN));
    printf("Color BLUE: %s\n", color_name(BLUE));
    
    return 0;
}
```

**Expected Output:**

```txt
Color RED: Red
Color GREEN: Green
Color BLUE: Blue
```

The `unreachable()` macro helps compilers optimize code by indicating impossible execution paths.

### **`__STDC_VERSION_STDDEF_H__`** (C23)

**Category:** Feature test macro
**Purpose:** Indicates C23 stddef.h compliance
**Declaration:** `#define __STDC_VERSION_STDDEF_H__ 202311L`
**Standard:** C23
**Printf Format:** `%ld`

**Usage Example:**

```c
#include <stdio.h>
#include <stddef.h>

int main(void) {
    printf("C23 stddef.h feature test macros:\n");
    
    #ifdef __STDC_VERSION_STDDEF_H__
    printf("__STDC_VERSION_STDDEF_H__: %ldL\n", __STDC_VERSION_STDDEF_H__);
    printf("This indicates C23 stddef.h support\n");
    #else
    printf("__STDC_VERSION_STDDEF_H__ not defined\n");
    #endif
    
    return 0;
}
```

**Expected Output:**

```txt
C23 stddef.h feature test macros:
__STDC_VERSION_STDDEF_H__: 202311L
This indicates C23 stddef.h support
```

This macro allows code to conditionally use C23-specific features from stddef.h.

## Format Specifiers Summary

When printing values of stddef.h types, use these format specifiers:

- **ptrdiff_t**: `%td` (signed)
- **size_t**: `%zu` (unsigned)
- **wchar_t**: `%lc` (character), `%ls` (string)
- **nullptr_t**: `%p` (as void pointer)
- **offsetof result**: `%zu` (returns size_t)

## Platform Considerations

The exact size and representation of these types are implementation-defined but follow these guidelines:

- On 32-bit systems: `ptrdiff_t` and `size_t` are typically 32-bit
- On 64-bit systems: `ptrdiff_t` and `size_t` are typically 64-bit
- `max_align_t` alignment is usually 8 or 16 bytes depending on the platform
- `wchar_t` size varies (2 bytes on Windows, 4 bytes on most Unix systems)

## Standards Evolution

- **C89/C90**: Introduced `ptrdiff_t`, `size_t`, `wchar_t`, `NULL`, `offsetof`
- **C11**: Added `max_align_t`
- **C23**: Added `nullptr_t`, `unreachable()`, and `__STDC_VERSION_STDDEF_H__`

The stddef.h header continues to evolve with new C standards while maintaining backward compatibility with existing code.
