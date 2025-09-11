# **Complete Reference Guide to C float.h Header**

## **Introduction**

The **`float.h`** header file in C contains platform-dependent macros that define the characteristics and limits of floating-point data types. This header provides crucial information about the implementation's floating-point arithmetic capabilities, making programs more portable across different systems.

## **Overview of Floating-Point Representation**

Before diving into the macros, it's essential to understand that floating-point numbers consist of four components:

- **Sign**: Positive or negative indicator
- **Base/Radix**: The number base used (typically 2 for binary systems)
- **Mantissa/Significand**: The precision digits
- **Exponent**: Power to which the base is raised

The value is calculated as: **`± mantissa × base^exponent`**

## **General System Macros**

### 1. **`FLT_RADIX`**

- **Type**: Integer constant
- **Description**: The base (radix) used for all floating-point type representations
- **Minimum Value**: ≥2
- **Typical Value**: 2 (binary system)
- **Usage**: `printf("%d", FLT_RADIX);`
- **Example Output**: `2`

### 2. **`FLT_ROUNDS`**

- **Type**: Integer constant
- **Description**: Specifies the rounding mode for floating-point operations
- **Possible Values**:
  - `-1`: Indeterminate
  - `0`: Toward zero
  - `1`: To nearest (most common)
  - `2`: Toward positive infinity
  - `3`: Toward negative infinity
- **Usage**: `printf("%d", FLT_ROUNDS);`
- **Example Output**: `1`

### 3. **`FLT_EVAL_METHOD`**

- **Type**: Integer constant (C99)
- **Description**: Determines how floating-point expressions are evaluated
- **Possible Values**:
  - `-1`: Undetermined
  - `0`: Evaluate just to the range and precision of the type
  - `1`: Evaluate float and double as double, long double as long double
  - `2`: Evaluate all as long double
- **Usage**: `printf("%d", FLT_EVAL_METHOD);`
- **Example Output**: `0`

### 4. **`DECIMAL_DIG`**

- **Type**: Integer constant (C99)
- **Description**: Minimum number of decimal digits needed to represent all significant digits of long double in round-trip conversions
- **Minimum Value**: ≥10
- **Typical Value**: 21
- **Usage**: `printf("%d", DECIMAL_DIG);`
- **Example Output**: `21`

## **Float Type Macros (FLT_*)**

### 1. **Float Precision and Range Macros**

- **`FLT_DIG`**
  - **Type**: Integer constant
  - **Description**: Number of decimal digits of precision
  - **Minimum Value**: ≥6
  - **Typical Value**: 6
  - **Usage**: `printf("%d", FLT_DIG);`
  - **Example Output**: `6`

- **`FLT_MANT_DIG`**
  - **Type**: Integer constant
  - **Description**: Number of base-FLT_RADIX digits in the mantissa
  - **Typical Value**: 24 (for IEEE 754 binary32)
  - **Usage**: `printf("%d", FLT_MANT_DIG);`
  - **Example Output**: `24`

### 2. **Float Exponent Macros**

- **`FLT_MIN_EXP`**
  - **Type**: Integer constant
  - **Description**: Minimum negative integer exponent (base FLT_RADIX) for normalized float
  - **Typical Value**: -125
  - **Usage**: `printf("%d", FLT_MIN_EXP);`
  - **Example Output**: `-125`
  
- **`FLT_MIN_10_EXP`**
  - **Type**: Integer constant
  - **Description**: Minimum negative integer exponent (base 10) for normalized float
  - **Maximum Value**: ≤-37
  - **Typical Value**: -37
  - **Usage**: `printf("%d", FLT_MIN_10_EXP);`
  - **Example Output**: `-37`

- **`FLT_MAX_EXP`**
  - **Type**: Integer constant
  - **Description**: Maximum positive integer exponent (base FLT_RADIX) for normalized float
  - **Typical Value**: 128
  - **Usage**: `printf("%d", FLT_MAX_EXP);`
  - **Example Output**: `128`

- **`FLT_MAX_10_EXP`**
  - **Type**: Integer constant
  - **Description**: Maximum positive integer exponent (base 10) for normalized float
  - **Minimum Value**: ≥37
  - **Typical Value**: 38
  - **Usage**: `printf("%d", FLT_MAX_10_EXP);`
  - **Example Output**: `38`

### 3. **Float Value Limit Macros**

- **`FLT_MAX`**
  - **Type**: Float constant
  - **Description**: Maximum finite representable float value
  - **Minimum Value**: ≥1E+37
  - **Typical Value**: 3.402823466e+38F
  - **Usage**: `printf("%.10e", FLT_MAX);`
  - **Example Output**: `3.4028234664e+38`

- **`FLT_EPSILON`**
  - **Type**: Float constant
  - **Description**: Difference between 1.0 and the next representable float value
  - **Maximum Value**: ≤1E-5
  - **Typical Value**: 1.192092896e-07F
  - **Usage**: `printf("%.10e", FLT_EPSILON);`
  - **Example Output**: `1.1920928955e-07`

- **`FLT_MIN`**
  - **Type**: Float constant
  - **Description**: Minimum normalized positive float value
  - **Maximum Value**: ≤1E-37
  - **Typical Value**: 1.175494351e-38F
  - **Usage**: `printf("%.10e", FLT_MIN);`
  - **Example Output**: `1.1754943508e-38`

### 4. **C11 Extended Float Macros**

- **`FLT_DECIMAL_DIG`**
  - **Type**: Integer constant (C11)
  - **Description**: Number of decimal digits for float round-trip conversion
  - **Minimum Value**: ≥6
  - **Typical Value**: 9
  - **Usage**: `printf("%d", FLT_DECIMAL_DIG);`
  - **Example Output**: `9`

- **`FLT_TRUE_MIN`**
  - **Type**: Float constant (C11)
  - **Description**: Minimum positive float value, including subnormal numbers
  - **Typical Value**: 1.40129846e-45F
  - **Usage**: `printf("%.10e", FLT_TRUE_MIN);`
  - **Example Output**: `1.4012984643e-45`
  
- **`FLT_HAS_SUBNORM`**
  - **Type**: Integer constant (C11)
  - **Description**: Indicates whether the float type supports subnormal numbers
  - **Possible Values**:
    - `-1`: Indeterminate
    - `0`: Absent (no subnormal support)
    - `1`: Present (subnormal numbers supported)
  - **Usage**: `printf("%d", FLT_HAS_SUBNORM);`
  - **Example Output**: `1`

## **Double Type Macros (DBL_*)**

### 1. **Double Precision and Range Macros**

- **`DBL_DIG`**
  - **Type**: Integer constant
  - **Description**: Number of decimal digits of precision for double
  - **Minimum Value**: ≥10
  - **Typical Value**: 15
  - **Usage**: `printf("%d", DBL_DIG);`
  - **Example Output**: `15`

- **`DBL_MANT_DIG`**
  - **Type**: Integer constant
  - **Description**: Number of base-FLT_RADIX digits in double mantissa
  - **Typical Value**: 53 (for IEEE 754 binary64)
  - **Usage**: `printf("%d", DBL_MANT_DIG);`
  - **Example Output**: `53`

### 2. **Double Exponent Macros**

- **`DBL_MIN_EXP`**
  - **Type**: Integer constant
  - **Description**: Minimum negative integer exponent (base FLT_RADIX) for normalized double
  - **Typical Value**: -1021
  - **Usage**: `printf("%d", DBL_MIN_EXP);`
  - **Example Output**: `-1021`

- **`DBL_MIN_10_EXP`**
  - **Type**: Integer constant
  - **Description**: Minimum negative integer exponent (base 10) for normalized double
  - **Maximum Value**: ≤-37
  - **Typical Value**: -307
  - **Usage**: `printf("%d", DBL_MIN_10_EXP);`
  - **Example Output**: `-307`

- **`DBL_MAX_EXP`**
  - **Type**: Integer constant
  - **Description**: Maximum positive integer exponent (base FLT_RADIX) for normalized double
  - **Typical Value**: 1024
  - **Usage**: `printf("%d", DBL_MAX_EXP);`
  - **Example Output**: `1024`

- **`DBL_MAX_10_EXP`**
  - **Type**: Integer constant
  - **Description**: Maximum positive integer exponent (base 10) for normalized double
  - **Minimum Value**: ≥37
  - **Typical Value**: 308
  - **Usage**: `printf("%d", DBL_MAX_10_EXP);`
  - **Example Output**: `308`

### 3. **Double Value Limit Macros**

- **`DBL_MAX`**
  - **Type**: Double constant
  - **Description**: Maximum finite representable double value
  - **Minimum Value**: ≥1E+37
  - **Typical Value**: 1.7976931348623158e+308
  - **Usage**: `printf("%.10e", DBL_MAX);`
  - **Example Output**: `1.7976931349e+308`

- **`DBL_EPSILON`**
  - **Type**: Double constant
  - **Description**: Difference between 1.0 and the next representable double value
  - **Maximum Value**: ≤1E-9
  - **Typical Value**: 2.2204460492503131e-016
  - **Usage**: `printf("%.10e", DBL_EPSILON);`
  - **Example Output**: `2.2204460493e-16`

- **`DBL_MIN`**
  - **Type**: Double constant
  - **Description**: Minimum normalized positive double value
  - **Maximum Value**: ≤1E-37
  - **Typical Value**: 2.2250738585072014e-308
  - **Usage**: `printf("%.10e", DBL_MIN);`
  - **Example Output**: `2.2250738585e-308`

### 4. **C11 Extended Double Macros**

- **`DBL_DECIMAL_DIG`**
  - **Type**: Integer constant (C11)
  - **Description**: Number of decimal digits for double round-trip conversion
  - **Minimum Value**: ≥10
  - **Typical Value**: 17
  - **Usage**: `printf("%d", DBL_DECIMAL_DIG);`
  - **Example Output**: `17`

- **`DBL_TRUE_MIN`**
  - **Type**: Double constant (C11)
  - **Description**: Minimum positive double value, including subnormal numbers
  - **Typical Value**: 4.9406564584124654e-324
  - **Usage**: `printf("%.10e", DBL_TRUE_MIN);`
  - **Example Output**: `4.9406564584e-324`

- **`DBL_HAS_SUBNORM`**
  - **Type**: Integer constant (C11)
  - **Description**: Indicates whether double type supports subnormal numbers
  - **Possible Values**: Same as FLT_HAS_SUBNORM
  - **Usage**: `printf("%d", DBL_HAS_SUBNORM);`
  - **Example Output**: `1`

## **Long Double Type Macros (LDBL_*)**

### 1. **Long Double Precision and Range Macros**

- **`LDBL_DIG`**
  - **Type**: Integer constant
  - **Description**: Number of decimal digits of precision for long double
  - **Minimum Value**: ≥10
  - **Typical Value**: 18-19 (implementation dependent)
  - **Usage**: `printf("%d", LDBL_DIG);`
  - **Example Output**: `18`

- **`LDBL_MANT_DIG`**
  - **Type**: Integer constant
  - **Description**: Number of base-FLT_RADIX digits in long double mantissa
  - **Typical Value**: 64 (for 80-bit extended precision)
  - **Usage**: `printf("%d", LDBL_MANT_DIG);`
  - **Example Output**: `64`

### 2. **Long Double Exponent Macros**

- **`LDBL_MIN_EXP`**
  - **Type**: Integer constant
  - **Description**: Minimum negative integer exponent (base FLT_RADIX) for normalized long double
  - **Typical Value**: -16381
  - **Usage**: `printf("%d", LDBL_MIN_EXP);`
  - **Example Output**: `-16381`

- **`LDBL_MIN_10_EXP`**
  - **Type**: Integer constant
  - **Description**: Minimum negative integer exponent (base 10) for normalized long double[^6]
  - **Maximum Value**: ≤-37
  - **Typical Value**: -4931
  - **Usage**: `printf("%d", LDBL_MIN_10_EXP);`
  - **Example Output**: `-4931`

- **`LDBL_MAX_EXP`**
  - **Type**: Integer constant
  - **Description**: Maximum positive integer exponent (base FLT_RADIX) for normalized long double[^6]
  - **Typical Value**: 16384
  - **Usage**: `printf("%d", LDBL_MAX_EXP);`
  - **Example Output**: `16384`

- **`LDBL_MAX_10_EXP`**
  - **Type**: Integer constant
  - **Description**: Maximum positive integer exponent (base 10) for normalized long double[^6]
  - **Minimum Value**: ≥37
  - **Typical Value**: 4932
  - **Usage**: `printf("%d", LDBL_MAX_10_EXP);`
  - **Example Output**: `4932`

### 3. **Long Double Value Limit Macros**

- **`LDBL_MAX`**
  - **Type**: Long double constant
  - **Description**: Maximum finite representable long double value
  - **Minimum Value**: ≥1E+37
  - **Typical Value**: 1.18973149535723176502e+4932L
  - **Usage**: `printf("%.10Le", LDBL_MAX);`
  - **Example Output**: `1.1897314954e+4932`

- **`LDBL_EPSILON`**
  - **Type**: Long double constant
  - **Description**: Difference between 1.0 and next representable long double value
  - **Maximum Value**: ≤1E-9
  - **Typical Value**: 1.08420217248550443401e-19L
  - **Usage**: `printf("%.10Le", LDBL_EPSILON);`
  - **Example Output**: `1.0842021725e-19`

- **`LDBL_MIN`**
  - **Type**: Long double constant
  - **Description**: Minimum normalized positive long double value
  - **Maximum Value**: ≤1E-37
  - **Typical Value**: 3.36210314311209350626e-4932L
  - **Usage**: `printf("%.10Le", LDBL_MIN);`
  - **Example Output**: `3.3621031431e-4932`

### 4. **C11 Extended Long Double Macros**

- **`LDBL_DECIMAL_DIG`**
  - **Type**: Integer constant (C11)
  - **Description**: Number of decimal digits for long double round-trip conversion
  - **Minimum Value**: ≥10
  - **Typical Value**: 21
  - **Usage**: `printf("%d", LDBL_DECIMAL_DIG);`
  - **Example Output**: `21`

- **`LDBL_TRUE_MIN`**
  - **Type**: Long double constant (C11)
  - **Description**: Minimum positive long double value, including subnormal numbers
  - **Typical Value**: Implementation dependent
  - **Usage**: `printf("%.10Le", LDBL_TRUE_MIN);`
  - **Example Output**: `3.6451995318e-4951`

- **`LDBL_HAS_SUBNORM`**
  - **Type**: Integer constant (C11)
  - **Description**: Indicates whether long double type supports subnormal numbers
  - **Possible Values**: Same as FLT_HAS_SUBNORM
  - **Usage**: `printf("%d", LDBL_HAS_SUBNORM);`
  - **Example Output**: `1`

## **Practical Examples**

### **Example 1: Basic float.h Usage**

```c
#include <stdio.h>
#include <float.h>

int main() {
    printf("Float precision: %d decimal digits\n", FLT_DIG);
    printf("Float range: %e to %e\n", FLT_MIN, FLT_MAX);
    printf("Float epsilon: %e\n", FLT_EPSILON);
    
    return 0;
}
```

**Expected Output**:

```txt
Float precision: 6 decimal digits
Float range: 1.175494e-38 to 3.402823e+38
Float epsilon: 1.192093e-07
```

### **Example 2: Checking Subnormal Support**

```c
#include <stdio.h>
#include <float.h>

int main() {
    #ifdef FLT_HAS_SUBNORM
    printf("Float subnormal support: ");
    switch(FLT_HAS_SUBNORM) {
        case -1: printf("Indeterminate\n"); break;
        case 0:  printf("Not supported\n"); break;
        case 1:  printf("Supported\n"); break;
    }
    
    #ifdef FLT_TRUE_MIN
    printf("True minimum float: %e\n", FLT_TRUE_MIN);
    #endif
    #endif
    
    return 0;
}
```

### **Example 3: Comprehensive System Information**

```c
#include <stdio.h>
#include <float.h>

int main() {
    printf("=== Floating-Point System Information ===\n");
    printf("Radix: %d\n", FLT_RADIX);
    printf("Rounding mode: %d\n", FLT_ROUNDS);
    
    #ifdef FLT_EVAL_METHOD
    printf("Evaluation method: %d\n", FLT_EVAL_METHOD);
    #endif
    
    #ifdef DECIMAL_DIG
    printf("Decimal digits for long double: %d\n", DECIMAL_DIG);
    #endif
    
    printf("\n=== Type Comparisons ===\n");
    printf("Type        | Digits | Min Exp | Max Exp | Epsilon\n");
    printf("------------|--------|---------|---------|--------\n");
    printf("float       | %6d | %7d | %7d | %e\n", 
           FLT_DIG, FLT_MIN_10_EXP, FLT_MAX_10_EXP, FLT_EPSILON);
    printf("double      | %6d | %7d | %7d | %e\n", 
           DBL_DIG, DBL_MIN_10_EXP, DBL_MAX_10_EXP, DBL_EPSILON);
    printf("long double | %6d | %7d | %7d | %Le\n", 
           LDBL_DIG, LDBL_MIN_10_EXP, LDBL_MAX_10_EXP, LDBL_EPSILON);
           
    return 0;
}
```

### **Example 4: Complete Example**

```c

#include <stdio.h>
#include <float.h>

int main() {
    printf("=== FLOAT.H COMPREHENSIVE DEMONSTRATION ===\n\n");

    // Radix and Rounding
    printf("FLT_RADIX = %d\n", FLT_RADIX);
    printf("FLT_ROUNDS = %d\n", FLT_ROUNDS);
    #ifdef FLT_EVAL_METHOD
    printf("FLT_EVAL_METHOD = %d\n", FLT_EVAL_METHOD);
    #endif
    #ifdef DECIMAL_DIG
    printf("DECIMAL_DIG = %d\n", DECIMAL_DIG);
    #endif

    printf("\n=== FLOAT TYPE MACROS ===\n");
    printf("FLT_DIG = %d\n", FLT_DIG);
    printf("FLT_MANT_DIG = %d\n", FLT_MANT_DIG);
    printf("FLT_MIN_EXP = %d\n", FLT_MIN_EXP);
    printf("FLT_MIN_10_EXP = %d\n", FLT_MIN_10_EXP);
    printf("FLT_MAX_EXP = %d\n", FLT_MAX_EXP);
    printf("FLT_MAX_10_EXP = %d\n", FLT_MAX_10_EXP);
    printf("FLT_MAX = %.10e\n", FLT_MAX);
    printf("FLT_EPSILON = %.10e\n", FLT_EPSILON);
    printf("FLT_MIN = %.10e\n", FLT_MIN);

    // C11 macros for float
    #ifdef FLT_DECIMAL_DIG
    printf("FLT_DECIMAL_DIG = %d\n", FLT_DECIMAL_DIG);
    #endif
    #ifdef FLT_TRUE_MIN
    printf("FLT_TRUE_MIN = %.10e\n", FLT_TRUE_MIN);
    #endif
    #ifdef FLT_HAS_SUBNORM
    printf("FLT_HAS_SUBNORM = %d\n", FLT_HAS_SUBNORM);
    #endif

    printf("\n=== DOUBLE TYPE MACROS ===\n");
    printf("DBL_DIG = %d\n", DBL_DIG);
    printf("DBL_MANT_DIG = %d\n", DBL_MANT_DIG);
    printf("DBL_MIN_EXP = %d\n", DBL_MIN_EXP);
    printf("DBL_MIN_10_EXP = %d\n", DBL_MIN_10_EXP);
    printf("DBL_MAX_EXP = %d\n", DBL_MAX_EXP);
    printf("DBL_MAX_10_EXP = %d\n", DBL_MAX_10_EXP);
    printf("DBL_MAX = %.10e\n", DBL_MAX);
    printf("DBL_EPSILON = %.10e\n", DBL_EPSILON);
    printf("DBL_MIN = %.10e\n", DBL_MIN);

    // C11 macros for double
    #ifdef DBL_DECIMAL_DIG
    printf("DBL_DECIMAL_DIG = %d\n", DBL_DECIMAL_DIG);
    #endif
    #ifdef DBL_TRUE_MIN
    printf("DBL_TRUE_MIN = %.10e\n", DBL_TRUE_MIN);
    #endif
    #ifdef DBL_HAS_SUBNORM
    printf("DBL_HAS_SUBNORM = %d\n", DBL_HAS_SUBNORM);
    #endif

    printf("\n=== LONG DOUBLE TYPE MACROS ===\n");
    printf("LDBL_DIG = %d\n", LDBL_DIG);
    printf("LDBL_MANT_DIG = %d\n", LDBL_MANT_DIG);
    printf("LDBL_MIN_EXP = %d\n", LDBL_MIN_EXP);
    printf("LDBL_MIN_10_EXP = %d\n", LDBL_MIN_10_EXP);
    printf("LDBL_MAX_EXP = %d\n", LDBL_MAX_EXP);
    printf("LDBL_MAX_10_EXP = %d\n", LDBL_MAX_10_EXP);
    printf("LDBL_MAX = %.10Le\n", LDBL_MAX);
    printf("LDBL_EPSILON = %.10Le\n", LDBL_EPSILON);
    printf("LDBL_MIN = %.10Le\n", LDBL_MIN);

    // C11 macros for long double
    #ifdef LDBL_DECIMAL_DIG
    printf("LDBL_DECIMAL_DIG = %d\n", LDBL_DECIMAL_DIG);
    #endif
    #ifdef LDBL_TRUE_MIN
    printf("LDBL_TRUE_MIN = %.10Le\n", LDBL_TRUE_MIN);
    #endif
    #ifdef LDBL_HAS_SUBNORM
    printf("LDBL_HAS_SUBNORM = %d\n", LDBL_HAS_SUBNORM);
    #endif

    return 0;
}
```

**Output**:

```txt
=== FLOAT.H COMPREHENSIVE DEMONSTRATION ===

FLT_RADIX = 2
FLT_ROUNDS = 1
FLT_EVAL_METHOD = 0
DECIMAL_DIG = 21

=== FLOAT TYPE MACROS ===
FLT_DIG = 6
FLT_MANT_DIG = 24
FLT_MIN_EXP = -125
FLT_MIN_10_EXP = -37
FLT_MAX_EXP = 128
FLT_MAX_10_EXP = 38
FLT_MAX = 3.4028234664e+38
FLT_EPSILON = 1.1920928955e-07
FLT_MIN = 1.1754943508e-38
FLT_DECIMAL_DIG = 9
FLT_TRUE_MIN = 1.4012984643e-45
FLT_HAS_SUBNORM = 1

=== DOUBLE TYPE MACROS ===
DBL_DIG = 15
DBL_MANT_DIG = 53
DBL_MIN_EXP = -1021
DBL_MIN_10_EXP = -307
DBL_MAX_EXP = 1024
DBL_MAX_10_EXP = 308
DBL_MAX = 1.7976931349e+308
DBL_EPSILON = 2.2204460493e-16
DBL_MIN = 2.2250738585e-308
DBL_DECIMAL_DIG = 17
DBL_TRUE_MIN = 4.9406564584e-324
DBL_HAS_SUBNORM = 1

=== LONG DOUBLE TYPE MACROS ===
LDBL_DIG = 18
LDBL_MANT_DIG = 64
LDBL_MIN_EXP = -16381
LDBL_MIN_10_EXP = -4931
LDBL_MAX_EXP = 16384
LDBL_MAX_10_EXP = 4932
LDBL_MAX = 7.1289939748e-313
LDBL_EPSILON = 7.1289939748e-313
LDBL_MIN = 7.1289939748e-313
LDBL_DECIMAL_DIG = 21
LDBL_TRUE_MIN = 7.1289939748e-313
LDBL_HAS_SUBNORM = 1
```

## **Standard Compliance and Portability**

### 1. **C90/C99/C11 Differences**

**C90 Macros**: All basic FLT_*, DBL_*, and LDBL_* macros

**C99 Additions**:

- `FLT_EVAL_METHOD`
- `DECIMAL_DIG`

**C11 Additions**:

- `FLT_DECIMAL_DIG`, `DBL_DECIMAL_DIG`, `LDBL_DECIMAL_DIG`
- `FLT_TRUE_MIN`, `DBL_TRUE_MIN`, `LDBL_TRUE_MIN`
- `FLT_HAS_SUBNORM`, `DBL_HAS_SUBNORM`, `LDBL_HAS_SUBNORM`

### 2. **Important Notes**

1. **Implementation Dependency**: Many values are implementation-specific but must meet minimum requirements
2. **IEEE 754 Compliance**: Most modern systems follow IEEE 754 standards, providing predictable values
3. **Subnormal Numbers**: The `*_TRUE_MIN` macros represent the actual smallest positive values, including subnormal numbers
4. **Round-trip Conversion**: The `*_DECIMAL_DIG` macros ensure that floating-point to decimal string and back conversions preserve the original value
5. **Compiler Macros**: Some compilers may define these as compiler-built macros (e.g., `__FLT_MAX__`) rather than in the header file

> The float.h header is essential for writing portable numerical code that needs to understand the limits and characteristics of floating-point arithmetic on different platforms. Always include appropriate feature test macros when using C99 or C11 extensions to ensure compatibility across different compilers and systems.
