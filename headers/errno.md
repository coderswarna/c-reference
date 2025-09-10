# C errno.h Header - Comprehensive Reference Guide

The `errno.h` header file in the C Standard Library is essential for error handling and reporting. It defines the integer variable `errno` and various macros for identifying different error conditions that can occur during program execution.

## **Table of Contents**

- [Core Components](#core-components)
  - [`errno` Variable](#errno-variable)
- [Standard Error Macros](#standard-error-macros)
  - [EDOM - Domain Error](#edom---domain-error)
  - [ERANGE - Range Error](#erange---range-error)
  - [EILSEQ - Illegal Byte Sequence](#eilseq---illegal-byte-sequence)
- [C23 Features](#c23-features)
  - [`__STDC_VERSION_ERRNO_H__` Macro](#__stdc_version_errno_h__-macro)
- [Related Function](#related-functions)
  - [`perror()` Funtion](#perror-function)
  - [`strerror()` Funtion](#strerror-function)
- [Complete Error Handling Example](#complete-error-handling-example)
- [Thread Safety](#thread-safety)
- [Best Practices](#best-practices)
- [Summary](#summary)

## **Core Components**

### **errno Variable**

**Type**: `extern int errno`  
**Description**: Global thread-local variable that stores error codes set by system calls and library functions when errors occur  
**Initial Value**: 0 at program startup  
**Behavior**: Set to positive values by library functions on error; never reset to zero by any library function  

```c
#include <stdio.h>
#include <errno.h>
#include <string.h>

int main() {
    FILE *file;
    
    // Reset errno before function call
    errno = 0;
    
    // Attempt to open non-existent file
    file = fopen("nonexistent.txt", "r");
    
    if (file == NULL) {
        printf("Error number: %d\n", errno);
        printf("Error message: %s\n", strerror(errno));
    } else {
        fclose(file);
        printf("File opened successfully\n");
    }
    
    return 0;
}
```

**Expected Output**:

```txt
Error number: 2
Error message: No such file or directory
```

## **Standard Error Macros**

### **EDOM - Domain Error**

**Type**: Macro expanding to integer constant expression  
**Value**: Typically 33  
**Description**: Represents a domain error, occurs when an input argument is outside the domain over which a mathematical function is defined  
**Usage**: Set by mathematical functions like `sqrt()`, `log()`, `acos()` when given invalid arguments  

```c
#include <stdio.h>
#include <errno.h>
#include <math.h>
#include <string.h>

int main() {
    double result;
    
    // Reset errno
    errno = 0;
    
    // This will cause a domain error
    result = sqrt(-1.0);
    
    if (errno == EDOM) {
        printf("Domain error occurred!\n");
        printf("Error: %s\n", strerror(errno));
        printf("EDOM value: %d\n", EDOM);
    }
    
    // Reset errno for next test
    errno = 0;
    
    // Valid operation
    result = sqrt(4.0);
    printf("sqrt(4.0) = %.2f\n", result);
    
    if (errno == 0) {
        printf("No error for valid operation\n");
    }
    
    return 0;
}
```

**Expected Output**:

```c
Domain error occurred!
Error: Numerical argument out of domain
EDOM value: 33
sqrt(4.0) = 2.00
No error for valid operation
```

### **ERANGE - Range Error**

**Type**: Macro expanding to integer constant expression  
**Value**: Typically 34  
**Description**: Represents a range error, occurs when a result is outside the range that can be represented  
**Usage**: Set by mathematical functions when results are too large or too small, or by conversion functions like `strtol()` on overflow  

```c
#include <stdio.h>
#include <errno.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>

int main() {
    char *endptr;
    long result;
    
    // Test ERANGE with strtol
    errno = 0;
    
    // This will cause a range error (number too large for long)
    result = strtol("999999999999999999999999999999", &endptr, 10);
    
    if (errno == ERANGE) {
        printf("Range error occurred!\n");
        printf("Error: %s\n", strerror(errno));
        printf("ERANGE value: %d\n", ERANGE);
        printf("Result clamped to: %ld\n", result);
    }
    
    // Reset for valid operation
    errno = 0;
    result = strtol("123", &endptr, 10);
    
    if (errno == 0) {
        printf("Valid conversion: %ld\n", result);
    }
    
    return 0;
}
```

**Expected Output**:

```c
Range error occurred!
Error: Numerical result out of range
ERANGE value: 34
Result clamped to: 9223372036854775807
Valid conversion: 123
```

### **EILSEQ - Illegal Byte Sequence**

**Type**: Macro expanding to integer constant expression  
**Value**: Typically 84  
**Description**: Represents an illegal byte sequence error in multibyte character operations  
**Added**: C95 Amendment 1  
**Usage**: Set by multibyte character conversion functions like `mbrtowc()`, `wcrtomb()` when encountering invalid sequences  

```c
#include <stdio.h>
#include <errno.h>
#include <wchar.h>
#include <string.h>
#include <locale.h>

int main() {
    // Set locale for multibyte operations
    setlocale(LC_ALL, "C");
    
    wchar_t wc;
    mbstate_t state = {0};
    
    // Invalid UTF-8 sequence
    char invalid_seq[] = "\xFF\xFE";
    
    errno = 0;
    
    size_t result = mbrtowc(&wc, invalid_seq, sizeof(invalid_seq), &state);
    
    if (errno == EILSEQ) {
        printf("Illegal byte sequence error!\n");
        printf("Error: %s\n", strerror(errno));
        printf("EILSEQ value: %d\n", EILSEQ);
    }
    
    return 0;
}
```

**Expected Output**:

```txt
Illegal byte sequence error!
Error: Invalid or incomplete multibyte or wide character
EILSEQ value: 84
```

## **C23 Features**

### **`__STDC_VERSION_ERRNO_H__ Macro`**

**Type**: Feature test macro  
**Value**: `202311L` in C23 compliant implementations  
**Description**: Indicates C23 compliance level of the errno.h header  
**Usage**: Preprocessor conditional compilation for C23-specific features  

```c
#include <stdio.h>
#include <errno.h>

int main() {
    printf("=== C23 errno.h Feature Test ===\n");
    
#ifdef __STDC_VERSION_ERRNO_H__
    printf("C23 errno.h detected\n");
    printf("__STDC_VERSION_ERRNO_H__ = %ld\n", __STDC_VERSION_ERRNO_H__);
#else
    printf("Pre-C23 errno.h implementation\n");
#endif

    // Display standard errno constants
    printf("\nStandard errno constants:\n");
    printf("EDOM = %d\n", EDOM);
    printf("ERANGE = %d\n", ERANGE);
    
#ifdef EILSEQ
    printf("EILSEQ = %d\n", EILSEQ);
#else
    printf("EILSEQ not defined (pre-C95)\n");
#endif

    return 0;
}
```

**Expected Output (C23)**:

```c
=== C23 errno.h Feature Test ===
C23 errno.h detected
__STDC_VERSION_ERRNO_H__ = 202311

Standard errno constants:
EDOM = 33
ERANGE = 34
EILSEQ = 84
```

## **Related Functions**

While not part of `errno.h` itself, these functions are commonly used with errno:

### **perror() Function**

**Header**: `<stdio.h>`  
**Prototype**: `void perror(const char *s)`  
**Description**: Prints descriptive error message to stderr  
**Usage**: Displays custom prefix followed by colon and errno description  

```c
#include <stdio.h>
#include <errno.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd;
    
    // Attempt to open file with specific flags
    fd = open("restricted_file.txt", O_RDWR | O_CREAT, 0000);
    
    if (fd == -1) {
        // perror prints to stderr with custom prefix
        perror("File operation failed");
        
        // Also show the errno value
        fprintf(stderr, "errno value: %d\n", errno);
        
        return 1;
    }
    
    printf("File opened successfully\n");
    close(fd);
    return 0;
}
```

**Expected Output (to stderr)**:

```c
File operation failed: Permission denied
errno value: 13
```

### **strerror() Function**

**Header**: `<string.h>`  
**Prototype**: `char *strerror(int errnum)`  
**Description**: Returns pointer to error message string for given error number  
**Usage**: Converts errno values to human-readable strings  

## **Complete Error Handling Example**

```c
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <string.h>
#include <math.h>

// Function to demonstrate proper error handling
int safe_divide(double a, double b, double *result) {
    if (b == 0.0) {
        errno = EDOM;  // Set domain error
        return -1;
    }
    
    *result = a / b;
    return 0;
}

// Function to demonstrate math error handling
int safe_sqrt(double x, double *result) {
    errno = 0;  // Clear errno
    
    *result = sqrt(x);
    
    if (errno == EDOM) {
        return -1;  // Domain error occurred
    }
    
    return 0;
}

int main() {
    double result;
    
    printf("=== Error Handling Demonstration ===\n\n");
    
    // Test 1: File operation error
    printf("1. File Operation Test:\n");
    FILE *fp = fopen("/root/secret.txt", "r");
    if (fp == NULL) {
        printf("   fopen failed - errno: %d\n", errno);
        printf("   Error message: %s\n", strerror(errno));
        perror("   perror output");
    }
    printf("\n");
    
    // Test 2: Domain error with sqrt
    printf("2. Math Domain Error Test:\n");
    if (safe_sqrt(-4.0, &result) == -1) {
        printf("   sqrt(-4.0) failed - errno: %d\n", errno);
        printf("   Error message: %s\n", strerror(errno));
        printf("   EDOM constant: %d\n", EDOM);
    }
    printf("\n");
    
    // Test 3: Custom function error
    printf("3. Custom Function Error Test:\n");
    if (safe_divide(10.0, 0.0, &result) == -1) {
        printf("   Division by zero - errno: %d\n", errno);
        printf("   Error message: %s\n", strerror(errno));
    }
    printf("\n");
    
    // Test 4: Range error with strtol
    printf("4. Range Error Test:\n");
    char *endptr;
    errno = 0;
    long val = strtol("99999999999999999999999999", &endptr, 10);
    if (errno == ERANGE) {
        printf("   strtol overflow - errno: %d\n", errno);
        printf("   Error message: %s\n", strerror(errno));
        printf("   ERANGE constant: %d\n", ERANGE);
        printf("   Clamped value: %ld\n", val);
    }
    
    return 0;
}
```

**Expected Output**:

```c
=== Error Handling Demonstration ===

1. File Operation Test:
   fopen failed - errno: 2
   Error message: No such file or directory
   perror output: No such file or directory

2. Math Domain Error Test:
   sqrt(-4.0) failed - errno: 33
   Error message: Numerical argument out of domain
   EDOM constant: 33

3. Custom Function Error Test:
   Division by zero - errno: 33
   Error message: Numerical argument out of domain

4. Range Error Test:
   strtol overflow - errno: 34
   Error message: Numerical result out of range
   ERANGE constant: 34
   Clamped value: 9223372036854775807
```

## **Thread Safety**

In modern C implementations, `errno` is thread-local, meaning each thread has its own copy of the errno variable. This ensures that error conditions in one thread don't interfere with error handling in other threads.

```c
#include <stdio.h>
#include <errno.h>
#include <pthread.h>
#include <unistd.h>
#include <string.h>

void* thread_function(void* arg) {
    int thread_num = *(int*)arg;
    
    // Each thread has its own errno
    FILE *fp = fopen("nonexistent.txt", "r");
    if (fp == NULL) {
        printf("Thread %d: errno = %d (%s)\n", 
               thread_num, errno, strerror(errno));
    }
    
    return NULL;
}

int main() {
    pthread_t threads[3];
    int thread_nums[] = {1, 2, 3};
    
    printf("=== Thread Safety Test ===\n");
    
    // Create multiple threads
    for (int i = 0; i < 3; i++) {
        pthread_create(&threads[i], NULL, thread_function, &thread_nums[i]);
    }
    
    // Wait for all threads
    for (int i = 0; i < 3; i++) {
        pthread_join(threads[i], NULL);
    }
    
    printf("All threads completed - errno in main: %d\n", errno);
    
    return 0;
}
```

**Expected Output**:

```c
=== Thread Safety Test ===
Thread 1: errno = 2 (No such file or directory)
Thread 2: errno = 2 (No such file or directory)
Thread 3: errno = 2 (No such file or directory)
All threads completed - errno in main: 0
```

## **Best Practices**

1. **Always reset errno** before calling functions that may set it, as successful function calls don't clear errno.

2. **Check errno immediately** after function calls that are documented to set it.

3. **Use appropriate error handling functions** like `perror()` for quick debugging and `strerror()` for custom error messages.

4. **Be aware of thread safety** - errno is thread-local in modern implementations.

5. **Test error conditions** systematically in your programs to ensure robust error handling.

## **Summary**

| Component | Type | Value | Description |
|-----------|------|-------|-------------|
| `errno` | extern int variable | 0 initially | Thread-local error code storage |
| `EDOM` | Macro constant | Typically 33 | Domain error for math functions |
| `ERANGE` | Macro constant | Typically 34 | Range error for overflow/underflow |
| `EILSEQ` | Macro constant | Typically 84 | Illegal byte sequence (C95+) |
| `__STDC_VERSION_ERRNO_H__` | Feature test macro | 202311L | C23 compliance indicator |

The `errno.h` header provides the foundation for systematic error handling in C programming, enabling developers to create robust applications that can detect, report, and recover from various error conditions.
