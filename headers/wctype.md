# **Complete C Reference for wctype.h**

Here is an extensive list of functions, keywords, and macros in the C header file **`wctype.h`** with detailed uses, return types, example code, and expected outputs, compiled from authoritative references:

***

## **Macros in wctype.h**

| Macro | Description | Type |
| :-- | :-- | :-- |
| **`WEOF`** | Constant expression of type `wint_t` used to indicate end-of-file or error. Equivalent conceptually to EOF in narrow character functions. | `wint_t` |

***

## **Data Types in wctype.h**

| Type | Description |
| :-- | :-- |
| **`wint_t`** | An integer type capable of storing any valid wide character code or WEOF. |
| **`wctype_t`** | An opaque type representing wide character classification descriptors (character classes). |
| **`wctrans_t`** | An opaque type representing wide character mapping descriptors (character mappings for case conversions and more). |

***

## **Functions for Wide Character Classification and Mapping**

All functions usually have prototypes as below and may also be defined as macros for performance:

```c
#include <wctype.h>
```

| Name | Prototype | Description | Return Type |
| :-- | :-- | :-- | :-- |
| **`iswalnum`** | **`int iswalnum(wint_t wc);`** | Checks if wide character `wc` is alphanumeric (letter or digit). |  Returns nonzero if true, 0 otherwise. |
| **`iswalpha`** | **`int iswalpha(wint_t wc);`** | Checks if `wc` is alphabetic (letter). ||
| **`iswblank`** | **`int iswblank(wint_t wc);`** | Checks if `wc` is a blank character (space or horizontal tab). ||
| **`iswcntrl`** | **`int iswcntrl(wint_t wc);`** | Checks if `wc` is a control character. ||
| **`iswdigit`** | **`int iswdigit(wint_t wc);`** | Checks if `wc` is a decimal digit. ||
| **`iswgraph`** | **`int iswgraph(wint_t wc);`** | Checks if `wc` has graphical representation (any visible character except space). ||
| **`iswlower`** | **`int iswlower(wint_t wc);`** | Checks if `wc` is lowercase letter. ||
| **`iswprint`** | **`int iswprint(wint_t wc);`** | Checks if `wc` is printable (includes space). ||
| **`iswpunct`** | **`int iswpunct(wint_t wc);`** | Checks if `wc` is a punctuation character. ||
| **`iswspace`** | **`int iswspace(wint_t wc);`** | Checks if `wc` is a whitespace character (space, tabs, newline, etc.). ||
| **`iswupper`** | **`int iswupper(wint_t wc);`** | Checks if `wc` is uppercase letter. ||
| **`iswxdigit`** | **`int iswxdigit(wint_t wc);`** | Checks if `wc` is a hexadecimal digit (0-9, a-f, A-F). ||
| **`iswctype`** | **`int iswctype(wint_t wc, wctype_t desc);`** | Checks if the wide character `wc` belongs to the character class described by descriptor `desc`. ||
| **`wctype`** | **`wctype_t wctype(const char *property);`** | Returns a descriptor `wctype_t` for the wide character class named by `property` string (e.g., "alpha", "digit"). ||
| **`towlower`** | **`wint_t towlower(wint_t wc);`** | Converts wide character to lowercase if possible.||
| **`towupper`** | **`wint_t towupper(wint_t wc);`** | Converts wide character to uppercase if possible.||
| **`towctrans`** | **`wint_t towctrans(wint_t wc, wctrans_t desc);`** | Transforms wide character `wc` according to mapping descriptor `desc`. ||
| **`wctrans`** | **`wctrans_t wctrans(const char *property);`** | Returns a mapping descriptor for character mapping named by the `property` string (e.g., "tolower", "toupper").||

***

## **Usage Examples with Output**

### **Example 1: Check if wide character is alphabetic using `iswalpha`**

```c
#include <wchar.h>
#include <wctype.h>
#include <stdio.h>

int main() {
    wchar_t wc = L'A';
    if (iswalpha(wc))
        wprintf(L"%lc is alphabetic.\n", wc);
    else
        wprintf(L"%lc is not alphabetic.\n", wc);
    return 0;
}
```

**Output:**
`A is alphabetic.`

***

### **Example 2: Use `wctype` and `iswctype` to test character classes**

```c
#include <wchar.h>
#include <wctype.h>
#include <stdio.h>

int main() {
    wchar_t wc = L'9';
    wctype_t desc = wctype("digit"); // descriptor for digit class

    if (iswctype(wc, desc))
        wprintf(L"%lc is a digit.\n", wc);
    else
        wprintf(L"%lc is not a digit.\n", wc);
    return 0;
}
```

**Output:**
`9 is a digit.`

***

### **Example 3: Convert to lowercase using `towlower`**

```c
#include <wchar.h>
#include <wctype.h>
#include <stdio.h>

int main() {
    wchar_t wc = L'Z';
    wchar_t lower = towlower(wc);
    wprintf(L"Lowercase of %lc is %lc\n", wc, lower);
    return 0;
}
```

**Output:**
`Lowercase of Z is z`

***

### **Example 4: Check for alphanumeric using loop and `wctype`**

```c
#include <wchar.h>
#include <wctype.h>
#include <stdio.h>

int main() {
    wchar_t str[] = L"Hello123";
    wctype_t desc = wctype("alnum"); // alphanumeric descriptor

    for (int i = 0; str[i] != L'\0'; ++i) {
        if (iswctype(str[i], desc))
            wprintf(L"%lc is alphanumeric.\n", str[i]);
        else
            wprintf(L"%lc is not alphanumeric.\n", str[i]);
    }
    return 0;
}
```

**Output:**

```txt
H is alphanumeric.
e is alphanumeric.
l is alphanumeric.
l is alphanumeric.
o is alphanumeric.
1 is alphanumeric.
2 is alphanumeric.
3 is alphanumeric.
```

***

## **Notes on Behavior and Usage**

- Arguments to these functions are of type `wint_t` and must either represent a valid `wchar_t` value or be equal to the macro `WEOF`. Behavior is undefined for any other values.
- The behavior of these functions is affected by the `LC_CTYPE` category of the current locale.
- Functions may be implemented as macros for performance.
- The classification names accepted by `wctype()` and `wctrans()` include:
`"alnum"`, `"alpha"`, `"blank"`, `"cntrl"`, `"digit"`, `"graph"`, `"lower"`, `"print"`, `"punct"`, `"space"`, `"upper"`, `"xdigit"` for classification and `"tolower"`, `"toupper"` for mappings.
- Wide character I/O functions (like `getwchar()`) return `WEOF` on error or end-of-file, consistent with use of `WEOF` in classification functions.

***

This detailed view covers all functions, macros, keywords, types, usage, and examples related to `<wctype.h>` in C based on POSIX and ISO C standards and trusted references.

***
