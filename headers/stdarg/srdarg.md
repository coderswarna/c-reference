# Comprehensive Reference for `<stdarg.h>`

This reference covers all types, macros, and functions defined in the C Standard `<stdarg.h>`, including their purpose, signatures, behaviors, and illustrative examples with outputs.

## Overview

The `<stdarg.h>` header provides facilities for accessing a function’s variable arguments (variadic functions). It defines:

- A type:
    - **va_list**
- Macros to manage variable argument lists:
    - **va_start**
    - **va_arg**
    - **va_end**
    - **va_copy** (since C99)

There are no functions or keywords beyond these; all facilities are provided as macros and a typedef.

***

## 1. va_list

**Purpose:**
Holds the state needed by the macros to traverse the variable arguments.

**Definition:**

```c
typedef /* implementation-defined */ va_list;
```

**Details:**
A `va_list` object must be declared in the function that accepts a variable number of arguments.

### Example Declaration

```c
#include <stdarg.h>

void example(const char *fmt, ...) {
    va_list args;
    /* ... */
}
```


***

## 2. va_start

**Purpose:**
Initializes a `va_list` object for argument retrieval.

**Signature:**

```c
#define va_start(ap, last_fixed_arg) /* implementation-defined */
```

- **ap**: `va_list` variable to initialize.
- **last_fixed_arg**: the name of the last named (fixed) parameter of the function.

**Behavior:**
After this macro, `ap` points to the first unnamed argument.

### Example

```c
#include <stdio.h>
#include <stdarg.h>

void print_ints(int count, ...) {
    va_list ap;
    va_start(ap, count);
    for (int i = 0; i < count; ++i) {
        int value = va_arg(ap, int);
        printf("%d ", value);
    }
    va_end(ap);
    printf("\n");
}

int main(void) {
    print_ints(5, 10, 20, 30, 40, 50);
    return 0;
}
```

**Output:**

```
10 20 30 40 50 
```


***

## 3. va_arg

**Purpose:**
Retrieves the next argument in the list.

**Signature:**

```c
#define va_arg(ap, type) /* expands to an expression of type */
```

- **ap**: the `va_list` object.
- **type**: the type to retrieve; must be a POD type or compatible object type.

**Return Value:**
The argument of the specified type. The evaluation of this macro modifies the state of `ap` so subsequent calls access subsequent arguments.

### Example

(See `print_ints` above: `va_arg(ap, int)` returns each successive `int`.)

***

## 4. va_end

**Purpose:**
Cleans up the `va_list` object when done.

**Signature:**

```c
#define va_end(ap) /* implementation-defined */
```

- **ap**: the `va_list` object initialized by `va_start` or `va_copy`.

**Behavior:**
Performs any necessary cleanup. After calling this macro, the `va_list` object is invalid until reinitialized.

### Example

(See `print_ints` above: `va_end(ap);`)

***

## 5. va_copy

**Availability:**
Added in C99 (not part of ANSI C89).

**Purpose:**
Creates a copy of an existing `va_list`.

**Signature:**

```c
#define va_copy(dest, src) /* implementation-defined */
```

- **dest**: `va_list` object to initialize as a copy of `src`.
- **src**: existing `va_list`.

**Behavior:**
Both `dest` and `src` can be used independently. Each must be ended with `va_end`.

### Example

```c
#include <stdio.h>
#include <stdarg.h>

void print_twice(int count, ...) {
    va_list ap1, ap2;
    va_start(ap1, count);
    va_copy(ap2, ap1);

    printf("First pass: ");
    for (int i = 0; i < count; ++i)
        printf("%d ", va_arg(ap1, int));
    printf("\n");

    printf("Second pass: ");
    for (int i = 0; i < count; ++i)
        printf("%d ", va_arg(ap2, int));
    printf("\n");

    va_end(ap1);
    va_end(ap2);
}

int main(void) {
    print_twice(4, 7, 14, 21, 28);
    return 0;
}
```

**Output:**

```
First pass: 7 14 21 28 
Second pass: 7 14 21 28 
```


***

## Summary of `stdarg.h` Facilities

| **Name** | **Type** | **Returns / Value** | **Use Case** |
| :-- | :-- | :-- | :-- |
| va_list | typedef | — (object type) | Holds state for variadic argument access. |
| va_start | macro | void | Initialize `va_list`. |
| va_arg | macro | specified `type` | Retrieve next argument of given type. |
| va_end | macro | void | Clean up `va_list`. |
| va_copy | macro (C99) | void | Duplicate a `va_list` for independent use. |

All macros incur no standard “return” beyond their specified behaviors. Use `va_start` → zero or more `va_arg` → `va_end` in sequence for safe and portable iteration over variable arguments. Continuous iteration without `va_end` or misuse of types with `va_arg` leads to undefined behavior. Continuous duplication with `va_copy` must be balanced with corresponding `va_end` calls.

