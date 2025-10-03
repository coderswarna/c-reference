# Complete Reference Guide to C `inttypes.h` Header File

## Introduction

The `inttypes.h` header file, introduced in C99, provides portable format specifiers for integer types defined in `stdint.h` and functions for working with the widest integer types. This header is essential for writing portable code that handles different integer sizes across various platforms.

## Header Dependencies

The `inttypes.h` header automatically includes `stdint.h`, making all fixed-width integer types available without explicitly including `stdint.h`.

## Functions

### `imaxabs()` - Absolute Value for Maximum Integer Type

**Signature:** `intmax_t imaxabs(intmax_t j);`

**Description:** Computes the absolute value of an `intmax_t` value, equivalent to `abs()` but for the maximum integer type.

**Return Value:** Returns the absolute value of the input. If the result cannot be represented, behavior is undefined.

**Example:**

```c
intmax_t negative_val = -12345;
intmax_t result = imaxabs(negative_val);
printf("imaxabs(-12345) = %jd\n", result);
// Output: imaxabs(-12345) = 12345
```

### `imaxdiv()` - Division with Quotient and Remainder

**Signature:** `imaxdiv_t imaxdiv(intmax_t numer, intmax_t denom);`

**Description:** Computes both quotient and remainder of `intmax_t` division in a single operation, similar to `div()` but for maximum integer types.

**Return Value:** Returns an `imaxdiv_t` structure containing both quotient (`quot`) and remainder (`rem`) fields.

**Example:**

```c
intmax_t numer = 45, denom = 7;
imaxdiv_t result = imaxdiv(numer, denom);
printf("imaxdiv(45, 7): quotient = %jd, remainder = %jd\n", 
       result.quot, result.rem);
// Output: imaxdiv(45, 7): quotient = 6, remainder = 3
```

### `strtoimax()` - String to Maximum Signed Integer Conversion

**Signature:** `intmax_t strtoimax(const char *restrict nptr, char **restrict endptr, int base);`

**Description:** Converts a string to an `intmax_t` value, equivalent to `strtol()` but returning the widest signed integer type.

**Parameters:**

- `nptr`: Pointer to null-terminated string to convert
- `endptr`: Pointer to pointer that will point to first invalid character
- `base`: Number base (2-36 or 0 for auto-detection)

**Return Value:** The converted `intmax_t` value. On overflow, returns `INTMAX_MAX` or `INTMAX_MIN` and sets `errno` to `ERANGE`.

**Example:**

```c
const char* str = "12345";
char* endptr;
intmax_t val = strtoimax(str, &endptr, 10);
printf("strtoimax(\"12345\", NULL, 10) = %jd\n", val);
// Output: strtoimax("12345", NULL, 10) = 12345
```

### `strtoumax()` - String to Maximum Unsigned Integer Conversion

**Signature:** `uintmax_t strtoumax(const char *restrict nptr, char **restrict endptr, int base);`

**Description:** Converts a string to a `uintmax_t` value, equivalent to `strtoul()` but returning the widest unsigned integer type.

**Return Value:** The converted `uintmax_t` value. On overflow, returns `UINTMAX_MAX` and sets `errno` to `ERANGE`.

**Example:**

```c
const char* str = "54321";
char* endptr;
uintmax_t val = strtoumax(str, &endptr, 10);
printf("strtoumax(\"54321\", NULL, 10) = %ju\n", val);
// Output: strtoumax("54321", NULL, 10) = 54321
```

### `wcstoimax()` - Wide String to Maximum Signed Integer Conversion

**Signature:** `intmax_t wcstoimax(const wchar_t *restrict nptr, wchar_t **restrict endptr, int base);`

**Description:** Wide character version of `strtoimax()`, converting wide character strings to `intmax_t`.

**Example:**

```c
const wchar_t* wstr = L"98765";
wchar_t* wendptr;
intmax_t val = wcstoimax(wstr, &wendptr, 10);
printf("wcstoimax(L\"98765\", NULL, 10) = %jd\n", val);
// Output: wcstoimax(L"98765", NULL, 10) = 98765
```

### `wcstoumax()` - Wide String to Maximum Unsigned Integer Conversion

**Signature:** `uintmax_t wcstoumax(const wchar_t *restrict nptr, wchar_t **restrict endptr, int base);`

**Description:** Wide character version of `strtoumax()`, converting wide character strings to `uintmax_t`.

**Example:**

```c
const wchar_t* wstr = L"56789";
wchar_t* wendptr;
uintmax_t val = wcstoumax(wstr, &wendptr, 10);
printf("wcstoumax(L\"56789\", NULL, 10) = %ju\n", val);
// Output: wcstoumax(L"56789", NULL, 10) = 56789
```

## Types

### `imaxdiv_t` - Structure for Division Results

**Definition:** Structure type returned by `imaxdiv()` function.

**Members:**

```c
typedef struct {
    intmax_t quot;  // Quotient
    intmax_t rem;   // Remainder  
} imaxdiv_t;
```

**Usage:**

```c
imaxdiv_t result = imaxdiv(45, 7);
printf("quot = %jd, rem = %jd\n", result.quot, result.rem);
// Output: quot = 6, rem = 3
```

## Format Specifier Macros

The `inttypes.h` header provides extensive format specifier macros for `printf()` and `scanf()` functions. These macros ensure portable formatting across different platforms and architectures.

### Macro Naming Convention

**Printf macros:** `PRI` + **format** + **type**
**Scanf macros:** `SCN` + **format** + **type**

Where:

- **format**: `d` (decimal), `i` (integer), `o` (octal), `u` (unsigned), `x` (hex lowercase), `X` (hex uppercase)
- **type**: `8`, `16`, `32`, `64`, `LEAST8`, `LEAST16`, `LEAST32`, `LEAST64`, `FAST8`, `FAST16`, `FAST32`, `FAST64`, `MAX`, `PTR`

### Printf Format Macros for Signed Integers (Decimal)

| Macro | Description | Example Usage | Output |
| :-- | :-- | :-- | :-- |
| `PRId8` | Format for `int8_t` (decimal) | `printf("%" PRId8, val8)` | `127` |
| `PRId16` | Format for `int16_t` (decimal) | `printf("%" PRId16, val16)` | `32767` |
| `PRId32` | Format for `int32_t` (decimal) | `printf("%" PRId32, val32)` | `2147483647` |
| `PRId64` | Format for `int64_t` (decimal) | `printf("%" PRId64, val64)` | `9223372036854775807` |
| `PRIdLEAST8` | Format for `int_least8_t` | `printf("%" PRIdLEAST8, val)` | `127` |
| `PRIdLEAST16` | Format for `int_least16_t` | `printf("%" PRIdLEAST16, val)` | `32767` |
| `PRIdLEAST32` | Format for `int_least32_t` | `printf("%" PRIdLEAST32, val)` | `2147483647` |
| `PRIdLEAST64` | Format for `int_least64_t` | `printf("%" PRIdLEAST64, val)` | `9223372036854775807` |
| `PRIdFAST8` | Format for `int_fast8_t` | `printf("%" PRIdFAST8, val)` | `127` |
| `PRIdFAST16` | Format for `int_fast16_t` | `printf("%" PRIdFAST16, val)` | `32767` |
| `PRIdFAST32` | Format for `int_fast32_t` | `printf("%" PRIdFAST32, val)` | `2147483647` |
| `PRIdFAST64` | Format for `int_fast64_t` | `printf("%" PRIdFAST64, val)` | `9223372036854775807` |
| `PRIdMAX` | Format for `intmax_t` | `printf("%" PRIdMAX, val)` | `9223372036854775807` |
| `PRIdPTR` | Format for `intptr_t` | `printf("%" PRIdPTR, val)` | `140737488355328` |

### Printf Format Macros for Signed Integers (Integer)

| Macro | Description | Example Usage | Output |
| :-- | :-- | :-- | :-- |
| `PRIi8` | Format for `int8_t` (integer) | `printf("%" PRIi8, val8)` | `127` |
| `PRIi16` | Format for `int16_t` (integer) | `printf("%" PRIi16, val16)` | `32767` |
| `PRIi32` | Format for `int32_t` (integer) | `printf("%" PRIi32, val32)` | `2147483647` |
| `PRIi64` | Format for `int64_t` (integer) | `printf("%" PRIi64, val64)` | `9223372036854775807` |
| `PRIiLEAST8` | Format for `int_least8_t` | `printf("%" PRIiLEAST8, val)` | `127` |
| `PRIiLEAST16` | Format for `int_least16_t` | `printf("%" PRIiLEAST16, val)` | `32767` |
| `PRIiLEAST32` | Format for `int_least32_t` | `printf("%" PRIiLEAST32, val)` | `2147483647` |
| `PRIiLEAST64` | Format for `int_least64_t` | `printf("%" PRIiLEAST64, val)` | `9223372036854775807` |
| `PRIiFAST8` | Format for `int_fast8_t` | `printf("%" PRIiFAST8, val)` | `127` |
| `PRIiFAST16` | Format for `int_fast16_t` | `printf("%" PRIiFAST16, val)` | `32767` |
| `PRIiFAST32` | Format for `int_fast32_t` | `printf("%" PRIiFAST32, val)` | `2147483647` |
| `PRIiFAST64` | Format for `int_fast64_t` | `printf("%" PRIiFAST64, val)` | `9223372036854775807` |
| `PRIiMAX` | Format for `intmax_t` | `printf("%" PRIiMAX, val)` | `9223372036854775807` |
| `PRIiPTR` | Format for `intptr_t` | `printf("%" PRIiPTR, val)` | `140737488355328` |

### Printf Format Macros for Unsigned Integers (Unsigned Decimal)

| Macro | Description | Example Usage | Output |
| :-- | :-- | :-- | :-- |
| `PRIu8` | Format for `uint8_t` (unsigned) | `printf("%" PRIu8, val8)` | `255` |
| `PRIu16` | Format for `uint16_t` (unsigned) | `printf("%" PRIu16, val16)` | `65535` |
| `PRIu32` | Format for `uint32_t` (unsigned) | `printf("%" PRIu32, val32)` | `4294967295` |
| `PRIu64` | Format for `uint64_t` (unsigned) | `printf("%" PRIu64, val64)` | `18446744073709551615` |
| `PRIuLEAST8` | Format for `uint_least8_t` | `printf("%" PRIuLEAST8, val)` | `255` |
| `PRIuLEAST16` | Format for `uint_least16_t` | `printf("%" PRIuLEAST16, val)` | `65535` |
| `PRIuLEAST32` | Format for `uint_least32_t` | `printf("%" PRIuLEAST32, val)` | `4294967295` |
| `PRIuLEAST64` | Format for `uint_least64_t` | `printf("%" PRIuLEAST64, val)` | `18446744073709551615` |
| `PRIuFAST8` | Format for `uint_fast8_t` | `printf("%" PRIuFAST8, val)` | `255` |
| `PRIuFAST16` | Format for `uint_fast16_t` | `printf("%" PRIuFAST16, val)` | `65535` |
| `PRIuFAST32` | Format for `uint_fast32_t` | `printf("%" PRIuFAST32, val)` | `4294967295` |
| `PRIuFAST64` | Format for `uint_fast64_t` | `printf("%" PRIuFAST64, val)` | `18446744073709551615` |
| `PRIuMAX` | Format for `uintmax_t` | `printf("%" PRIuMAX, val)` | `18446744073709551615` |
| `PRIuPTR` | Format for `uintptr_t` | `printf("%" PRIuPTR, val)` | `140737488355328` |

### Printf Format Macros for Unsigned Integers (Octal)

| Macro | Description | Example Usage | Output |
| :-- | :-- | :-- | :-- |
| `PRIo8` | Format for `uint8_t` (octal) | `printf("%" PRIo8, val8)` | `377` |
| `PRIo16` | Format for `uint16_t` (octal) | `printf("%" PRIo16, val16)` | `177777` |
| `PRIo32` | Format for `uint32_t` (octal) | `printf("%" PRIo32, val32)` | `37777777777` |
| `PRIo64` | Format for `uint64_t` (octal) | `printf("%" PRIo64, val64)` | `1777777777777777777777` |
| `PRIoLEAST8` | Format for `uint_least8_t` | `printf("%" PRIoLEAST8, val)` | `377` |
| `PRIoLEAST16` | Format for `uint_least16_t` | `printf("%" PRIoLEAST16, val)` | `177777` |
| `PRIoLEAST32` | Format for `uint_least32_t` | `printf("%" PRIoLEAST32, val)` | `37777777777` |
| `PRIoLEAST64` | Format for `uint_least64_t` | `printf("%" PRIoLEAST64, val)` | `1777777777777777777777` |
| `PRIoFAST8` | Format for `uint_fast8_t` | `printf("%" PRIoFAST8, val)` | `377` |
| `PRIoFAST16` | Format for `uint_fast16_t` | `printf("%" PRIoFAST16, val)` | `177777` |
| `PRIoFAST32` | Format for `uint_fast32_t` | `printf("%" PRIoFAST32, val)` | `37777777777` |
| `PRIoFAST64` | Format for `uint_fast64_t` | `printf("%" PRIoFAST64, val)` | `1777777777777777777777` |
| `PRIoMAX` | Format for `uintmax_t` | `printf("%" PRIoMAX, val)` | `1777777777777777777777` |
| `PRIoPTR` | Format for `uintptr_t` | `printf("%" PRIoPTR, val)` | `2000000000000` |

### Printf Format Macros for Unsigned Integers (Hexadecimal Lowercase)

| Macro | Description | Example Usage | Output |
| :-- | :-- | :-- | :-- |
| `PRIx8` | Format for `uint8_t` (hex lowercase) | `printf("%" PRIx8, val8)` | `ff` |
| `PRIx16` | Format for `uint16_t` (hex lowercase) | `printf("%" PRIx16, val16)` | `ffff` |
| `PRIx32` | Format for `uint32_t` (hex lowercase) | `printf("%" PRIx32, val32)` | `ffffffff` |
| `PRIx64` | Format for `uint64_t` (hex lowercase) | `printf("%" PRIx64, val64)` | `ffffffffffffffff` |
| `PRIxLEAST8` | Format for `uint_least8_t` | `printf("%" PRIxLEAST8, val)` | `ff` |
| `PRIxLEAST16` | Format for `uint_least16_t` | `printf("%" PRIxLEAST16, val)` | `ffff` |
| `PRIxLEAST32` | Format for `uint_least32_t` | `printf("%" PRIxLEAST32, val)` | `ffffffff` |
| `PRIxLEAST64` | Format for `uint_least64_t` | `printf("%" PRIxLEAST64, val)` | `ffffffffffffffff` |
| `PRIxFAST8` | Format for `uint_fast8_t` | `printf("%" PRIxFAST8, val)` | `ff` |
| `PRIxFAST16` | Format for `uint_fast16_t` | `printf("%" PRIxFAST16, val)` | `ffff` |
| `PRIxFAST32` | Format for `uint_fast32_t` | `printf("%" PRIxFAST32, val)` | `ffffffff` |
| `PRIxFAST64` | Format for `uint_fast64_t` | `printf("%" PRIxFAST64, val)` | `ffffffffffffffff` |
| `PRIxMAX` | Format for `uintmax_t` | `printf("%" PRIxMAX, val)` | `ffffffffffffffff` |
| `PRIxPTR` | Format for `uintptr_t` | `printf("%" PRIxPTR, val)` | `7fffffff0000` |

### Printf Format Macros for Unsigned Integers (Hexadecimal Uppercase)

| Macro | Description | Example Usage | Output |
| :-- | :-- | :-- | :-- |
| `PRIX8` | Format for `uint8_t` (hex uppercase) | `printf("%" PRIX8, val8)` | `FF` |
| `PRIX16` | Format for `uint16_t` (hex uppercase) | `printf("%" PRIX16, val16)` | `FFFF` |
| `PRIX32` | Format for `uint32_t` (hex uppercase) | `printf("%" PRIX32, val32)` | `FFFFFFFF` |
| `PRIX64` | Format for `uint64_t` (hex uppercase) | `printf("%" PRIX64, val64)` | `FFFFFFFFFFFFFFFF` |
| `PRIXLEAST8` | Format for `uint_least8_t` | `printf("%" PRIXLEAST8, val)` | `FF` |
| `PRIXLEAST16` | Format for `uint_least16_t` | `printf("%" PRIXLEAST16, val)` | `FFFF` |
| `PRIXLEAST32` | Format for `uint_least32_t` | `printf("%" PRIXLEAST32, val)` | `FFFFFFFF` |
| `PRIXLEAST64` | Format for `uint_least64_t` | `printf("%" PRIXLEAST64, val)` | `FFFFFFFFFFFFFFFF` |
| `PRIXFAST8` | Format for `uint_fast8_t` | `printf("%" PRIXFAST8, val)` | `FF` |
| `PRIXFAST16` | Format for `uint_fast16_t` | `printf("%" PRIXFAST16, val)` | `FFFF` |
| `PRIXFAST32` | Format for `uint_fast32_t` | `printf("%" PRIXFAST32, val)` | `FFFFFFFF` |
| `PRIXFAST64` | Format for `uint_fast64_t` | `printf("%" PRIXFAST64, val)` | `FFFFFFFFFFFFFFFF` |
| `PRIXMAX` | Format for `uintmax_t` | `printf("%" PRIXMAX, val)` | `FFFFFFFFFFFFFFFF` |
| `PRIXPTR` | Format for `uintptr_t` | `printf("%" PRIXPTR, val)` | `7FFFFFFF0000` |

### Scanf Format Macros

The `inttypes.h` header also provides corresponding scanf format macros with the `SCN` prefix. These follow the same naming pattern:

#### Scanf Format Macros for Signed Integers (Decimal)

| Macro | Description | Example Usage |
| :-- | :-- | :-- |
| `SCNd8` | Scanf format for `int8_t` (decimal) | `scanf("%" SCNd8, &val8)` |
| `SCNd16` | Scanf format for `int16_t` (decimal) | `scanf("%" SCNd16, &val16)` |
| `SCNd32` | Scanf format for `int32_t` (decimal) | `scanf("%" SCNd32, &val32)` |
| `SCNd64` | Scanf format for `int64_t` (decimal) | `scanf("%" SCNd64, &val64)` |
| `SCNdLEAST8` | Scanf format for `int_least8_t` | `scanf("%" SCNdLEAST8, &val)` |
| `SCNdLEAST16` | Scanf format for `int_least16_t` | `scanf("%" SCNdLEAST16, &val)` |
| `SCNdLEAST32` | Scanf format for `int_least32_t` | `scanf("%" SCNdLEAST32, &val)` |
| `SCNdLEAST64` | Scanf format for `int_least64_t` | `scanf("%" SCNdLEAST64, &val)` |
| `SCNdFAST8` | Scanf format for `int_fast8_t` | `scanf("%" SCNdFAST8, &val)` |
| `SCNdFAST16` | Scanf format for `int_fast16_t` | `scanf("%" SCNdFAST16, &val)` |
| `SCNdFAST32` | Scanf format for `int_fast32_t` | `scanf("%" SCNdFAST32, &val)` |
| `SCNdFAST64` | Scanf format for `int_fast64_t` | `scanf("%" SCNdFAST64, &val)` |
| `SCNdMAX` | Scanf format for `intmax_t` | `scanf("%" SCNdMAX, &val)` |
| `SCNdPTR` | Scanf format for `intptr_t` | `scanf("%" SCNdPTR, &val)` |

Similar patterns exist for `SCNi` (integer), `SCNo` (octal), `SCNu` (unsigned), and `SCNx` (hexadecimal) format specifiers.

## Practical Usage Examples

### Complete Example Program

## Standards Compliance

The `inttypes.h` header was introduced in **C99** and is part of the ISO C standard. It's also available in C++ as `<cinttypes>`. For C++ programs, the format macros are only available when `__STDC_FORMAT_MACROS` is defined before including the header.

## Key Benefits

1. **Portability**: Ensures consistent formatting across different platforms and architectures
2. **Type Safety**: Eliminates guesswork about correct format specifiers for fixed-width types
3. **Standards Compliance**: Part of the C standard library since C99
4. **Wide Character Support**: Includes wide character versions of conversion functions
5. **Maximum Integer Types**: Provides functions for the widest available integer types

The `inttypes.h` header is essential for modern C programming when working with fixed-width integer types, providing both the format specifiers needed for portable I/O operations and utility functions for the maximum-width integer types.
