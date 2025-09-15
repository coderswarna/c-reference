# **Complete Reference for `uchar.md` in C**

## **Overview**

This reference covers the **`uchar.h`** header introduced in C11 (with full support in C23), which provides functions and types for converting between multibyte sequences and UTF-16/UTF-32 code units. It includes:

- **Types**: `char16_t`, `char32_t`
- **Functions**: `mbrtoc16`, `c16rtomb`, `mbrtoc32`, `c32rtomb`
- **Macros**: `NULL`

Each item below is described with its purpose, signature (return type and parameters), behavior, an example snippet, and expected output.

***

## **Types**

- **`char16_t`**:

  - 16-bit unsigned integer type capable of holding UTF-16 code units.
  - Used to represent UTF-16 code units in a portable way.

- **`char32_t`**:

  - 32-bit unsigned integer type capable of holding UTF-32 code units.
  - Used to represent Unicode code points directly.

***

## **Macros**

- **`NULL`**

  - Expands to the null pointer constant.
  - Used in `mbrtoc16`/`mbrtoc32` to reset the conversion state.

***

## **Functions**

### **1. size_t mbrtoc16(char16_t *pc16, const char *s, size_t n, mbstate_t *ps);**

Converts up to `n` bytes from the multibyte sequence `s` into a single UTF-16 code unit.

- Return value:
  - `(size_t)-2` if conversion state is incomplete and more bytes are needed.
  - `(size_t)-1` on encoding error (sets `errno` to `EILSEQ`).
  - `0` if `s` points to the null terminator (stores `u'\0'` in `*pc16` if non-NULL).
  - Otherwise, the number of bytes processed (1–MB_CUR_MAX).
- Parameters:
  - `pc16`: Destination pointer for the UTF-16 code unit (may be NULL to just advance state).
  - `s`: Pointer to the beginning of the multibyte sequence.
  - `n`: Maximum number of bytes available in `s`.
  - `ps`: Pointer to `mbstate_t` object for tracking shift state (or NULL to use internal static state).

#### **Example Program 1**

```c
#include <stdio.h>
#include <uchar.h>
#include <wchar.h>
#include <string.h>
#include <errno.h>

int main(void) {
    const char *utf8 = "Ω";        // Greek capital omega, U+03A9
    mbstate_t state = {0};
    char16_t c16;
    size_t ret = mbrtoc16(&c16, utf8, strlen(utf8), &state);

    if (ret > 0) {
        printf("Processed bytes: %zu\n", ret);
        printf("UTF-16 code unit: 0x%04X\n", c16);
    } else if (ret == (size_t)-1) {
        perror("mbrtoc16 error");
    }
    return 0;
}
```

**Expected output:**

```c
Processed bytes: 2
UTF-16 code unit: 0x03A9
```

***

### **2. size_t c16rtomb(char *s, char16_t c16, mbstate_t *ps);**

Converts a single UTF-16 code unit `c16` into its multibyte (UTF-8) representation stored in `s`.

- Return value:
  - `(size_t)-1` on encoding error (sets `errno` to `EILSEQ`).
  - Otherwise, number of bytes written to `s` (≥1).
- Parameters:
  - `s`: Buffer (at least MB_CUR_MAX bytes) to receive the multibyte sequence (may be NULL to just reset state).
  - `c16`: The UTF-16 code unit to convert.
  - `ps`: Pointer to `mbstate_t` for shift state (or NULL for internal state).

#### **Example Program 2**

```c
#include <stdio.h>
#include <uchar.h>
#include <string.h>
#include <errno.h>

int main(void) {
    mbstate_t state = {0};
    char buf[MB_CUR_MAX];
    char16_t c16 = 0x03A9;        // Greek capital omega

    size_t ret = c16rtomb(buf, c16, &state);
    if (ret != (size_t)-1) {
        printf("Bytes written: %zu\n", ret);
        printf("UTF-8 sequence: ");
        for (size_t i = 0; i < ret; i++) {
            printf("%02X ", (unsigned char)buf[i]);
        }
        printf("\n");
    } else {
        perror("c16rtomb error");
    }
    return 0;
}
```

**Expected output:**

```c
Bytes written: 2
UTF-8 sequence: CE A9 
```

***

### **3. size_t mbrtoc32(char32_t *pc32, const char *s, size_t n, mbstate_t *ps);**

Converts up to `n` bytes from the multibyte sequence `s` into a single UTF-32 code unit.

- Return value:
  - `(size_t)-2` if incomplete multibyte sequence.
  - `(size_t)-1` on error (sets `errno`).
  - `0` if null terminator encountered (stores `U'\0'`).
  - Otherwise, number of bytes processed.
- Parameters:
  - `pc32`: Destination for UTF-32 code unit (may be NULL to skip actual storage).
  - `s`: Pointer to multibyte data.
  - `n`: Maximum bytes to examine.
  - `ps`: Shift state object pointer (or NULL).

#### **Example Program 3**

```c
#include <stdio.h>
#include <uchar.h>
#include <string.h>
#include <errno.h>

int main(void) {
    const char *utf8 = "𐍈";       // Gothic letter faihu, U+10348 (requires surrogate pair)
    mbstate_t state = {0};
    char32_t c32;
    size_t ret = mbrtoc32(&c32, utf8, strlen(utf8), &state);

    if (ret > 0) {
        printf("Processed bytes: %zu\n", ret);
        printf("UTF-32 code point: U+%X\n", c32);
    } else if (ret == (size_t)-1) {
        perror("mbrtoc32 error");
    }
    return 0;
}
```

**Expected output:**

```txt
Processed bytes: 4
UTF-32 code point: U+10348
```

***

### **4. size_t c32rtomb(char *s, char32_t c32, mbstate_t *ps);**

Converts a single UTF-32 code point `c32` into its multibyte (UTF-8) sequence stored in `s`.

- Return value:
  - `(size_t)-1` on encoding error.
  - Otherwise, number of bytes written.
- Parameters:
  - `s`: Buffer for the resulting bytes (or NULL to reset state).
  - `c32`: The UTF-32 code point to convert.
  - `ps`: Conversion state pointer (or NULL).

#### **Example Program 4**

```c
#include <stdio.h>
#include <uchar.h>
#include <errno.h>

int main(void) {
    mbstate_t state = {0};
    char buf[MB_CUR_MAX];
    char32_t c32 = 0x10348;      // Gothic letter faihu

    size_t ret = c32rtomb(buf, c32, &state);
    if (ret != (size_t)-1) {
        printf("Bytes written: %zu\n", ret);
        printf("UTF-8 sequence: ");
        for (size_t i = 0; i < ret; i++) {
            printf("%02X ", (unsigned char)buf[i]);
        }
        printf("\n");
    } else {
        perror("c32rtomb error");
    }
    return 0;
}
```

**Expected output:**

```txt
Bytes written: 4
UTF-8 sequence: F0 90 8D 88 
```

***

## **Notes and Best Practices**

- Always initialize the **`mbstate_t`** object (commonly with **`{0}`**) before first use.
- Use **`NULL`** for **`ps`** and **`pc16`/`pc32`** to reset state or to just consume input without storing output.
- These functions handle partial sequences gracefully—check for **`(size_t)-2`** to know when to read more bytes.
- Buffers passed to **`c16rtomb`** and **`c32rtomb`** must be at least **`MB_CUR_MAX`** bytes long, which is typically 4 on UTF-8 locales.

This completes the exhaustive listing of all types, macros, and functions provided by **`uchar.h`** in standard C.
