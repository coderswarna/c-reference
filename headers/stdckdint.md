# **Complete Reference for C23 `<stdckdint.h>` Header**

## **Overview**

The **`<stdckdint.h>`** header was introduced in **C23** to provide type-generic macros for **checked integer arithmetic**. This header addresses the long-standing issue of integer overflow detection in C programming by offering standardized functions that can detect arithmetic overflow conditions.

## **Complete Inventory**

The **`<stdckdint.h>`** header contains **exactly 4 components**:

- **1 feature test macro**
- **3 type-generic macros**
- **No other functions and keywords**

## **Header Information**

- **Standard**: C23 (ISO/IEC 9899:2024)  
- **Header Type**: Type-generic macros only (header-only implementation)
- **Purpose**: Checked integer arithmetic operations with overflow detection
- **Compiler Support**: GCC, Clang (via `__builtin_*_overflow` functions)

## **Feature Test Macro**

### **`__STDC_VERSION_STDCKDINT_H__`**

**Type**: Preprocessor macro
**Value**: `202311L`
**Description**: Feature test macro that indicates the availability and version of the `<stdckdint.h>` header.

**Usage**:

```c
#ifdef __STDC_VERSION_STDCKDINT_H__
    // stdckdint.h is available (C23)
    // Safe to use ckd_add, ckd_sub, ckd_mul
#endif
```

## **Type-Generic Macros**

### **`ckd_add(R, A, B)`**

**Type**: Type-generic macro
**Return Type**: `bool`
**Return Values**:

- `false`: No overflow occurred, result is valid
- `true`: Overflow occurred, result contains wrapped value

**Parameters**:

- `R`: Pointer to result variable (modifiable lvalue of integer type, any integer type except plain char, bool, bit-precise types, enumerated types)
- `A`: First operand (any integer type except plain char, bool, bit-precise types, enumerated types)
- `B`: Second operand (any integer type except plain char, bool, bit-precise types, enumerated types)

**Description**: Performs checked addition `A + B` and stores the result in `*R`. The arithmetic is performed as if using infinite precision, then checked if it fits in the result type

**Implementation**: **`__builtin_add_overflow((A), (B), (R))`**

**Example**:

```c
#include <stdckdint.h>
#include <stdio.h>
#include <limits.h>

int main() {
    int result;
    int a = INT_MAX;  // 2147483647
    int b = 1;
    
    if (ckd_add(&result, a, b)) {
        printf("Addition overflow detected!\n");
        printf("Wrapped result: %d\n", result);  // -2147483648
    } else {
        printf("No overflow: %d\n", result);
    }
    return 0;
}
```

**Output**:

```txt
Addition overflow detected!
Wrapped result: -2147483648
```

### **`ckd_sub(R, A, B)`**

**Type**: Type-generic macro
**Return Type**: `bool`
**Return Values**:

- `false`: No overflow occurred, result is valid
- `true`: Overflow occurred, result contains wrapped value

**Parameters**:

- `R`: Pointer to result variable (modifiable lvalue of integer type)
- `A`: Minuend (any integer type except plain char, bool, bit-precise types, enumerated types)
- `B`: Subtrahend (any integer type except plain char, bool, bit-precise types, enumerated types)

**Description**: Performs checked subtraction `A - B` and stores the result in `*R`. The arithmetic is performed as if using infinite precision, then checked if it fits in the result type.

**Implementation**: `__builtin_sub_overflow((A), (B), (R))`

**Example**:

```c
#include <stdckdint.h>
#include <stdio.h>
#include <limits.h>

int main() {
    int result;
    int a = INT_MIN;  // -2147483648
    int b = 1;
    
    if (ckd_sub(&result, a, b)) {
        printf("Subtraction overflow detected!\n");
        printf("Wrapped result: %d\n", result);  // 2147483647
    } else {
        printf("No overflow: %d\n", result);
    }
    return 0;
}
```

**Output**:

```txt
Subtraction overflow detected!
Wrapped result: 2147483647
```

### **`ckd_mul(R, A, B)`**

**Type**: Type-generic macro
**Return Type**: `bool`
**Return Values**:

- `false`: No overflow occurred, result is valid
- `true`: Overflow occurred, result contains wrapped value

**Parameters**:

- `R`: Pointer to result variable (modifiable lvalue of integer type)
- `A`: First factor (any integer type except plain char, bool, bit-precise types, enumerated types)
- `B`: Second factor (any integer type except plain char, bool, bit-precise types, enumerated types)

**Description**: Performs checked multiplication `A × B` and stores the result in `*R`. The arithmetic is performed as if using infinite precision, then checked if it fits in the result type.

**Implementation**: `__builtin_mul_overflow((A), (B), (R))`

**Example**:

```c
#include <stdckdint.h>
#include <stdio.h>

int main() {
    int result;
    int a = 46341;  // sqrt(INT_MAX) + 1 approximately
    int b = 46341;
    
    if (ckd_mul(&result, a, b)) {
        printf("Multiplication overflow detected!\n");
        printf("Wrapped result: %d\n", result);
    } else {
        printf("No overflow: %d\n", result);
    }
    return 0;
}
```

**Output**:

```txt
Multiplication overflow detected!
Wrapped result: -2147479015
```

## **Implementation Details**

### **Compiler Implementation**

Most compilers implement these macros using built-in overflow detection functions:

```c
#define ckd_add(R, A, B) __builtin_add_overflow((A), (B), (R))
#define ckd_sub(R, A, B) __builtin_sub_overflow((A), (B), (R))
#define ckd_mul(R, A, B) __builtin_mul_overflow((A), (B), (R))
```

### **Compiler Support**

**GCC**: Available from GCC 14 onwards
**Clang**: Available from Clang 18 onwards
**MSVC**: Not yet supported in standard MSVC

### **Type Requirements**

**Allowed Types**:

- `signed char`, `unsigned char`
- `short`, `unsigned short`
- `int`, `unsigned int`
- `long`, `unsigned long`
- `long long`, `unsigned long long`
- Extended integer types

**Disallowed Types**:

- Plain `char`
- `bool` (`_Bool`)
- Bit-precise integer types
- Enumerated types

### **Mixed-Type Operations**

The macros support operations between different integer types:

```c
int result;
long a = 1000000L;
short b = 32000;

if (ckd_mul(&result, a, b)) {
    printf("Overflow when multiplying long and short\n");
}
```

## **Practical Usage Example**

### **Complete Example Program**

```c
#include <stdckdint.h>
#include <stdio.h>
#include <limits.h>

void test_operation(const char* op_name, bool overflow, int result) {
    if (overflow) {
        printf("%s: OVERFLOW DETECTED - wrapped result: %d\n", op_name, result);
    } else {
        printf("%s: SUCCESS - result: %d\n", op_name, result);
    }
}

int main() {
    printf("C23 stdckdint.h Checked Integer Arithmetic Demo\n");
    printf("===============================================\n");
    
    #ifdef __STDC_VERSION_STDCKDINT_H__
    printf("stdckdint.h version: %ld\n\n", __STDC_VERSION_STDCKDINT_H__);
    #else
    printf("stdckdint.h not available\n");
    return 1;
    #endif
    
    int result;
    
    // Test addition overflow
    test_operation("ADD(INT_MAX, 1)", 
                   ckd_add(&result, INT_MAX, 1), result);
    
    // Test subtraction overflow  
    test_operation("SUB(INT_MIN, 1)", 
                   ckd_sub(&result, INT_MIN, 1), result);
    
    // Test multiplication overflow
    test_operation("MUL(46341, 46341)", 
                   ckd_mul(&result, 46341, 46341), result);
    
    // Test successful operations
    test_operation("ADD(100, 200)", 
                   ckd_add(&result, 100, 200), result);
    
    test_operation("SUB(500, 200)", 
                   ckd_sub(&result, 500, 200), result);
    
    test_operation("MUL(123, 456)", 
                   ckd_mul(&result, 123, 456), result);
    
    return 0;
}
```

### **Safe Array Indexing**

```c
#include <stdckdint.h>
#include <stdlib.h>

void* safe_array_access(void* array, size_t element_size, size_t index, size_t count) {
    size_t offset;
    size_t total_elements;
    
    if (ckd_mul(&offset, index, element_size)) {
        return NULL;  // Index calculation overflow
    }
    
    if (ckd_mul(&total_elements, count, element_size)) {
        return NULL;  // Size calculation overflow  
    }
    
    if (offset >= total_elements) {
        return NULL;  // Out of bounds
    }
    
    return (char*)array + offset;
}
```

### **Safe Memory Allocation**

```c
#include <stdckdint.h>
#include <stdlib.h>

void* safe_calloc(size_t count, size_t size) {
    size_t total_size;
    
    if (ckd_mul(&total_size, count, size)) {
        return NULL;  // Size calculation would overflow
    }
    
    return calloc(count, size);
}
```

### **Arithmetic with Error Handling**

```c
#include <stdckdint.h>
#include <stdio.h>

typedef struct {
    int value;
    bool valid;
} safe_int;

safe_int safe_add(int a, int b) {
    safe_int result;
    result.valid = !ckd_add(&result.value, a, b);
    return result;
}

int main() {
    safe_int sum = safe_add(2147483647, 1);
    
    if (sum.valid) {
        printf("Sum: %d\n", sum.value);
    } else {
        printf("Addition would overflow!\n");
    }
    
    return 0;
}
```

## **Comparison with Manual Overflow Checking**

### **Before C23 (Manual Checking)**

```c
#include <limits.h>
#include <stdbool.h>

bool safe_add_manual(int a, int b, int *result) {
    if (a > 0 && b > INT_MAX - a) {
        return false;  // Positive overflow
    }
    if (a < 0 && b < INT_MIN - a) {
        return false;  // Negative overflow  
    }
    *result = a + b;
    return true;
}
```

### **With C23 stdckdint.h**

```c
#include <stdckdint.h>

bool safe_add_c23(int a, int b, int *result) {
    return !ckd_add(result, a, b);
}
```

## **Summary**

The `<stdckdint.h>` header is a **minimal but powerful** addition to C23, containing exactly:

1. **`__STDC_VERSION_STDCKDINT_H__`** - Feature test macro (value: 202311L)
2. **`ckd_add(R, A, B)`** - Checked addition with overflow detection
3. **`ckd_sub(R, A, B)`** - Checked subtraction with overflow detection
4. **`ckd_mul(R, A, B)`** - Checked multiplication with overflow detection

These components provide a standardized, type-generic interface for safe integer arithmetic, addressing decades of integer overflow vulnerabilities in C programs. The header is implemented using compiler built-ins and requires no library functions, making it a header-only solution for checked arithmetic. The header represents a significant improvement in C's ability to handle arithmetic safely and is an essential tool for security-conscious C programming.
