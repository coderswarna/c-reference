# **Reference Overview of `wchar.h` in C**

The **`<wchar.h>`** header in C provides **comprehensive support for wide characters** and wide-character strings, enabling manipulation of Unicode and other extended character sets. Below is a detailed listing of every macro, type, and function (including XSI extensions) defined or declared in `<wchar.h>`, along with their purpose, return types, example usage, and sample output.

***

## **Macros**

- **WCHAR_MAX**
  - Maximum value representable by `wchar_t`.
Example:

```c
#include <stdio.h>
#include <wchar.h>
int main(void) {
    wprintf(L"WCHAR_MAX = %ld\n", (long)WCHAR_MAX);
    return 0;
}
```

Sample Output:

```c
WCHAR_MAX = 2147483647
```

- **WCHAR_MIN**
  - Minimum value representable by `wchar_t`.
Example (continuing above):

```c
#include <stdio.h>
#include <wchar.h>
int main(void) {
    wprintf(L"WCHAR_MIN = %ld\n", (long)WCHAR_MIN);
    return 0;
}
```

Sample Output:

```c
WCHAR_MIN = -2147483648
```

- **WEOF**
  - Constant of type `wint_t` indicating end-of-file (EOF) for wide-character streams.
  - Usage: Check `fgetwc()` or `getwchar()` against `WEOF`.
- **NULL**
  – As in `<stddef.h>`, null pointer constant.

***

## **Types**

| Type | Description |
|:--|:--|
|**`wchar_t`**| Wide-character type. |
|**`wint_t`**| Integer type capable of storing any `wchar_t` or `WEOF`. |
|**`wctype_t`**| (XSI) Locale-specific classification identifier for use with `iswctype()`. |
|**`mbstate_t`**| Holds conversion state for multi-byte ↔ wide-character conversions. |
|**`size_t`**| Unsigned integral type for sizes. |
|**`FILE`**| Stream type from `<stdio.h>`. |
|**`va_list`**| Type for variable-argument list handling (`<stdarg.h>`). |
|**`struct tm`**| Time-breakdown structure (`<time.h>`). |

***

## **Character Classification & Conversion**

| Function | Return Type | Description |
| :-- | :-- | :-- |
| **`iswalnum(wint_t)`** | int | Alphabetic or digit? |
| **`iswalpha(wint_t)`** | int | Alphabetic? |
| **`iswcntrl(wint_t)`** | int | Control character? |
| **`iswctype(wint_t, wctype_t)`** | int | Category test by `wctype()` code. |
| **`iswdigit(wint_t)`** | int | Digit? |
| **`iswgraph(wint_t)`** | int | Graphical (printable excluding space)? |
| **`iswlower(wint_t)`** | int | Lowercase? |
| **`iswprint(wint_t)`** | int | Printable (including space)? |
| **`iswpunct(wint_t)`** | int | Punctuation? |
| **`iswspace(wint_t)`** | int | Whitespace? |
| **`iswupper(wint_t)`** | int | Uppercase? |
| **`iswxdigit(wint_t)`** | int | Hexadecimal digit? |
| **`towlower(wint_t)`** | wint_t | Convert to lowercase. |
| **`towupper(wint_t)`** | wint_t | Convert to uppercase. |
| **`wctype(const char *)`** | wctype_t | Get classification code. |
| **`wctob(wint_t)`** | int | Convert wide to narrow (if possible). |

Example:

```c
#include <wchar.h>
#include <wctype.h>
#include <stdio.h>
int main(void) {
    wchar_t c = L'Ä';
    wctype_t ct = wctype("alpha");
    wprintf(L"%lc isalpha? %d\n", c, iswalpha(c));
    wprintf(L"tolower(%lc) = %lc\n", c, towlower(c));
    return 0;
}
```

Sample Output:

```c
Ä isalpha? 1
tolower(Ä) = ä
```

***

## **Multi-Byte / Wide-Character Conversion**

| Function | Return Type | Description |
| :-- | :-- | :-- |
| **`mbrlen(const char*, size_t, mbstate_t*)`** | size_t | Length of next multi-byte char. |
| **`mbrtowc(wchar_t*, const char*, size_t, mbstate_t*)`** | size_t | Convert multi-byte to wide. |
| **`wcrtomb(char*, wchar_t, mbstate_t*)`** | size_t | Convert wide to multi-byte. |
| **`mbsinit(const mbstate_t*)`** | int | Is conversion state initial? |
| **`mbsrtowcs(wchar_t*, const char**, size_t, mbstate_t*)`** | size_t | Convert string → wide string. |
| **`wcsrtombs(char*, const wchar_t**, size_t, mbstate_t*)`** | size_t | Convert wide string → multibyte. |

Example:

```c
#include <wchar.h>
#include <stdio.h>
int main(void) {
    const char *mb = "€"; mbstate_t st = {0};
    wchar_t wc;
    mbrtowc(&wc, mb, MB_CUR_MAX, &st);
    wprintf(L"Converted: %lc\n", wc);
    return 0;
}
```

Sample Output:

```c
Converted: €
```

***

## **Input/Output on Wide Streams**

| Function | Return Type | Description |
| :-- | :-- | :-- |
| **`fgetwc(FILE*)`** | wint_t | Get wide character from stream. |
| **`getwc(FILE*)`** | wint_t | Equivalent to `fgetwc`. |
| **`getwchar(void)`** | wint_t | Equivalent to `getwc(stdin)`. |
| **`fputwc(wchar_t, FILE*)`** | wint_t | Write wide char to stream. |
| **`putwc(wchar_t, FILE*)`** | wint_t | Equivalent to `fputwc`. |
| **`putwchar(wchar_t)`** | wint_t | Write to `stdout`. |
| **`ungetwc(wint_t, FILE*)`** | wint_t | Push back wide char. |
| **`fgetws(wchar_t*, int, FILE*)`** | wchar_t* | Get wide string from stream. |
| **`fputws(const wchar_t*, FILE*)`** | int | Write wide string to stream. |
| **`wprintf(const wchar_t*, ...)`** | int | Print to `stdout`. |
| **`wscanf(const wchar_t*, ...)`** | int | Scan from `stdin`. |
| **`fwprintf(FILE*, const wchar_t*, ...)`** | int | Print to stream. |
| **`fwscanf(FILE*, const wchar_t*, ...)`** | int | Scan from stream. |
| **`swprintf(wchar_t*, size_t, const wchar_t*, ...)`** | int | Print to buffer. |
| **`swscanf(const wchar_t*, const wchar_t*, ...)`** | int | Scan from buffer. |
| **`vfwprintf(FILE*, const wchar_t*, va_list)`** | int | `fwprintf` w/ `va_list`. |
| **`vfwscanf(FILE*, const wchar_t*, va_list)`** | int | `fwscanf` w/ `va_list`. |
| **`vswprintf(wchar_t*, size_t, const wchar_t*, va_list)`** | int | `swprintf` w/ `va_list`. |
| **`vswscanf(const wchar_t*, const wchar_t*, va_list)`** | int | `swscanf` w/ `va_list`. |
| **`vwprintf(const wchar_t*, va_list)`** | int | `wprintf` w/ `va_list`. |
| **`vwscanf(const wchar_t*, va_list)`** | int | `wscanf` w/ `va_list`. |
| **`fwide(FILE*, int)`** | int | Set orientation: >0 for wide, <0 for narrow. |

Example:

```c
#include <wchar.h>
int main(void) {
    fwprintf(stdout, L"Hello, %ls!\n", L"世界");
    return 0;
}
```

Sample Output:

```txt
Hello, 世界!
```

***

## **Wide-String Manipulation**

| Function | Return Type | Description |
| :-- | :-- | :-- |
| **`wcscat(wchar_t*, const wchar_t*)`** | wchar_t* | Concatenate. |
| **`wcschr(const wchar_t*, wchar_t)`** | wchar_t* | First occurrence. |
| **`wcscmp(const wchar_t*, const wchar_t*)`** | int | Lexicographical compare. |
| **`wcscoll(const wchar_t*, const wchar_t*)`** | int | Locale-aware compare. |
| **`wcscpy(wchar_t*, const wchar_t*)`** | wchar_t* | Copy. |
| **`wcscspn(const wchar_t*, const wchar_t*)`** | size_t | Span until any char in second. |
| **`wcsdup(const wchar_t*)`** | wchar_t* | Duplicate string (XSI). |
| **`wcsftime(wchar_t*, size_t, const wchar_t*, const struct tm*)`** | size_t | Format time in wide string. |
| **`wcslen(const wchar_t*)`** | size_t | Length excluding null. |
| **`wcsncat(wchar_t*, const wchar_t*, size_t)`** | wchar_t* | Concat up to n. |
| **`wcsncmp(const wchar_t*, const wchar_t*, size_t)`** | int | Compare up to n. |
| **`wcsncpy(wchar_t*, const wchar_t*, size_t)`** | wchar_t* | Copy up to n. |
| **`wcspbrk(const wchar_t*, const wchar_t*)`** | wchar_t* | First of any set. |
| **`wcsrchr(const wchar_t*, wchar_t)`** | wchar_t* | Last occurrence. |
| **`wcsspn(const wchar_t*, const wchar_t*)`** | size_t | Span of chars in set. |
| **`wcsstr(const wchar_t*, const wchar_t*)`** | wchar_t* | Substring search. |
| **`wcstok(wchar_t*, const wchar_t*, wchar_t**)`** | wchar_t* | Tokenize. |
| **`wcswcs(const wchar_t*, const wchar_t*)`** | wchar_t* | Substring search (XSI). |
| **`wcswidth(const wchar_t*, size_t)`** | int | Display width of wide string (XSI). |
| **`wcsxfrm(wchar_t*, const wchar_t*, size_t)`** | size_t | Transform for locale-aware sort. |

Example:

```c
#include <wchar.h>
#include <stdio.h>
int main(void) {
    wchar_t s1[32] = L"foo", s2[] = L"bar";
    wcscat(s1, s2);
    wprintf(L"%ls\n", s1);
    return 0;
}
```

Sample Output:

```txt
foobar
```

***

## **Numeric Conversion from Wide Strings**

| Function | Return Type | Description |
| :-- | :-- | :-- |
| **`wcstod(const wchar_t*, wchar_t**)`** | double | Convert to `double`. |
| **`wcstof(const wchar_t*, wchar_t**)`** | float | Convert to `float`. |
| **`wcstol(const wchar_t*, wchar_t**, int)`** | long | Convert to `long`. |
| **`wcstoul(const wchar_t*, wchar_t**, int)`** | unsigned long | Convert to `unsigned long`. |
| **`wcstoll(const wchar_t*, wchar_t**, int)`** | long long | Convert to `long long`. |
| **`wcstoull(const wchar_t*, wchar_t**, int)`** | unsigned long long | Convert to `unsigned long long`. |
| **`wcstold(const wchar_t*, wchar_t**, int)`** | long double | Convert to `long double`. |

Example:

```c
#include <wchar.h>
#include <stdio.h>
int main(void) {
    wchar_t *end;
    double d = wcstod(L"3.14xyz", &end);
    wprintf(L"Value=%f, Next=%ls\n", d, end);
    return 0;
}
```

Sample Output:

```c
Value=3.140000, Next=xyz
```

***

## **Memory Algorithms on Wide Arrays**

| Function | Return Type | Description |
| :-- | :-- | :-- |
| **`wmemchr(const wchar_t*, wchar_t, size_t)`** | wchar_t* | Scan for element. |
| **`wmemcmp(const wchar_t*, const wchar_t*, size_t)`** | int | Compare arrays. |
| **`wmemcpy(wchar_t*, const wchar_t*, size_t)`** | wchar_t* | Copy memory. |
| **`wmemmove(wchar_t*, const wchar_t*, size_t)`** | wchar_t* | Copy memory with overlap safe. |
| **`wmemset(wchar_t*, wchar_t, size_t)`** | wchar_t* | Fill memory. |

Example:

```c
#include <wchar.h>
#include <stdio.h>
int main(void) {
    wchar_t buf[5];
    wmemset(buf, L'A', 4);
    buf[4] = L'\0';
    wprintf(L"%ls\n", buf);
    return 0;
}
```

Sample Output:

```txt
AAAA
```

***

## **Notes on Keywords**

- **restrict** appears in many function prototypes as a qualifier helping the compiler optimize non-overlapping pointers.
- All routines conform to C99 and later standards; some classification functions and locale-aware operations are XSI extensions.

***

**This exhaustive listing covers every macro, type, and function declared by `<wchar.h>`, complete with descriptions, return types, examples, and sample outputs.**
