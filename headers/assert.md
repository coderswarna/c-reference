# Complete Reference Guide to assert.h in C

## Overview

The **assert.h** header file is a critical component of the C standard library that provides debugging and program verification capabilities. It defines macros and keywords for both runtime and compile-time assertions, helping developers catch errors early in the development process.

## Primary Functions and Macros

### Runtime Assertion: `assert()`

The **assert()** macro is the cornerstone of the assert.h header, providing runtime assertion checking.

**Syntax:** `assert(expression)`  
**Return Type:** void  
**Standard:** C89/C90 and later

**Behavior:**

- **If expression is true (non-zero):** Program continues execution without interruption
- **If expression is false (zero):** Prints diagnostic message to stderr and calls abort()

**Diagnostic Message Format:**

- C89/C90: `Assertion failed: expression, file filename, line line-number`
- C99+: `filename:line: function: Assertion 'expression' failed.`

The diagnostic message automatically includes:

- Source filename using `__FILE__` macro
- Line number using `__LINE__` macro  
- Function name using `__func__` identifier (C99+)
- The actual expression that failed

### Compile-Time Assertion: `static_assert()`

**Syntax:**

- C11: `static_assert(constant-expression, message)`
- C23: `static_assert(constant-expression)` (message optional)

**Return Type:** void  
**Standard:** C11 and later

The `static_assert()` macro performs compile-time verification, causing compilation to fail if the constant expression evaluates to false. This is particularly useful for validating assumptions about data sizes, structure layouts, and other compile-time constants.

### Compile-Time Assertion Keyword: `_Static_assert`

**Syntax:**

- C11: `_Static_assert(constant-expression, message)`
- C23: `_Static_assert(constant-expression)` (message optional)

**Return Type:** void  
**Standard:** C11 and later

The `_Static_assert` keyword provides the same functionality as `static_assert()` but can be used without including assert.h. In C11, `static_assert` is actually a macro that maps to `_Static_assert`.

## Control Macro: `NDEBUG`

**Purpose:** Disables all `assert()` macros when defined

**Effect:** Transforms `assert(expression)` into `((void)0)` - a no-operation

**Definition Methods:**

- `#define NDEBUG` before including assert.h
- Compiler flag: `gcc -DNDEBUG`
- Compiler flag: `cl /DNDEBUG` (Microsoft)

When `NDEBUG` is defined, all assert statements are completely removed from the compiled code, improving performance in release builds.

## Predefined Macros Used by assert.h

### `__FILE__`

- **Type:** Standard predefined macro
- **Returns:** String literal containing the current source filename
- **Usage:** Included in assert diagnostic messages

### `__LINE__`  

- **Type:** Standard predefined macro
- **Returns:** Integer representing the current line number
- **Usage:** Included in assert diagnostic messages

### `__func__`

- **Type:** Standard predefined identifier (C99+)
- **Returns:** String containing the current function name
- **Usage:** Included in assert diagnostic messages (C99 and later)

## Related Functions

### `abort()`

- **Header:** stdlib.h
- **Purpose:** Terminates program abnormally
- **Called by:** assert() macro when assertion fails
- **Return Type:** void (never returns)

## Evolution Across C Standards

### C89/C90

- Introduced `assert` macro
- Support for `NDEBUG` macro
- Diagnostic includes filename and line number

### C99

- Added `__func__` to diagnostic messages
- Enhanced diagnostic format

### C11

- Introduced `static_assert` macro
- Added `_Static_assert` keyword
- Compile-time assertion support

### C23

- Made message parameter optional for static assertions
- `static_assert` became a keyword

## Complete Reference Table

| Item | Type | Syntax | Purpose | Return Type | Standard | Notes |
|------|------|--------|---------|-------------|----------|-------|
| **`assert`** | Runtime Macro | **`assert(expression)`** | Tests condition at runtime, aborts if false | void | C89/C90 | Can be disabled with NDEBUG |
| **`static_assert`** | Compile-time Macro | **`static_assert(expr, msg)`** | Tests condition at compile-time | void | C11+ | Message optional in C23 |
| **`_Static_assert`** | Compile-time Keyword | **`_Static_assert(expr, msg)`** | Tests condition at compile-time | void | C11+ | No header required |
| **`NDEBUG`** | Control Macro | **`#define NDEBUG`** | Disables assert() macros | N/A | C89/C90 | Must be defined before #include |
| **`__FILE__`** | Predefined Macro | **`__FILE__`** | Current source filename | const char* | C89/C90 | Used in assert diagnostics |
| **`__LINE__`** | Predefined Macro | **`__LINE__`** | Current line number | int | C89/C90 | Used in assert diagnostics |
| **`__func__`** | Predefined Identifier | **`__func__`** | Current function name | const char* | C99+ | Used in assert diagnostics |
| **`abort`** | Library Function | **`abort(void)`** | Terminates program abnormally | void (never returns) | C89/C90 | Called by assert() on failure |

## Practical Examples

### Basic Assert Example

```c
#include <stdio.h>
#include <assert.h>

int main() {
    int x = 5;
    
    // This assertion passes
    assert(x == 5);
    printf("First assertion passed\n");
    
    // This assertion will fail and abort the program
    assert(x == 10);
    printf("This line will never be reached\n");
    
    return 0;
}

/* Expected Output:
First assertion passed
Assertion failed: x == 10, file example.c, line 11
Aborted (core dumped)
*/
```

### Function Parameter Validation

```c
#include <stdio.h>
#include <assert.h>
#include <string.h>

void string_copy(char* dest, const char* src, size_t dest_size) {
    // Validate function parameters
    assert(dest != NULL);        // dest must not be NULL
    assert(src != NULL);         // src must not be NULL  
    assert(dest_size > 0);       // dest_size must be positive
    assert(strlen(src) < dest_size); // src must fit in dest
    
    strcpy(dest, src);
}

int main() {
    char buffer[100];
    
    // Valid call
    string_copy(buffer, "Hello", sizeof(buffer));
    printf("Result: %s\n", buffer);
    
    // This will trigger assertion failure
    string_copy(NULL, "Hello", sizeof(buffer));
    
    return 0;
}

/* Expected Output:
Result: Hello
Assertion failed: dest != NULL, file example.c, line 7
Aborted (core dumped)
*/
```

### Array Bounds Checking

```c
#include <stdio.h>
#include <assert.h>

#define ARRAY_SIZE 10

void array_access(int arr[], int index, int size) {
    // Check array bounds
    assert(arr != NULL);           // Array must not be NULL
    assert(index >= 0);            // Index must be non-negative
    assert(index < size);          // Index must be within bounds
    assert(size <= ARRAY_SIZE);    // Size must not exceed maximum
    
    printf("arr[%d] = %d\n", index, arr[index]);
}

int main() {
    int numbers[ARRAY_SIZE] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // Valid access
    array_access(numbers, 5, ARRAY_SIZE);
    
    // This will trigger assertion failure (out of bounds)
    array_access(numbers, 15, ARRAY_SIZE);
    
    return 0;
}

/* Expected Output:
arr[5] = 6
Assertion failed: index < size, file example.c, line 9
Aborted (core dumped)
*/
```

### Static Assert Example

```c
#include <stdio.h>
#include <assert.h>
#include <stddef.h>

struct Point {
    int x;
    int y;
};

struct Rectangle {
    struct Point top_left;
    struct Point bottom_right;
};

// Compile-time assertions
static_assert(sizeof(int) >= 4, "int must be at least 4 bytes");
static_assert(sizeof(struct Point) == 8, "Point struct must be 8 bytes");
static_assert(offsetof(struct Rectangle, bottom_right) == 8, "bottom_right must be at offset 8");

// C23 syntax (message optional)
#if __STDC_VERSION__ >= 202311L
static_assert(sizeof(void*) >= sizeof(int));
#endif

int main() {
    printf("All compile-time assertions passed!\n");
    printf("sizeof(int): %zu\n", sizeof(int));
    printf("sizeof(struct Point): %zu\n", sizeof(struct Point));
    printf("offsetof bottom_right: %zu\n", offsetof(struct Rectangle, bottom_right));
    
    return 0;
}

/* Expected Output (if all assertions pass):
All compile-time assertions passed!
sizeof(int): 4
sizeof(struct Point): 8
offsetof bottom_right: 8
*/
```

### NDEBUG Example

```c
// Compile with: gcc -DNDEBUG example.c
// Or define NDEBUG before including assert.h

#define NDEBUG  // Disable assertions
#include <stdio.h>
#include <assert.h>

int divide(int a, int b) {
    // This assertion will be completely removed when NDEBUG is defined
    assert(b != 0);  // With NDEBUG: becomes ((void)0)
    
    return a / b;
}

int main() {
    printf("10 / 2 = %d\n", divide(10, 2));
    
    // This would normally cause assertion failure, but with NDEBUG it's ignored
    printf("10 / 0 = %d\n", divide(10, 0));  // Undefined behavior!
    
    return 0;
}

/* Expected Output with NDEBUG defined:
10 / 2 = 5
10 / 0 = [undefined behavior - may crash or return garbage]

Without NDEBUG:
10 / 2 = 5
Assertion failed: b != 0, file example.c, line 8
Aborted (core dumped)
*/
```

### Advanced Assert with Message

```c
#include <stdio.h>
#include <assert.h>

int main() {
    int balance = 100;
    int withdrawal = 150;
    
    // Using comma operator to include custom message
    assert(("Insufficient funds", balance >= withdrawal));
    
    printf("Withdrawal successful\n");
    
    return 0;
}

/* Expected Output:
Assertion failed: ("Insufficient funds", balance >= withdrawal), file example.c, line 9
Aborted (core dumped)

The custom message "Insufficient funds" appears in the assertion expression.
*/
```

### _Static_assert Keyword Example

```c
// Using _Static_assert keyword directly (C11+)
// No need to include assert.h for _Static_assert

#include <stdio.h>

// These work without including assert.h
_Static_assert(sizeof(char) == 1, "char must be 1 byte");
_Static_assert(sizeof(int) >= 2, "int must be at least 2 bytes");

// C23 allows omitting the message
#if __STDC_VERSION__ >= 202311L
_Static_assert(sizeof(long) >= sizeof(int));
#endif

int main() {
    printf("All static assertions passed!\n");
    return 0;
}

/* Expected Output:
All static assertions passed!
*/
```

## Best Practices and Usage Guidelines

### When to Use assert()

1. **Parameter validation** - Check function preconditions
2. **Array bounds checking** - Prevent buffer overflows
3. **Null pointer validation** - Catch NULL pointer dereferences
4. **State validation** - Ensure objects are in valid states
5. **Post-condition verification** - Validate function results

### When to Use static_assert()

1. **Data type size validation** - Ensure expected sizes
2. **Structure layout verification** - Check member offsets
3. **Constant value validation** - Verify compile-time constants
4. **Platform-specific assumptions** - Validate architecture requirements

### Important Warnings

- **Never use side effects in assert expressions** - They won't execute when `NDEBUG` is defined
- **Assert is for debugging, not error handling** - Don't use for user input validation
- **Assertions should check programmer assumptions, not user errors**

## Compilation Examples

### Standard Compilation

```bash
gcc -o program program.c
# Assertions are enabled
```

### Release Build (Assertions Disabled)

```bash
gcc -DNDEBUG -O2 -o program program.c
# All assert() macros are disabled
```

### Debug Build with Extra Information

```bash
gcc -g -Wall -Wextra -o program program.c
# Assertions enabled with debug symbols
```

## Summary

The assert.h header provides a comprehensive set of tools for both runtime and compile-time program verification:

- **Runtime assertions** with `assert()` for dynamic condition checking
- **Compile-time assertions** with `static_assert()` and `_Static_assert` for static validation
- **Conditional compilation** support via `NDEBUG` for release builds
- **Automatic diagnostic information** including file, line, and function details
- **Evolution across C standards** from C89 to modern C23

This makes assert.h an essential component for developing robust and reliable C programs, providing developers with powerful debugging and verification capabilities that help catch errors early in the development process.
