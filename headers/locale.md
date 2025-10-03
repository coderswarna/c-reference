# Complete Reference Guide to C locale.h Header

## **Introduction to locale.h**

The `locale.h` header provides functions, macros, and data structures for locale-specific behavior in C programs. Locales allow programs to adapt to different cultural conventions for formatting numbers, currency, dates, and text collation. This header is essential for internationalization (i18n) of C applications, enabling software to handle multiple languages and regional preferences appropriately.

Key capabilities provided by `locale.h` include:

- Setting or querying the program's current locale
- Accessing locale-specific formatting information
- Controlling character classification and conversion behavior
- Managing currency and numeric display formats
- Supporting different collation (sorting) orders for text

## **Functions**

### **1. setlocale()**

**Syntax:** `char *setlocale(int category, const char *locale)`

**Parameters:**

- `int category`: Specifies which locale category to set/query `(LC_ALL, LC_COLLATE, etc.)`
- `const char *locale`: Locale string to set, or NULL to query

**Return Type:** `char *`

**Return Value:** Pointer to string representing the current locale on success, NULL on failure

**Description:** Sets or queries the program's current locale settings. This function changes locale-specific behavior for various C library functions

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

```txt
Current locale: C
C locale: C
US locale: en_US.UTF-8
```

### **2. localeconv()**

**Syntax:** `struct lconv *localeconv(void)`

**Parameters:** None (void)

**Return Type:** `struct lconv *`

**Return Value:** Pointer to lconv structure containing formatting information for current locale

**Description:** Returns a structure containing complete information about locale-specific numeric and monetary formatting rules

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

```txt
Decimal point: '.'
Thousands separator: ''
Currency symbol: ''
International currency: ''
Fractional digits: 255
```

## **Macros**

### **A. LC_ALL**

**Syntax:** `#define LC_ALL <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects all locale categories - sets the entire locale of the program

**Usage:** Used with setlocale() to set all categories simultaneously

**Example:**

```c
setlocale(LC_ALL, "en_US.UTF-8");  // Sets all categories
```

### **B. LC_COLLATE**

**Syntax:** `#define LC_COLLATE <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects string collation functions `strcoll()` and `strxfrm()`

**Usage:** Controls string comparison and sorting behavior

**Example:**

```c
setlocale(LC_COLLATE, "de_DE");  // German collation rules
```

### **C. LC_CTYPE**

**Syntax:** `#define LC_CTYPE <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects character classification and conversion functions

**Usage:** Controls behavior of functions like `isalpha()`, `tolower()`, `toupper()`

**Example:**

```c
setlocale(LC_CTYPE, "fr_FR");  // French character classification
```

### **D. LC_MONETARY**

**Syntax:** `#define LC_MONETARY <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects monetary information returned by `localeconv()`

**Usage:** Controls currency formatting and monetary display

**Example:**

```c
setlocale(LC_MONETARY, "en_GB");  // British currency formatting
```

### **E. LC_NUMERIC**

**Syntax:** `#define LC_NUMERIC <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects decimal-point character and numeric formatting information

**Usage:** Controls numeric display in `printf()`, `scanf()`, and `localeconv()`

**Example:**

```c
setlocale(LC_NUMERIC, "de_DE");  // German numeric format (comma decimal)
```

### **F. LC_TIME**

**Syntax:** `#define LC_TIME <integer constant>`

**Value:** Implementation-defined integer constant

**Description:** Affects time and date formatting with `strftime()`

**Usage:** Controls date/time display format

**Example:**

```c
setlocale(LC_TIME, "ja_JP");  // Japanese date/time formatting
```

### **G.NULL**

**Syntax:** `#define NULL <null pointer constant>`

**Value:** Either `0`, `0L`, or `(void *)0`

**Description:** Null pointer constant defined in multiple headers including locale.h

**Usage:** Used to represent null pointers and query current locale settings

**Example:**

```c
char *current = setlocale(LC_ALL, NULL);  // Query without changing
```

## **Structure: struct lconv**

The `struct lconv` contains at least the following members for numeric and monetary formatting:

### **1. Numeric (Non-monetary) Members**

- **decimal_point**

  - **Type:** `char *`
  - **Description:** Decimal point character for non-monetary values
  - **Default in C locale:** `"."`
  - **Example:** `"."` in C/US locale, `","` in German locale

- **thousands_sep**

  - **Type:** `char *`
  - **Description:** Character used to separate groups of digits to the left of decimal point
  - **Default in C locale:** `""` (empty string)
  - **Example:** `","` in US locale for numbers like `"1,000"`

- **grouping**

  - **Type:** `char *`
  - **Description:** String indicating size of each digit group in non-monetary quantities
  - **Default in C locale:** `""` (empty string)
  - **Values:** Each character represents group size; `CHAR_MAX` means no further grouping; `0` means repeat previous element

### **2. Monetary Members**

- **int_curr_symbol**

  - **Type:** `char *`
  - **Description:** International currency symbol (ISO 4217) plus separator character
  - **Default in C locale:** `""` (empty string)
  - **Example:** `"USD "` for US Dollar, `"EUR "` for Euro

- **currency_symbol**

  - **Type:** `char *`
  - **Description:** Local currency symbol for current locale
  - **Default in C locale:** `""` (empty string)
  - **Example:** `"$"` for US Dollar, `"€"` for Euro

- **mon_decimal_point**

  - **Type:** `char *`
  - **Description:** Decimal point character used in monetary values
  - **Default in C locale:** `""` (empty string)
  - **Example:** `"."` in most locales, `","` in some European locales

- **mon_thousands_sep**

  - **Type:** `char *`
  - **Description:** Thousands separator character for monetary values
  - **Default in C locale:** `""` (empty string)
  - **Example:** `","` in US locale, `" "` (space) in some locales

- **mon_grouping**

  - **Type:** `char *`
  - **Description:** String indicating size of digit groups in monetary quantities
  - **Default in C locale:** `""` (empty string)
  - **Usage:** Same format as `grouping` member

- **positive_sign**

  - **Type:** `char *`
  - **Description:** String used to indicate positive monetary values
  - **Default in C locale:** `""` (empty string)
  - **Example:** Usually empty, `"+"` in some locales

- **negative_sign**

  - **Type:** `char *`
  - **Description:** String used to indicate negative monetary values
  - **Default in C locale:** `""` (empty string)
  - **Example:** `"-"` in most locales

### **3. Formatting Control Members**

- **int_frac_digits**

  - **Type:** `char`
  - **Description:** Number of fractional digits in international monetary formatting
  - **Default in C locale:** `CHAR_MAX`
  - **Example:** `2` for most currencies (cents), `0` for yen

- **frac_digits**

  - **Type:** `char`
  - **Description:** Number of fractional digits in local monetary formatting
  - **Default in C locale:** `CHAR_MAX`
  - **Example:** `2` for dollars (cents), `0` for yen

- **p_cs_precedes**

  - **Type:** `char`
  - **Description:** `1` if currency symbol precedes positive values, `0` if it follows
  - **Default in C locale:** `CHAR_MAX`
  - **Example:** `1` for `"$100"`, `0` for `"100$"`

- **p_sep_by_space**

  - **Type:** `char`
  - **Description:** `1` if currency symbol is separated by space from positive values
  - **Default in C locale:** `CHAR_MAX`
  - **Values:** `0` = no space, `1` = space between, `2` = space separates symbol and sign

- **n_cs_precedes**

  - **Type:** `char`
  - **Description:** `1` if currency symbol precedes negative values, `0` if it follows
  - **Default in C locale:** `CHAR_MAX`
  - **Example:** `1` for `"-$100"`, `0` for `"-100$"`

- **n_sep_by_space**

  - **Type:** `char`
  - **Description:** `1` if currency symbol is separated by space from negative values
  - **Default in C locale:** `CHAR_MAX`
  - **Values:** Same as `p_sep_by_space`

- **p_sign_posn**

  - **Type:** `char`
  - **Description:** Position of positive sign in monetary values
  - **Default in C locale:** `CHAR_MAX`
  - **Values:**

    - `0`: Parentheses encapsulate value and currency symbol
    - `1`: Sign precedes value and currency symbol
    - `2`: Sign succeeds value and currency symbol
    - `3`: Sign immediately precedes currency symbol
    - `4`: Sign immediately succeeds currency symbol

- **n_sign_posn**

  - **Type:** `char`
  - **Description:** Position of negative sign in monetary values
  - **Default in C locale:** `CHAR_MAX`
  - **Values:** Same as `p_sign_posn`

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

1. **Thread Safety**: `setlocale()` is not thread-safe on many implementations
2. **Default Locale**: Programs start with the equivalent of `setlocale(LC_ALL, "C")`
3. **Environment Variables**: When locale is set to empty string `""`, the system uses environment variables like `LANG`, `LC_ALL`, etc.
4. **CHAR_MAX Values**: In the lconv structure, `CHAR_MAX` values indicate that the information is not available in the current locale
5. **Return Value**: The string returned by `setlocale()` can be used in subsequent calls to restore the locale

This comprehensive reference covers all standard components of locale.h as specified in the C standard, including functions, macros, and the complete lconv structure with all its members.
