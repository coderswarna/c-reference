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

---

## **String Manipulation Functions**

These functions handle copying, concatenation, and duplication of null-terminated strings.

| Function | Syntax | Description | Return Value |
| :--- | :--- | :--- | :--- |
|`strcpy`| **`char *strcpy(char *dest, const char *src)`** | Copies the string `src` to `dest`. | A pointer to the destination string `dest`. |
|`strncpy`| **`char *strncpy(char *dest, const char *src, size_t n)`** | Copies at most `n` characters from `src` to `dest`. May not be null-terminated if `strlen(src) >= n`. | A pointer to the destination string `dest`. |
|`strcat`| **`char *strcat(char *dest, const char *src)`** | Appends the `src` string to the end of the `dest` string. | A pointer to the resulting string `dest`. |
|`strncat`| **`char *strncat(char *dest, const char *src, size_t n)`** | Appends at most `n` characters from `src` to `dest`, adding a null terminator. | A pointer to the resulting string `dest`. |
|`strdup`| **`char *strdup(const char *str)`** | Creates a dynamically allocated duplicate of the string `str`. | A pointer to the new string, or `NULL` on failure. |
|`strndup`| **`char *strndup(const char *str, size_t n)`** | Creates a duplicate of at most `n` characters from `str`, null-terminating the result. | A pointer to the new string, or `NULL` on failure. |

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

---

## **Non-Standard Functions**

⚠️ These functions are not part of the standard C library and may not be available on all systems. Their use can lead to non-portable code.

| Function | Syntax | Description | Return Value |
| :--- | :--- | :--- | :--- |
|`strlwr`| **`char *strlwr(char *str)`** | Converts a string to lowercase. | A pointer to the converted string. |
|`strupr`| **`char *strupr(char *str)`** | Converts a string to uppercase. | A pointer to the converted string. |
|`strrev`| **`char *strrev(char *str)`** | Reverses the characters in a string. | A pointer to the reversed string. |

---
