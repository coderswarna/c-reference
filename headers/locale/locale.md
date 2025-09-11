<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Complete Reference Guide to C locale.h Header

Based on my extensive research of C standard documentation and references, here is the comprehensive list of all functions, macros, keywords, and structures in the **locale.h** header file.

## **Functions**

### **setlocale()**

**Syntax:** `char *setlocale(int category, const char *locale)`

**Parameters:**

- `int category`: Specifies which locale category to set/query
- `const char *locale`: Locale string to set, or NULL to query

**Return Type:** `char *`

**Return Value:** Pointer to string representing the current locale on success, NULL on failure[^1][^2][^3]

**Description:** Sets or queries the program's current locale settings. This function changes locale-specific behavior for various C library functions[^4][^5]

**Usage Example:**

```c
#include <locale.h>
#include <stdio.h>

int main() {
    // Query current locale
    char *current = setlocale(LC_ALL, NULL);
    printf("Current locale: %s\n", current);
    
    // Set to C locale
    char *c_locale = setlocale(LC_ALL, "C");
    printf("C locale: %s\n", c_locale);
    
    // Set to US English
    char *us_locale = setlocale(LC_ALL, "en_US.UTF-8");
    printf("US locale: %s\n", us_locale ? us_locale : "Failed");
    
    return 0;
}
```

**Expected Output:**

```
Current locale: C
C locale: C
US locale: en_US.UTF-8
```


### **localeconv()**

**Syntax:** `struct lconv *localeconv(void)`

**Parameters:** None (void)

**Return Type:** `struct lconv *`

**Return Value:** Pointer to lconv structure containing formatting information for current locale[^6][^7][^8]

**Description:** Returns a structure containing complete information about locale-specific numeric and monetary formatting rules[^9]

**Usage Example:**

```c
#include <locale.h>
#include <stdio.h>

int main() {
    setlocale(LC_ALL, "C");
    struct lconv *lc = localeconv();
    
    printf("Decimal point: '%s'\n", lc->decimal_point);
    printf("Thousands separator: '%s'\n", lc->thousands_sep);
    printf("Currency symbol: '%s'\n", lc->currency_symbol);
    printf("International currency: '%s'\n", lc->int_curr_symbol);
    printf("Fractional digits: %d\n", lc->frac_digits);
    
    return 0;
}
```

**Expected Output:**

```
Decimal point: '.'
Thousands separator: ''
Currency symbol: ''
International currency: ''
Fractional digits: 255
```


## **Macros**

### **LC_ALL**

**Syntax:** `#define LC_ALL <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects all locale categories - sets the entire locale of the program[^10][^11][^12]

**Usage:** Used with setlocale() to set all categories simultaneously

**Example:**

```c
setlocale(LC_ALL, "en_US.UTF-8");  // Sets all categories
```


### **LC_COLLATE**

**Syntax:** `#define LC_COLLATE <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects string collation functions `strcoll()` and `strxfrm()`[^13][^12][^10]

**Usage:** Controls string comparison and sorting behavior

**Example:**

```c
setlocale(LC_COLLATE, "de_DE");  // German collation rules
```


### **LC_CTYPE**

**Syntax:** `#define LC_CTYPE <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects character classification and conversion functions[^12][^10][^13]

**Usage:** Controls behavior of functions like `isalpha()`, `tolower()`, `toupper()`

**Example:**

```c
setlocale(LC_CTYPE, "fr_FR");  // French character classification
```


### **LC_MONETARY**

**Syntax:** `#define LC_MONETARY <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects monetary information returned by `localeconv()`[^10][^13][^12]

**Usage:** Controls currency formatting and monetary display

**Example:**

```c
setlocale(LC_MONETARY, "en_GB");  // British currency formatting
```


### **LC_NUMERIC**

**Syntax:** `#define LC_NUMERIC <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects decimal-point character and numeric formatting information[^13][^12][^10]

**Usage:** Controls numeric display in `printf()`, `scanf()`, and `localeconv()`

**Example:**

```c
setlocale(LC_NUMERIC, "de_DE");  // German numeric format (comma decimal)
```


### **LC_TIME**

**Syntax:** `#define LC_TIME <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects time and date formatting with `strftime()`[^12][^10][^13]

**Usage:** Controls date/time display format

**Example:**

```c
setlocale(LC_TIME, "ja_JP");  // Japanese date/time formatting
```


### **NULL**

**Syntax:** `#define NULL <null pointer constant>`

**Value:** Either 0, 0L, or (void *)0[^14][^10]

**Description:** Null pointer constant defined in multiple headers including locale.h[^14]

**Usage:** Used to represent null pointers and query current locale settings

**Example:**

```c
char *current = setlocale(LC_ALL, NULL);  // Query without changing
```


## **Structure: struct lconv**

The `struct lconv` contains at least the following members for numeric and monetary formatting:[^15][^16][^13]

### **Numeric (Non-monetary) Members**

#### **decimal_point**

**Type:** `char *`
**Description:** Decimal point character for non-monetary values[^9][^15][^13]
**Default in C locale:** `"."`
**Example:** `"."` in C/US locale, `","` in German locale

#### **thousands_sep**

**Type:** `char *`
**Description:** Character used to separate groups of digits to the left of decimal point[^15][^13][^9]
**Default in C locale:** `""` (empty string)
**Example:** `","` in US locale for numbers like `"1,000"`

#### **grouping**

**Type:** `char *`
**Description:** String indicating size of each digit group in non-monetary quantities[^13][^9][^15]
**Default in C locale:** `""` (empty string)
**Values:** Each character represents group size; `CHAR_MAX` means no further grouping; `0` means repeat previous element

### **Monetary Members**

#### **int_curr_symbol**

**Type:** `char *`
**Description:** International currency symbol (ISO 4217) plus separator character[^9][^15][^13]
**Default in C locale:** `""` (empty string)
**Example:** `"USD "` for US Dollar, `"EUR "` for Euro

#### **currency_symbol**

**Type:** `char *`
**Description:** Local currency symbol for current locale[^15][^13][^9]
**Default in C locale:** `""` (empty string)
**Example:** `"$"` for US Dollar, `"€"` for Euro

#### **mon_decimal_point**

**Type:** `char *`
**Description:** Decimal point character used in monetary values[^13][^9][^15]
**Default in C locale:** `""` (empty string)
**Example:** `"."` in most locales, `","` in some European locales

#### **mon_thousands_sep**

**Type:** `char *`
**Description:** Thousands separator character for monetary values[^9][^15][^13]
**Default in C locale:** `""` (empty string)
**Example:** `","` in US locale, `" "` (space) in some locales

#### **mon_grouping**

**Type:** `char *`
**Description:** String indicating size of digit groups in monetary quantities[^15][^13][^9]
**Default in C locale:** `""` (empty string)
**Usage:** Same format as `grouping` member

#### **positive_sign**

**Type:** `char *`
**Description:** String used to indicate positive monetary values[^13][^9][^15]
**Default in C locale:** `""` (empty string)
**Example:** Usually empty, `"+"` in some locales

#### **negative_sign**

**Type:** `char *`
**Description:** String used to indicate negative monetary values[^9][^15][^13]
**Default in C locale:** `""` (empty string)
**Example:** `"-"` in most locales

### **Formatting Control Members**

#### **int_frac_digits**

**Type:** `char`
**Description:** Number of fractional digits in international monetary formatting[^15][^13][^9]
**Default in C locale:** `CHAR_MAX`
**Example:** `2` for most currencies (cents), `0` for yen

#### **frac_digits**

**Type:** `char`
**Description:** Number of fractional digits in local monetary formatting[^13][^9][^15]
**Default in C locale:** `CHAR_MAX`
**Example:** `2` for dollars (cents), `0` for yen

#### **p_cs_precedes**

**Type:** `char`
**Description:** `1` if currency symbol precedes positive values, `0` if it follows[^9][^15][^13]
**Default in C locale:** `CHAR_MAX`
**Example:** `1` for `"$100"`, `0` for `"100$"`

#### **p_sep_by_space**

**Type:** `char`
**Description:** `1` if currency symbol is separated by space from positive values[^15][^13][^9]
**Default in C locale:** `CHAR_MAX`
**Values:** `0` = no space, `1` = space between, `2` = space separates symbol and sign

#### **n_cs_precedes**

**Type:** `char`
**Description:** `1` if currency symbol precedes negative values, `0` if it follows[^13][^9][^15]
**Default in C locale:** `CHAR_MAX`
**Example:** `1` for `"-$100"`, `0` for `"-100$"`

#### **n_sep_by_space**

**Type:** `char`
**Description:** `1` if currency symbol is separated by space from negative values[^9][^15][^13]
**Default in C locale:** `CHAR_MAX`
**Values:** Same as `p_sep_by_space`

#### **p_sign_posn**

**Type:** `char`
**Description:** Position of positive sign in monetary values[^15][^13][^9]
**Default in C locale:** `CHAR_MAX`
**Values:**

- `0`: Parentheses encapsulate value and currency symbol
- `1`: Sign precedes value and currency symbol
- `2`: Sign succeeds value and currency symbol
- `3`: Sign immediately precedes currency symbol
- `4`: Sign immediately succeeds currency symbol


#### **n_sign_posn**

**Type:** `char`
**Description:** Position of negative sign in monetary values[^13][^9][^15]
**Default in C locale:** `CHAR_MAX`
**Values:** Same as `p_sign_posn`

## **Complete Example Program**

Here's a comprehensive example demonstrating all locale.h functionality:

```c
#include <locale.h>
#include <stdio.h>
#include <limits.h>

int main() {
    printf("=== setlocale() Function Examples ===\n");
    
    // Query current locale
    char *current_locale = setlocale(LC_ALL, NULL);
    printf("Current locale: %s\n", current_locale ? current_locale : "NULL");
    
    // Set locale to C (default)
    char *c_locale = setlocale(LC_ALL, "C");
    printf("Set to C locale: %s\n", c_locale ? c_locale : "Failed");
    
    // Try to set different locales
    char *us_locale = setlocale(LC_ALL, "en_US.UTF-8");
    printf("Set to US locale: %s\n", us_locale ? us_locale : "Failed");
    
    // Set specific categories
    char *numeric = setlocale(LC_NUMERIC, "C");
    printf("LC_NUMERIC set to C: %s\n", numeric ? numeric : "Failed");
    
    printf("\n=== localeconv() Function Examples ===\n");
    
    // Get locale formatting information
    struct lconv *lc = localeconv();
    
    printf("Decimal point: '%s'\n", lc->decimal_point);
    printf("Thousands separator: '%s'\n", lc->thousands_sep);
    printf("Currency symbol: '%s'\n", lc->currency_symbol);
    printf("International currency symbol: '%s'\n", lc->int_curr_symbol);
    printf("Monetary decimal point: '%s'\n", lc->mon_decimal_point);
    printf("Positive sign: '%s'\n", lc->positive_sign);
    printf("Negative sign: '%s'\n", lc->negative_sign);
    
    // Display numeric values (note CHAR_MAX means not available)
    printf("Fractional digits: %d%s\n", 
           lc->frac_digits, 
           (lc->frac_digits == CHAR_MAX) ? " (not available)" : "");
    printf("International fractional digits: %d%s\n", 
           lc->int_frac_digits,
           (lc->int_frac_digits == CHAR_MAX) ? " (not available)" : "");
    
    // Display formatting flags
    printf("Currency precedes positive: %d%s\n", 
           lc->p_cs_precedes,
           (lc->p_cs_precedes == CHAR_MAX) ? " (not available)" : "");
    printf("Space separates currency from positive: %d%s\n", 
           lc->p_sep_by_space,
           (lc->p_sep_by_space == CHAR_MAX) ? " (not available)" : "");
    printf("Positive sign position: %d%s\n", 
           lc->p_sign_posn,
           (lc->p_sign_posn == CHAR_MAX) ? " (not available)" : "");
    
    return 0;
}
```


## **Key Notes**

1. **Thread Safety**: `setlocale()` is not thread-safe on many implementations[^2]
2. **Default Locale**: Programs start with the equivalent of `setlocale(LC_ALL, "C")`[^17][^5]
3. **Environment Variables**: When locale is set to empty string `""`, the system uses environment variables like `LANG`, `LC_ALL`, etc.[^5][^2]
4. **CHAR_MAX Values**: In the lconv structure, `CHAR_MAX` values indicate that the information is not available in the current locale[^12][^13]
5. **Return Value**: The string returned by `setlocale()` can be used in subsequent calls to restore the locale[^17][^5]

This comprehensive reference covers all standard components of locale.h as specified in the C standard, including functions, macros, and the complete lconv structure with all its members.[^18][^19][^20][^11][^1][^6][^2][^10][^12][^9][^15][^13]
<span style="display:none">[^21][^22][^23][^24][^25][^26][^27][^28][^29][^30][^31][^32][^33][^34][^35][^36][^37][^38][^39][^40][^41][^42][^43][^44][^45][^46][^47][^48][^49][^50][^51]</span>

<div style="text-align: center">⁂</div>

[^1]: https://en.wikibooks.org/wiki/C_Programming/locale.h

[^2]: https://www.ibm.com/docs/en/i/7.4.0?topic=functions-setlocale-set-locale

[^3]: https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/setlocale-wsetlocale?view=msvc-170

[^4]: https://www.tutorialspoint.com/c_standard_library/c_function_setlocale.htm

[^5]: https://www.ibm.com/docs/en/zos/2.4.0?topic=functions-setlocale-set-locale

[^6]: https://www.tutorialspoint.com/c_standard_library/c_function_localeconv.htm

[^7]: https://en.cppreference.com/w/c/locale/localeconv

[^8]: https://pubs.opengroup.org/onlinepubs/009695199/functions/localeconv.html

[^9]: https://www.oreilly.com/library/view/c-in-a/0596006977/re153.html

[^10]: https://www.qnx.com/developers/docs/6.4.1/dinkum_en/c99/locale.html

[^11]: https://pubs.opengroup.org/onlinepubs/7908799/xsh/locale.h.html

[^12]: https://publications.gbdirect.co.uk/c_book/chapter9/localization.html

[^13]: https://www.ibm.com/docs/en/zvm/7.3?topic=descriptions-localeh

[^14]: https://stackoverflow.com/questions/12023476/what-header-defines-null-in-c

[^15]: https://www.tutorialspoint.com/c_standard_library/c_standard_library_quick_guide.htm

[^16]: https://en.cppreference.com/w/c/locale/lconv

[^17]: https://www.cs.auckland.ac.nz/references/unix/digital/AQTLTBTE/DOCU_090.HTM

[^18]: https://www.ibm.com/docs/en/i/7.5.0?topic=extensions-standard-c-library-functions-table-by-name

[^19]: https://pubs.opengroup.org/onlinepubs/9699919799.2008edition/basedefs/locale.h.html

[^20]: https://cplusplus.com/reference/clibrary/

[^21]: https://www.qnx.com/developers/docs/6.4.1/dinkum_en/ecpp/locale.html

[^22]: https://devdocs.io/c/

[^23]: https://cplusplus.com/reference/clocale/

[^24]: https://www.gnu.org/software/libc/manual/html_node/Setting-the-Locale.html

[^25]: https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/localeconv?view=msvc-170

[^26]: https://www.tutorialspoint.com/c_standard_library/locale_h.htm

[^27]: https://en.wikipedia.org/wiki/C_standard_library

[^28]: https://www.geeksforgeeks.org/c/header-files-in-c-cpp-and-its-uses/

[^29]: https://heycoach.in/blog/character-sets-and-locales-in-c-using-locale-h/

[^30]: https://unstop.com/blog/library-functions-in-c

[^31]: https://www.w3schools.com/php/func_string_localeconv.asp

[^32]: https://developer.arm.com/documentation/dui0808/e/the-c-and-c---library-functions-reference/lconv-structure

[^33]: https://www.tutorialspoint.com/c_standard_library/c_macro_null.htm

[^34]: https://en.cppreference.com/w/c/locale/LC_categories

[^35]: https://pubs.opengroup.org/onlinepubs/009695399/basedefs/locale.h.html

[^36]: https://stackoverflow.com/questions/57232936/which-c-standard-header-file-defines-null-character

[^37]: https://www.ibm.com/docs/en/i/7.5?topic=files-localeh

[^38]: https://en.cppreference.com/w/c/header/locale.html

[^39]: https://man7.org/linux/man-pages/man0/locale.h.0p.html

[^40]: https://www.qnx.com/developers/docs/8.0/com.qnx.doc.neutrino.lib_ref/topic/l/localeconv.html

[^41]: https://learn.microsoft.com/en-us/cpp/standard-library/locale-functions?view=msvc-170

[^42]: https://www.chem.kuleuven.be/peeters/manuals/pgi/doc/pgC++_lib/stdlibug/sta_9169.htm

[^43]: https://thephd.dev/c23-is-coming-here-is-what-is-on-the-menu

[^44]: https://en.wikipedia.org/wiki/C_localization_functions

[^45]: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3088.pdf

[^46]: https://www.simplilearn.com/tutorials/c-tutorial/function-in-c-programming

[^47]: https://en.cppreference.com/w/c/header/locale

[^48]: https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions

[^49]: https://www.geeksforgeeks.org/cpp/clocale-header-file-in-c/

[^50]: https://stackoverflow.com/questions/330793/how-to-initialize-a-struct-in-accordance-with-c-programming-language-standards

[^51]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/af94e876def51aed20d013ae05274618/62a3116b-1766-4992-b2ef-776b44f83e31/cbe6440d.csv

