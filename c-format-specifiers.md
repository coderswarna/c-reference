# C Format Specifiers and Type Specifiers: Complete Reference

A comprehensive guide to format specifiers and type specifiers in C programming language, including all modifiers and usage examples.

## Table of Contents

- [Overview](#overview)
- [Format Specifier Structure](#format-specifier-structure)
- [Basic Format Specifiers](#basic-format-specifiers)
- [Length Modifiers](#length-modifiers)
- [Width Specification](#width-specification)
- [Precision Specification](#precision-specification)
- [Flag Modifiers](#flag-modifiers)
- [scanf Format Specifiers](#scanf-format-specifiers)
- [Advanced Examples](#advanced-examples)
- [Best Practices](#best-practices)

## Overview

Format specifiers in C are essential components used with input/output functions like `printf()` and `scanf()` to specify the type and format of data being processed. They act as placeholders that tell the compiler about the type of data to be printed or scanned.

## Format Specifier Structure

A complete format specifier follows this pattern:

```c
%[flags][width][.precision][length]specifier
```

## Basic Format Specifiers

### Integer Format Specifiers

| Specifier | Data Type | Description |
|-----------|-----------|-------------|
| `%d` or `%i` | `int` | Signed decimal integer |
| `%u` | `unsigned int` | Unsigned decimal integer |
| `%o` | `int` | Unsigned octal integer |
| `%x` | `int` | Unsigned hexadecimal (lowercase a-f) |
| `%X` | `int` | Unsigned hexadecimal (uppercase A-F) |

**Examples:**

```c
int num = 42;
printf("%d\n", num);    // Output: 42
printf("%u\n", num);    // Output: 42
printf("%o\n", num);    // Output: 52
printf("%x\n", num);    // Output: 2a
printf("%X\n", num);    // Output: 2A
```

### Character and String Format Specifiers

| Specifier | Data Type | Description |
|-----------|-----------|-------------|
| `%c` | `char` | Single character |
| `%s` | `char*` | String (null-terminated) |

**Examples:**

```c
char ch = 'A';
char str[] = "Hello World";
printf("%c\n", ch);     // Output: A
printf("%s\n", str);    // Output: Hello World
```

### Floating-Point Format Specifiers

| Specifier | Data Type | Description |
|-----------|-----------|-------------|
| `%f` | `float/double` | Decimal floating-point |
| `%e` | `float/double` | Scientific notation (lowercase e) |
| `%E` | `float/double` | Scientific notation (uppercase E) |
| `%g` | `float/double` | Chooses %f or %e based on precision |
| `%G` | `float/double` | Chooses %f or %E based on precision |
| `%a` | `float/double` | Hexadecimal floating-point (lowercase) |
| `%A` | `float/double` | Hexadecimal floating-point (uppercase) |

**Examples:**

```c
float pi = 3.14159;
printf("%f\n", pi);     // Output: 3.141590
printf("%e\n", pi);     // Output: 3.141590e+00
printf("%E\n", pi);     // Output: 3.141590E+00
printf("%g\n", pi);     // Output: 3.14159
printf("%a\n", pi);     // Output: 0x1.921facp+1
```

### Special Format Specifiers

| Specifier | Description |
|-----------|-------------|
| `%p` | Pointer address in hexadecimal |
| `%n` | Number of characters written so far (no output) |
| `%%` | Literal percent sign |

**Examples:**

```c
int x = 10;
int *ptr = &x;
int count;
printf("Address: %p\n", ptr);           // Output: Address: 0x7fff5fbff6ac
printf("Hello%n World\n", &count);      // Output: Hello World (count = 5)
printf("100%% complete\n");             // Output: 100% complete
```

## Length Modifiers

Length modifiers alter the size of the expected argument:

### Short Modifiers

- **`h`**: Modifies `d`, `i`, `o`, `u`, `x`, `X` for `short int` or `unsigned short int`
- **`hh`**: Modifies for `signed char` or `unsigned char`

### Long Modifiers

- **`l`**: Modifies `d`, `i`, `o`, `u`, `x`, `X` for `long int` or `unsigned long int`
- **`ll`** or **`L`**: Modifies for `long long int` or `unsigned long long int`

### Floating-Point Modifiers

- **`l`**: With `%f`, `%e`, `%g` for `double`
- **`L`**: With `%f`, `%e`, `%g` for `long double`

### Size-Specific Modifiers

- **`j`**: For `intmax_t` and `uintmax_t`
- **`z`**: For `size_t`
- **`t`**: For `ptrdiff_t`

**Examples:**

```c
short s = 32767;
long l = 123456789L;
long long ll = 123456789012345LL;
long double ld = 3.14159265358979L;

printf("%hd\n", s);     // Output: 32767
printf("%ld\n", l);     // Output: 123456789
printf("%lld\n", ll);   // Output: 123456789012345
printf("%Lf\n", ld);    // Output: 3.141593
```

## Width Specification

Width specifies the minimum number of characters to be printed:

```c
printf("%5d\n", 42);     // Output: "   42" (right-aligned in 5 characters)
printf("%-5d\n", 42);    // Output: "42   " (left-aligned in 5 characters)
printf("%*d\n", 5, 42);  // Output: "   42" (dynamic width using variable)
```

## Precision Specification

Precision meaning varies by data type:

### For Integers (`%d`, `%i`, `%u`, `%o`, `%x`, `%X`)

Specifies minimum number of digits (adds leading zeros):

```c
printf("%.5d\n", 42);      // Output: "00042"
printf("%.3d\n", 12345);   // Output: "12345" (no truncation)
```

### For Floating-Point (`%f`, `%e`, `%g`)

Specifies number of digits after decimal point:

```c
printf("%.2f\n", 3.14159);   // Output: "3.14"
printf("%.6f\n", 3.14159);   // Output: "3.141590"
```

### For Strings (`%s`)

Specifies maximum number of characters to print:

```c
printf("%.5s\n", "Hello World");  // Output: "Hello"
```

### Dynamic Precision

```c
printf("%.*f\n", 2, 3.14159);    // Output: "3.14" (precision specified by argument)
```

## Flag Modifiers

Flags modify the output format:

| Flag | Description |
|------|-------------|
| `-` | Left-justify output |
| `+` | Always show sign for numbers |
| ` ` (space) | Prefix positive numbers with space |
| `#` | Use alternative form (0x for hex, decimal point for floats) |
| `0` | Pad with zeros instead of spaces |

**Examples:**

```c
printf("%-10d|\n", 42);    // Output: "42        |" (left-justified)
printf("%+d\n", 42);       // Output: "+42"
printf("% d\n", 42);       // Output: " 42"
printf("%#x\n", 255);      // Output: "0xff"
printf("%010d\n", 42);     // Output: "0000000042"
```

## scanf Format Specifiers

For input with `scanf()`, similar specifiers are used:

### Basic scanf Specifiers

| Specifier | Input Type | Description |
|-----------|------------|-------------|
| `%d` | `int*` | Decimal integer |
| `%i` | `int*` | Integer (detects base automatically) |
| `%u` | `unsigned int*` | Unsigned integer |
| `%o` | `int*` | Octal integer |
| `%x` | `int*` | Hexadecimal integer |
| `%f` | `float*` | Floating-point number |
| `%lf` | `double*` | Double precision floating-point |
| `%c` | `char*` | Single character |
| `%s` | `char*` | String |

**Examples:**

```c
int num;
float value;
char ch;
char str[100];

scanf("%d", &num);       // Read integer
scanf("%f", &value);     // Read float
scanf(" %c", &ch);       // Read character (note the space)
scanf("%s", str);        // Read string
```

### scanf Width and Suppression

```c
scanf("%5d", &num);      // Read at most 5 digits
scanf("%*d %d", &num);   // Skip first integer, read second
```

### Scanset Specifiers

```c
scanf("%[abc]", str);    // Read only characters a, b, c
scanf("%[^abc]", str);   // Read any character except a, b, c
scanf("%[0-9]", str);    // Read only digits
scanf("%[^\n]", str);    // Read until newline (entire line)
```

## Advanced Examples

### Complex Format Strings

```c
// Width, precision, and flags combined
printf("%+08.2f\n", 3.14159);    // Output: "+0003.14"
printf("%-15.10s|\n", "Hello");  // Output: "Hello          |"

// Multiple specifiers
char name[] = "Alice";
int age = 25;
float height = 5.6;
printf("Name: %s, Age: %d, Height: %.1f\n", name, age, height);
// Output: Name: Alice, Age: 25, Height: 5.6
```

### Dynamic Formatting

```c
int width = 10, precision = 2;
float value = 3.14159;
printf("%*.*f\n", width, precision, value);  // Output: "      3.14"
```

### Error Handling with scanf

```c
int number;
int result = scanf("%d", &number);
if (result == 1) {
    printf("Successfully read: %d\n", number);
} else {
    printf("Input error or EOF\n");
}
```

### Complete Example Program

```c
#include <stdio.h>

int main() {
    int integer = 42;
    float floating = 3.14159;
    char character = 'A';
    char string[] = "Hello, World!";
    void *pointer = &integer;
    
    printf("=== Basic Format Specifiers ===\n");
    printf("Integer: %d\n", integer);
    printf("Float: %f\n", floating);
    printf("Character: %c\n", character);
    printf("String: %s\n", string);
    printf("Pointer: %p\n", pointer);
    
    printf("\n=== Width and Precision ===\n");
    printf("Width 10: '%10d'\n", integer);
    printf("Left-aligned: '%-10d'\n", integer);
    printf("Zero-padded: '%010d'\n", integer);
    printf("Precision 2: '%.2f'\n", floating);
    
    printf("\n=== Flags ===\n");
    printf("With sign: '%+d'\n", integer);
    printf("With space: '% d'\n", integer);
    printf("Hex with #: '%#x'\n", integer);
    
    printf("\n=== Length Modifiers ===\n");
    long long bignum = 123456789012345LL;
    printf("Long long: %lld\n", bignum);
    
    return 0;
}
```

## Best Practices

1. **Always use appropriate length modifiers** for the data type being processed
2. **Check return values** of `scanf()` to handle input errors
3. **Use precision specifiers** to control floating-point output format
4. **Be careful with string input** to avoid buffer overflows when using `%s`
5. **Use width specifiers** for consistent formatting and alignment
6. **Add spaces in scanf format strings** when reading characters after other inputs
7. **Use scansets** for more controlled string input
8. **Always validate input** when using scanf functions
9. **Consider using safer alternatives** like `fgets()` for string input
10. **Use `%%` to print literal percent signs**

## Common Pitfalls

- **Forgetting `&` operator** with scanf for variables
- **Not checking scanf return values** for input validation
- **Buffer overflow** with `%s` in scanf
- **Mixing `%lf` in printf** (should be `%f` for both float and double)
- **Wrong length modifiers** causing undefined behavior
- **Not handling whitespace** properly in scanf format strings

---

This reference guide provides comprehensive coverage of C format specifiers and their modifications. Keep it handy for quick reference during C programming!
