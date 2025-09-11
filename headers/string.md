# **Complete Reference for string.h in C**

## **Introduction**

The `<string.h>` header in C provides a comprehensive set of functions for manipulating and handling strings and memory blocks. It includes operations such as copying, concatenation, comparison, searching, and tokenization of strings, as well as functions to handle raw memory areas. These functions are essential for everyday programming tasks involving text processing and buffer management, offering efficient and standardized tools for working with null-terminated character arrays and arbitrary memory regions.

---

## **Macros**

Description: This section lists macros available in the C standard library related to string and memory handling.

| Name |  Type | Syntax | Description | Use Case | Return Value | Example Code | Example Output |
| --- | --- | --- | --- | --- | --- | --- | --- |
| NULL | Macro | ```#define NULL ((void*)0)``` | NULL pointer constant | Used to represent null pointer | Null pointer value | `char *ptr = NULL;` | ptr is NULL pointer |

## **Types**

Description: This section lists types available in the C standard library related to string and memory handling.

| Name | Type | Syntax | Description | Use Case | Return Value | Example Code | Example Output |
| --- | --- | --- | --- | --- | --- | --- | --- |
| size_t | Data Type | `typedef unsigned long size_t` | Unsigned integer type for sizes | Used for array indices and memory sizes | Unsigned integer | `size_t len = strlen("hello");` | `len()` is of type `size_t` |

## **Memory Manipulation Functions**

These functions operate on blocks of memory and are not limited by null terminators.

| Function | Syntax | Description | Return Value |
| :--- | :--- | :--- | :--- |
|`memcpy`| `void *memcpy(void *dest, const void *src, size_t n)` | Copies `n` bytes from `src` to `dest`. **Undefined behavior** if memory areas overlap. | A pointer to the destination `dest`. |
|`memmove`| `void *memmove(void *dest, const void *src, size_t n)` | Copies `n` bytes from `src` to `dest`. **Safely handles** overlapping memory areas. | A pointer to the destination `dest`. |
|`memchr`| `void *memchr(const void *str, int c, size_t n)` | Searches the first `n` bytes of `str` for the first occurrence of the character `c`. | A pointer to the matching byte, or `NULL` if not found. |
|`memcmp`| `int memcmp(const void *str1, const void *str2, size_t n)` | Compares the first `n` bytes of `str1` and `str2`. | An integer: `< 0` if `str1 < str2`, `0` if equal, `> 0` if `str1 > str2`. |
|`memset`| `void *memset(void *str, int c, size_t n)` | Fills the first `n` bytes of the memory area `str` with the constant byte `c`. | A pointer to the memory area `str`. |
|`memccpy`|`void *memccpy(void *dest, const void *src, int c, size_t n)`| Copies up to `n` bytes from `src` to `dest`, stopping after `c` is copied. | A pointer to the byte in `dest` after `c`, or `NULL` if `c` was not found. |

### **Examples for Memory Manipulation Function**

- **`memchr()`**

```c
#include <stdio.h>
#include <string.h>
    int main() {
    char str[] = "Hello World";
    char *result = (char*)memchr(str, 'o', strlen(str));
    if (result != NULL)
        printf("Found 'o' at position: %ld\n", result - str);
    else
        printf("Character not found\n");
    return 0;
}

```

```txt
Output: Found 'o' at position: 4
```

- **`memcmp()`**

```c
#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABC";
    char str2[] = "ABD";

    int result = memcmp(str1, str2, 3);

    if (result == 0)
        printf("Memory blocks are equal\n");
    else if (result < 0)
        printf("First block is less than second\n");
    else
        printf("First block is greater than second\n");
    return 0;
}

```

```txt
Output: First block is less than second
```

- **`memcpy`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char source[] = "Hello World";
    char destination[^20];
    memcpy(destination, source, strlen(source) + 1);
    printf("Source: %s\n", source);
    printf("Destination: %s\n", destination);
    return 0;
}
```

```txt
Output:
    Source: Hello World
    Destination: Hello World
```

- **`memmove()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "1234567890";

    printf("Before memmove: %s\n", str);
    memmove(str + 2, str, 5); // Overlapping memory
    printf("After memmove: %s\n", str);
    return 0;
}
```

```txt
Output:
    Before memmove: 1234567890
    After memmove: 1212347890
```

- **`memset()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "Hello World";
    printf("Before memset: %s\n", str);
    memset(str, '\*', 5);
    printf("After memset: %s\n", str);
    return 0;
}
```

```txt
Output:
    Before memset: Hello World
    After memset: ***** World
```

---

## **String Manipulation Functions**

These functions handle copying, concatenation, and duplication of null-terminated strings.

| Function | Syntax | Description | Return Value |
| :--- | :--- | :--- | :--- |
|`strcpy`| **`char *strcpy(char *dest, const char *src)`** | Copies the string `src` to `dest`. | A pointer to the destination string `dest`. |
|`strncpy`| **`char *strncpy(char *dest, const char *src, size_t n)`** | Copies at most `n` characters from `src` to `dest`. May not be null-terminated if `strlen(src) >= n`. | A pointer to the destination string `dest`. |
|`strcat`| **`char *strcat(char *dest, const char *src)`** | Appends the `src` string to the end of the `dest` string. | A pointer to the resulting string `dest`. |
|`strncat`| **`char *strncat(char *dest, const char *src, size_t n)`** | Appends at most `n` characters from `src` to `dest`, adding a null terminator. | A pointer to the resulting string `dest`. |
|`strdup` (POSIX/GNU)| **`char *strdup(const char *str)`** | Creates a dynamically allocated duplicate of the string `str`. | A pointer to the new string, or `NULL` on failure. |
|`strndup` (POSIX/GNU)| **`char *strndup(const char *str, size_t n)`** | Creates a duplicate of at most `n` characters from `str`, null-terminating the result. | A pointer to the new string, or `NULL` on failure. |

### **Examples for String Manipulation Functions**

- **`strcpy()`**

```c
#include <stdio.h>
#include <string.h>

int main() {
    char source[] = "Programming";
    char destination[^20];
    strcpy(destination, source);
    printf("Source: %s\n", source);
    printf("Destination: %s\n", destination);
    return 0;
}
```

```txt
Output:
    Source: Programming
    Destination: Programming
```

- **`strncpy()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char source[] = "Programming";
    char destination[20] = {0}; // Initialize with zeros
    strncpy(destination, source, 4);
    destination[4] = '\0'; // Null terminate manually
    printf("Source: %s\n", source);
    printf("Destination: %s\n", destination);
    return 0;
}
```

```txt
Output:
    Source: Programming
    Destination: Prog
```

- **`strcat()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str1[] = "Hello ";
    char str2[] = "World!";
    printf("Before strcat: %s\n", str1);
    strcat(str1, str2);
    printf("After strcat: %s\n", str1);
    return 0;
}
```

```txt
Output:
    Before strcat: Hello
    After strcat: Hello World!
```

- **`strncat()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str1[^30] = "Hello ";
    char str2[] = "Programming World!";
    printf("Before strncat: %s\n", str1);
    strncat(str1, str2, 4);
    printf("After strncat: %s\n", str1);
    return 0;
}
```

```txt
Output:
    Before strncat: Hello
    After strncat: Hello Prog
```

- **`strdup()`**

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
int main() {
    char *original = "Hello World";
    char *copy = strdup(original);
    if (copy) {
        printf("Duplicated: %s\n", copy);
        free(copy); // Don't forget to free!
    }
    return 0;
}
```

```txt
Output:
    Duplicated: Hello World
```

---

## **String Searching & Comparison Functions**

These functions are used to find characters/substrings or compare two strings.

| Function | Syntax | Description | Return Value |
| :--- | :--- | :--- | :--- |
|**`strcmp`**| **`int strcmp(const char *str1, const char *str2)`** | Performs a lexicographical comparison of two strings. | An integer: `< 0` if `str1 < str2`, `0` if equal, `> 0` if `str1 > str2`. |
|**`strncmp`**| **`int strncmp(const char *str1, const char *str2, size_t n)`** | Compares at most the first `n` characters of two strings. | An integer: `< 0` if `str1 < str2`, `0` if equal, `> 0` if `str1 > str2`. |
|**`strcoll`**| **`int strcoll(const char *str1, const char *str2)`** | Compares two strings according to the current locale's collating order. | A locale-dependent integer comparison result. |
|**`strchr`**| **`char *strchr(const char *str, int c)`** | Finds the first occurrence of the character `c` in `str`. | A pointer to the first occurrence, or `NULL` if not found. |
|**`strrchr`**| **`char *strrchr(const char *str, int c)`** | Finds the last occurrence of the character `c` in `str`. | A pointer to the last occurrence, or `NULL` if not found. |
|**`strstr`**| **`char *strstr(const char *haystack, const char *needle)`** | Finds the first occurrence of the substring `needle` in `haystack`. | A pointer to the beginning of the substring, or `NULL` if not found. |
|**`strpbrk`**| **`char *strpbrk(const char *str1, const char *str2)`** | Finds the first character in `str1` that matches any character in `str2`. | A pointer to the located character, or `NULL` if no match is found. |

### **Examples for String searching & comparison Functions**

- **`strcmp()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str1[] = "Apple";
    char str2[] = "Banana";
    int result = strcmp(str1, str2);
    printf("Comparing '%s' and '%s':\n", str1, str2);
    if (result == 0)
        printf("Strings are equal\n");
    else if (result < 0)
        printf("First string is less than second\n");
    else
        printf("First string is greater than second\n");
    return 0;
}
```

```txt
Output:
    Comparing 'Apple' and 'Banana':
    First string is less than second
```

- **`strncmp()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str1[] = "Hello World";
    char str2[] = "Hello Programming";
    int result = strncmp(str1, str2, 5);
    printf("Comparing first 5 characters:\n");
    printf("String 1: %s\n", str1);
    printf("String 2: %s\n", str2);
    if (result == 0)
        printf("First 5 characters are equal\n");
    else
        printf("First 5 characters are not equal\n");
    return 0;
}
```

```txt
Output:
    Comparing first 5 characters:
    String 1: Hello World
    String 2: Hello Programming
    First 5 characters are equal
```

- **`strchr()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "Programming in C";
    char *result = strchr(str, 'g');
    if (result != NULL) {
        printf("Found 'g' at position: %ld\n", result - str);
        printf("Remaining string: %s\n", result);
    } else {
        printf("Character not found\n");
        }
    return 0;
}
```

```txt
Output:
    Found 'g' at position: 3
    Remaining string: gramming in C
```

- **`strrchr()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "Programming in C";
    char *result = strrchr(str, 'g');
    if (result != NULL) {
        printf("Last 'g' found at position: %ld\n", result - str);
        printf("Remaining string: %s\n", result);
    } else {
        printf("Character not found\n");
    }
    return 0;
}
```

```txt
Output:
    Last 'g' found at position: 7
    Remaining string: g in C
```

- **`strstr()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char haystack[] = "Programming in C language";
    char needle[] = "in C";
    char *result = strstr(haystack, needle);
    if (result != NULL) {
        printf("Found '%s' at position: %ld\n", needle, result - haystack);
        printf("Remaining string: %s\n", result);
    } else {
        printf("Substring not found\n");
    }
    return 0;
}
```

```txt
Output:
    Found 'in C' at position: 12
    Remaining string: in C language
```

- **`strpbrk()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "Hello World";
    char charset[] = "aeiou";
    char *result = strpbrk(str, charset);
    if (result != NULL) {
        printf("First vowel found: '%c' at position: %ld\n", *result, result - str);
    } else {
        printf("No vowels found\n");
    }
    return 0;
}
```

```txt
Output:
    First vowel found: 'e' at position: 1
```

---

## **Other String Operations**

Functions for measurement, tokenization, and transformation.

| Function | Syntax | Description | Return Value |
| :--- | :--- | :--- | :--- |
|`strlen`| **`size_t strlen(const char *str)`** | Calculates the length of a string, excluding the null terminator. | The length of the string as a `size_t`. |
|`strspn`| **`size_t strspn(const char *str1, const char *str2)`** | Calculates the length of the initial segment of `str1` consisting only of characters from `str2`. | The length of the segment as a `size_t`. |
|`strcspn`| **`size_t strcspn(const char *str1, const char *str2)`** | Calculates the length of the initial segment of `str1` that consists of characters *not* in `str2`. | The length of the segment as a `size_t`. |
|`strtok`| **`char *strtok(char *str, const char *delim)`** | Breaks `str` into tokens using `delim`. **Modifies the source string.** | A pointer to the next token, or `NULL` if no more tokens exist. |
|`strerror`| **`char *strerror(int errnum)`** | Returns a pointer to a string that describes the error code `errnum`. | A pointer to the corresponding error message string. |
|`strxfrm`| **`size_t strxfrm(char *dest, const char *src, size_t n)`**| Transforms `src` based on the current locale, placing `n` bytes of the result in `dest`. | The length of the transformed string. |

### **Examples for other String Functions**

- **`strspn()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "123456hello";
    char digits[] = "0123456789";
    size_t len = strspn(str, digits);
    printf("String: %s\n", str);
    printf("Length of initial digit sequence: %zu\n", len);
    return 0;
}
```

```txt
Output:
    String: 123456hello
    Length of initial digit sequence: 6
```

- **`str()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "Hello123World";
    char digits[] = "0123456789";
    size_t len = strcspn(str, digits);
    printf("String: %s\n", str);
    printf("Length before first digit: %zu\n", len);
    return 0;
}
```

```txt
Output:
    String: Hello123World
    Length before first digit: 5
```

- **`strlen()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "Programming in C";
    size_t length = strlen(str);
    printf("String: %s\n", str);
    printf("Length: %zu characters\n", length);
    return 0;
}

```

```txt
Output:
    String: Programming in C
    Length: 16 characters
```

- **`strtok()`**

```c
#include <stdio.h>
#include <string.h>
int main() {
    char str[] = "apple,banana,cherry,date";
    char *token;
    printf("Original string: %s\n", "apple,banana,cherry,date");
    printf("Tokens:\n");
    token = strtok(str, ",");
    while (token != NULL) {
        printf(" %s\n", token);
        token = strtok(NULL, ",");
    }
    return 0;
}
```

```txt
Output:
    Original string: apple,banana,cherry,date
    Tokens:
    apple
    banana
    cherry
    date
```

- **`strerror()`**

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>
int main() {
    // Simulate some error codes
    for (int i = 1; i <= 3; i++) {
    printf("Error %d: %s\n", i, strerror(i));
    }
    return 0;
}
```

```txt
Output:
    Error 1: Operation not permitted
    Error 2: No such file or directory
    Error 3: No such process
```

---

## **Non-Standard Functions**

⚠️ These functions are not part of the standard C library and may not be available on all systems. Their use can lead to non-portable code.

| Function | Syntax | Description | Return Value |
| :--- | :--- | :--- | :--- |
|`strlwr`| **`char *strlwr(char *str)`** | Converts a string to lowercase. | A pointer to the converted string. |
|`strupr`| **`char *strupr(char *str)`** | Converts a string to uppercase. | A pointer to the converted string. |
|`strrev`| **`char *strrev(char *str)`** | Reverses the characters in a string. | A pointer to the reversed string. |
|`strset()`|**`char *strset(char *_Str, int _Val)`**|Set all characters of a given string (except for the terminating null character \0) to a specified character.|It returns a pointer to char|
|`strnset()`|**`char *strnset(char *_Str, int _Val, size_t _MaxCount)`**|Set portion of characters within a string to a given character.|It returns a pointer to char|

---

## **Related Keywords and Qualifiers**

- **`restrict`**: Keyword used in function parameters to indicate non-overlapping memory
regions
- **`const`**: Qualifier indicating the pointed-to data should not be modified
- **`volatile`**: Rarely used in string.h, indicates value may change unexpectedly

## **Best Practices and Important Notes**

1. **Always include the header**: Use `#include <string.h>` before calling any string functions
2. **Check return values**: Always verify that search functions don't return `NULL`
3. **Buffer management**: Ensure destination buffers are large enough for string operations
4. **Null termination**: Remember that strncpy may not null-terminate the destination string
5. **Memory overlap**: Use memmove instead of memcpy when source and destination may overlap
6. **Use `size_t`**: Always use size_t for string lengths and loop counters rather than int.

---
