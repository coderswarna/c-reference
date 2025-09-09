# Complete Reference: C stdlib.h Library

## Overview

The `<stdlib.h>` header is one of the most important standard library headers in C programming. It provides general-purpose utilities including memory management, string conversions, random number generation, process control, and mathematical functions.

---

## Macros and Constants

### NULL

**Value:** `((void *)0)`, `0`, or `0L`  
**Description:** Null pointer constant representing a pointer that doesn't point to any valid memory location  
**Usage:** Used to initialize pointers and check for invalid pointer values  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr = NULL;
    if (ptr == NULL) {
        printf("Pointer is null\n");
    }
    return 0;
}
```

**Output:** `Pointer is null`

### EXIT_SUCCESS

**Value:** `0`  
**Description:** Successful program termination status  
**Usage:** Used with `exit()` function and `main()` return value to indicate successful completion  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("Program completed successfully\n");
    exit(EXIT_SUCCESS);
    return 0;  // This line won't be reached
}
```

**Output:** `Program completed successfully`

### EXIT_FAILURE

**Value:** Non-zero (typically `1` or `8`)  
**Description:** Unsuccessful program termination status  
**Usage:** Used with `exit()` function and `main()` return value to indicate error conditions  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr = malloc(sizeof(int));
    if (ptr == NULL) {
        fprintf(stderr, "Memory allocation failed\n");
        exit(EXIT_FAILURE);
    }
    free(ptr);
    return EXIT_SUCCESS;
}
```

### RAND_MAX

**Value:** At least `32767`  
**Description:** Maximum value returned by `rand()` function  
**Usage:** Upper bound for random number generation, used for scaling random numbers  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("RAND_MAX value: %d\n", RAND_MAX);
    int random_num = rand();
    printf("Random number: %d\n", random_num);
    return 0;
}
```

**Output:** `RAND_MAX value: 32767` (or higher)  
`Random number: 15023` (example)

### MB_CUR_MAX

**Value:** Runtime expression ≥ 1  
**Description:** Maximum number of bytes in a multibyte character for the current locale  
**Usage:** Locale-dependent maximum size of multibyte characters  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("MB_CUR_MAX: %d\n", (int)MB_CUR_MAX);
    return 0;
}
```

**Output:** `MB_CUR_MAX: 1` (in C locale)

---

## Data Types

### size_t

**Definition:** Unsigned integer type  
**Description:** Type returned by `sizeof` operator, represents sizes and counts  
**Usage:** Used for array indices, loop counters, and memory sizes  

### wchar_t

**Definition:** Wide character type  
**Description:** Type capable of storing wide character constants  
**Usage:** Stores Unicode and wide characters  

### div_t

**Definition:** `struct { int quot, rem; }`  
**Description:** Structure returned by `div()` function  
**Usage:** Contains quotient and remainder of integer division  

### ldiv_t

**Definition:** `struct { long quot, rem; }`  
**Description:** Structure returned by `ldiv()` function  
**Usage:** Contains quotient and remainder of long integer division  

### lldiv_t (C99)

**Definition:** `struct { long long quot, rem; }`  
**Description:** Structure returned by `lldiv()` function  
**Usage:** Contains quotient and remainder of long long integer division  

---

## String Conversion Functions

- Basic converters

### atoi()

**Signature:** `int atoi(const char *str)`  
**Return Type:** `int`  
**Description:** Converts string to integer  
**Parameters:** `str` - null-terminated string  
**Return Value:** Converted integer value, 0 if conversion fails  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    const char *str1 = "12345";
    const char *str2 = "-678";
    const char *str3 = "123abc";
    
    printf("atoi(\"%s\") = %d\n", str1, atoi(str1));
    printf("atoi(\"%s\") = %d\n", str2, atoi(str2));
    printf("atoi(\"%s\") = %d\n", str3, atoi(str3));
    return 0;
}
```

**Output:**

```c
atoi("12345") = 12345
atoi("-678") = -678
atoi("123abc") = 123
```

### atol()

**Signature:** `long atol(const char *str)`  
**Return Type:** `long`  
**Description:** Converts string to long integer  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    const char *str = "9876543210";
    long result = atol(str);
    printf("atol(\"%s\") = %ld\n", str, result);
    return 0;
}
```

**Output:** `atol("9876543210") = 9876543210`

### atoll() (C99)

**Signature:** `long long atoll(const char *str)`  
**Return Type:** `long long`  
**Description:** Converts string to long long integer  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    const char *str = "123456789012345";
    long long result = atoll(str);
    printf("atoll(\"%s\") = %lld\n", str, result);
    return 0;
}
```

**Output:** `atoll("123456789012345") = 123456789012345`

### atof()

**Signature:** `double atof(const char *str)`  
**Return Type:** `double`  
**Description:** Converts string to floating-point number  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    const char *str1 = "3.14159";
    const char *str2 = "-2.718";
    printf("atof(\"%s\") = %.5f\n", str1, atof(str1));
    printf("atof(\"%s\") = %.3f\n", str2, atof(str2));
    return 0;
}
```

**Output:**

```c
atof("3.14159") = 3.14159
atof("-2.718") = -2.718
```

- Advanced converters with error handling

### strtol()

**Signature:** `long strtol(const char *str, char **endptr, int base)`  
**Return Type:** `long`  
**Description:** Converts string to long with error checking and base specification  
**Parameters:**

- `str` - string to convert
- `endptr` - pointer to end of conversion (can be NULL)
- `base` - number base (2-36 or 0 for auto-detection)

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char *endptr;
    const char *str1 = "12345abc";
    const char *str2 = "1010";
    const char *str3 = "ff";
    
    long val1 = strtol(str1, &endptr, 10);
    printf("strtol(\"%s\", base 10) = %ld, stopped at: %s\n", str1, val1, endptr);
    
    long val2 = strtol(str2, &endptr, 2);
    printf("strtol(\"%s\", base 2) = %ld, stopped at: %s\n", str2, val2, endptr);
    
    long val3 = strtol(str3, &endptr, 16);
    printf("strtol(\"%s\", base 16) = %ld, stopped at: %s\n", str3, val3, endptr);
    
    return 0;
}
```

**Output:**

```c
strtol("12345abc", base 10) = 12345, stopped at: abc
strtol("1010", base 2) = 10, stopped at: 
strtol("ff", base 16) = 255, stopped at: 
```

### strtoll() (C99)

**Signature:** `long long strtoll(const char *str, char **endptr, int base)`  
**Return Type:** `long long`  
**Description:** Converts string to long long with error checking  

### strtoul()

**Signature:** `unsigned long strtoul(const char *str, char **endptr, int base)`  
**Return Type:** `unsigned long`  
**Description:** Converts string to unsigned long with error checking  

### strtoull() (C99)

**Signature:** `unsigned long long strtoull(const char *str, char **endptr, int base)`  
**Return Type:** `unsigned long long`  
**Description:** Converts string to unsigned long long with error checking  

### strtod()

**Signature:** `double strtod(const char *str, char **endptr)`  
**Return Type:** `double`  
**Description:** Converts string to double with error checking  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char *endptr;
    const char *str = "3.14159abc";
    double val = strtod(str, &endptr);
    printf("strtod(\"%s\") = %.5f, stopped at: %s\n", str, val, endptr);
    return 0;
}
```

**Output:** `strtod("3.14159abc") = 3.14159, stopped at: abc`

### strtof() (C99)

**Signature:** `float strtof(const char *str, char **endptr)`  
**Return Type:** `float`  
**Description:** Converts string to float with error checking  

### strtold() (C99)

**Signature:** `long double strtold(const char *str, char **endptr)`  
**Return Type:** `long double`  
**Description:** Converts string to long double with error checking  

---

## Memory Management Functions

### malloc()

**Signature:** `void *malloc(size_t size)`  
**Return Type:** `void *`  
**Description:** Allocates dynamic memory  
**Parameters:** `size` - number of bytes to allocate  
**Return Value:** Pointer to allocated memory, NULL on failure  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr = malloc(5 * sizeof(int));
    if (arr == NULL) {
        printf("Memory allocation failed\n");
        return 1;
    }
    
    for (int i = 0; i < 5; i++) {
        arr[i] = i * i;
        printf("arr[%d] = %d\n", i, arr[i]);
    }
    
    free(arr);
    return 0;
}
```

**Output:**

```c
arr[0] = 0
arr[1] = 1
arr[2] = 4
arr[3] = 9
arr[4] = 16
```

### calloc()

**Signature:** `void *calloc(size_t nmemb, size_t size)`  
**Return Type:** `void *`  
**Description:** Allocates and zero-initializes memory  
**Parameters:** `nmemb` - number of elements, `size` - size of each element  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr = calloc(5, sizeof(int));
    if (arr != NULL) {
        printf("Calloc initialized array:\n");
        for (int i = 0; i < 5; i++) {
            printf("arr[%d] = %d\n", i, arr[i]);
        }
        free(arr);
    }
    return 0;
}
```

**Output:**

```c
Calloc initialized array:
arr[0] = 0
arr[1] = 0
arr[2] = 0
arr[3] = 0
arr[4] = 0
```

### realloc()

**Signature:** `void *realloc(void *ptr, size_t size)`  
**Return Type:** `void *`  
**Description:** Resizes allocated memory block  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr = malloc(3 * sizeof(int));
    if (arr == NULL) return 1;
    
    // Initialize original array
    for (int i = 0; i < 3; i++) {
        arr[i] = i + 1;
    }
    
    // Resize to 5 elements
    arr = realloc(arr, 5 * sizeof(int));
    if (arr == NULL) return 1;
    
    // Initialize new elements
    arr[3] = 4;
    arr[4] = 5;
    
    printf("Resized array:\n");
    for (int i = 0; i < 5; i++) {
        printf("arr[%d] = %d\n", i, arr[i]);
    }
    
    free(arr);
    return 0;
}
```

**Output:**

```c
Resized array:
arr[0] = 1
arr[1] = 2
arr[2] = 3
arr[3] = 4
arr[4] = 5
```

### free()

**Signature:** `void free(void *ptr)`  
**Return Type:** `void`  
**Description:** Deallocates dynamic memory  
**Parameters:** `ptr` - pointer to memory to free  

### aligned_alloc() (C11)

**Signature:** `void *aligned_alloc(size_t alignment, size_t size)`  
**Return Type:** `void *`  
**Description:** Allocates memory with specified alignment  
**Parameters:** `alignment` - alignment boundary (power of 2), `size` - size in bytes (multiple of alignment)  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    // Allocate 64 bytes aligned to 16-byte boundary
    void *ptr = aligned_alloc(16, 64);
    if (ptr != NULL) {
        printf("Aligned memory allocated at: %p\n", ptr);
        printf("Address modulo 16: %d\n", (int)((uintptr_t)ptr % 16));
        free(ptr);
    }
    return 0;
}
```

**Output:** `Aligned memory allocated at: 0x...` (address will be 16-byte aligned)

---

## Mathematical Functions

### abs()

**Signature:** `int abs(int j)`  
**Return Type:** `int`  
**Description:** Returns absolute value of integer  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int a = -15, b = 25, c = 0;
    printf("abs(%d) = %d\n", a, abs(a));
    printf("abs(%d) = %d\n", b, abs(b));
    printf("abs(%d) = %d\n", c, abs(c));
    return 0;
}
```

**Output:**

```c
abs(-15) = 15
abs(25) = 25
abs(0) = 0
```

### labs()

**Signature:** `long labs(long j)`  
**Return Type:** `long`  
**Description:** Returns absolute value of long integer  

### llabs() (C99)

**Signature:** `long long llabs(long long j)`  
**Return Type:** `long long`  
**Description:** Returns absolute value of long long integer  

### div()

**Signature:** `div_t div(int numer, int denom)`  
**Return Type:** `div_t`  
**Description:** Performs integer division returning quotient and remainder  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    div_t result = div(17, 5);
    printf("17 / 5: quotient = %d, remainder = %d\n", result.quot, result.rem);
    
    result = div(-17, 5);
    printf("-17 / 5: quotient = %d, remainder = %d\n", result.quot, result.rem);
    return 0;
}
```

**Output:**

```c
17 / 5: quotient = 3, remainder = 2
-17 / 5: quotient = -3, remainder = -2
```

### ldiv()

**Signature:** `ldiv_t ldiv(long numer, long denom)`  
**Return Type:** `ldiv_t`  
**Description:** Performs long integer division returning quotient and remainder  

### lldiv() (C99)

**Signature:** `lldiv_t lldiv(long long numer, long long denom)`  
**Return Type:** `lldiv_t`  
**Description:** Performs long long integer division returning quotient and remainder  

---

## Random Number Functions

### rand()

**Signature:** `int rand(void)`  
**Return Type:** `int`  
**Description:** Generates pseudo-random number between 0 and RAND_MAX  

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(NULL));  // Seed with current time
    
    printf("Random numbers:\n");
    for (int i = 0; i < 5; i++) {
        printf("%d\n", rand());
    }
    
    // Random number in range [1, 6] (dice roll)
    int dice = (rand() % 6) + 1;
    printf("Dice roll: %d\n", dice);
    return 0;
}
```

**Output:** (varies each run)

```c
Random numbers:
1804289383
846930886
1681692777
1714636915
1957747793
Dice roll: 4
```

### srand()

**Signature:** `void srand(unsigned int seed)`  
**Return Type:** `void`  
**Description:** Seeds the random number generator  
**Parameters:** `seed` - seed value for random number generation  

---

## Program Control Functions

### exit()

**Signature:** `void exit(int status)`  
**Return Type:** `void` (noreturn)  
**Description:** Normal program termination with cleanup  
**Parameters:** `status` - exit status (0 for success, non-zero for error)  

```c
#include <stdio.h>
#include <stdlib.h>

void cleanup(void) {
    printf("Cleanup function called\n");
}

int main() {
    atexit(cleanup);
    printf("About to exit...\n");
    exit(EXIT_SUCCESS);
    printf("This won't be printed\n");
    return 0;
}
```

**Output:**

```txt
About to exit...
Cleanup function called
```

### abort()

**Signature:** `void abort(void)`  
**Return Type:** `void` (noreturn)  
**Description:** Abnormal program termination without cleanup  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("Program will abort...\n");
    abort();
    printf("This won't be printed\n");
    return 0;
}
```

**Output:** `Program will abort...` (followed by abnormal termination)

### atexit()

**Signature:** `int atexit(void (*func)(void))`  
**Return Type:** `int`  
**Description:** Registers function to be called at normal program termination  
**Return Value:** 0 on success, non-zero on failure  

### quick_exit() (C11)

**Signature:** `void quick_exit(int status)`  
**Return Type:** `void` (noreturn)  
**Description:** Quick program termination without full cleanup  

### at_quick_exit() (C11)

**Signature:** `int at_quick_exit(void (*func)(void))`  
**Return Type:** `int`  
**Description:** Registers function for quick_exit  

### _Exit() (C99)

**Signature:** `void _Exit(int status)`  
**Return Type:** `void` (noreturn)  
**Description:** Immediate program termination without any cleanup  

---

## Environment Functions

### getenv()

**Signature:** `char *getenv(const char *name)`  
**Return Type:** `char *`  
**Description:** Gets environment variable value  
**Return Value:** Pointer to value string, NULL if not found  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char *path = getenv("PATH");
    if (path != NULL) {
        printf("PATH = %s\n", path);
    } else {
        printf("PATH environment variable not found\n");
    }
    
    char *home = getenv("HOME");
    if (home != NULL) {
        printf("HOME = %s\n", home);
    }
    return 0;
}
```

**Output:** (system-dependent)

```c
PATH = /usr/local/bin:/usr/bin:/bin
HOME = /home/username
```

### system()

**Signature:** `int system(const char *command)`  
**Return Type:** `int`  
**Description:** Executes system command  
**Return Value:** Command exit status, -1 on error  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("Directory listing:\n");
    int result = system("ls -la");
    printf("Command returned: %d\n", result);
    return 0;
}
```

**Output:** (system-dependent directory listing)

---

## Search and Sort Functions

### bsearch()

**Signature:** `void *bsearch(const void *key, const void *base, size_t nmemb, size_t size, int (*compar)(const void *, const void *))`  
**Return Type:** `void *`  
**Description:** Binary search in sorted array  
**Return Value:** Pointer to found element, NULL if not found  

```c
#include <stdio.h>
#include <stdlib.h>

int compare_ints(const void *a, const void *b) {
    int ia = *(const int*)a;
    int ib = *(const int*)b;
    return (ia > ib) - (ia < ib);
}

int main() {
    int arr[] = {1, 3, 5, 7, 9, 11, 13, 15};
    int key = 7;
    int *result = bsearch(&key, arr, 8, sizeof(int), compare_ints);
    
    if (result != NULL) {
        printf("Found %d at position %ld\n", key, result - arr);
    } else {
        printf("%d not found\n", key);
    }
    return 0;
}
```

**Output:** `Found 7 at position 3`

### qsort()

**Signature:** `void qsort(void *base, size_t nmemb, size_t size, int (*compar)(const void *, const void *))`  
**Return Type:** `void`  
**Description:** Sorts array elements using quicksort algorithm  

```c
#include <stdio.h>
#include <stdlib.h>

int compare_ints(const void *a, const void *b) {
    int ia = *(const int*)a;
    int ib = *(const int*)b;
    return (ia > ib) - (ia < ib);
}

int main() {
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    printf("Original array:\n");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    
    qsort(arr, n, sizeof(int), compare_ints);
    
    printf("Sorted array:\n");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    return 0;
}
```

**Output:**

```c
Original array:
64 34 25 12 22 11 90 
Sorted array:
11 12 22 25 34 64 90 
```

---

## Multibyte Character Functions

### mblen()

**Signature:** `int mblen(const char *s, size_t n)`  
**Return Type:** `int`  
**Description:** Determines length of multibyte character  
**Return Value:** Character length in bytes, -1 if invalid, 0 for null character  

### mbtowc()

**Signature:** `int mbtowc(wchar_t *pwc, const char *s, size_t n)`  
**Return Type:** `int`  
**Description:** Converts multibyte character to wide character  

### wctomb()

**Signature:** `int wctomb(char *s, wchar_t wchar)`  
**Return Type:** `int`  
**Description:** Converts wide character to multibyte character  

### mbstowcs()

**Signature:** `size_t mbstowcs(wchar_t *dest, const char *src, size_t n)`  
**Return Type:** `size_t`  
**Description:** Converts multibyte string to wide character string  

### wcstombs()

**Signature:** `size_t wcstombs(char *dest, const wchar_t *src, size_t n)`  
**Return Type:** `size_t`  
**Description:** Converts wide character string to multibyte string  

```c
#include <stdio.h>
#include <stdlib.h>
#include <wchar.h>

int main() {
    wchar_t wide_str[] = L"Hello, World!";
    char mb_str[50];
    
    size_t converted = wcstombs(mb_str, wide_str, sizeof(mb_str));
    if (converted != (size_t)-1) {
        printf("Converted string: %s\n", mb_str);
        printf("Bytes converted: %zu\n", converted);
    }
    return 0;
}
```

**Output:**

```c
Converted string: Hello, World!
Bytes converted: 13
```

---

## Summary

The `stdlib.h` header provides 49+ functions, 5 macros, and 5 data types that form the foundation of C programming:

- **Memory Management:** `malloc()`, `calloc()`, `realloc()`, `free()`, `aligned_alloc()`
- **String Conversion:** `atoi()`, `atol()`, `atoll()`, `atof()`, `strtol()`, `strtod()`, etc.
- **Mathematical Operations:** `abs()`, `labs()`, `llabs()`, `div()`, `ldiv()`, `lldiv()`
- **Random Numbers:** `rand()`, `srand()`
- **Program Control:** `exit()`, `abort()`, `atexit()`, `quick_exit()`, `_Exit()`
- **Environment:** `getenv()`, `system()`
- **Search/Sort:** `bsearch()`, `qsort()`
- **Multibyte Support:** `mblen()`, `mbtowc()`, `wctomb()`, `mbstowcs()`, `wcstombs()`

This comprehensive reference covers all major functions and their usage patterns with practical examples.
