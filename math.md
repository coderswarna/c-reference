# Comprehensive Reference Guide: C math.h Functions, Macros, and Keywords

## Overview

The `math.h` header file in C provides a comprehensive collection of mathematical functions, constants, and macros for performing complex mathematical operations. This library is part of the C standard library and contains **63 functions** across **11 categories** plus **23 essential macros and constants**.

## Function Categories and Complete Reference

### Trigonometric Functions

**Basic Trigonometric Functions:**

- **`sin(x)`** - Returns sine of x (x in radians)
- **`cos(x)`** - Returns cosine of x (x in radians)
- **`tan(x)`** - Returns tangent of x (x in radians)

**Inverse Trigonometric Functions:**

- **`asin(x)`** - Returns arcsine of x in radians
- **`acos(x)`** - Returns arccosine of x in radians
- **`atan(x)`** - Returns arctangent of x in radians
- **`atan2(y, x)`** - Returns arctangent of y/x using signs to determine correct quadrant

All trigonometric functions work with **radians**, not degrees.

### Hyperbolic Functions

- **`sinh(x)`** - Returns hyperbolic sine of x
- **`cosh(x)`** - Returns hyperbolic cosine of x
- **`tanh(x)`** - Returns hyperbolic tangent of x
- **`asinh(x)`** - Returns inverse hyperbolic sine of x
- **`acosh(x)`** - Returns inverse hyperbolic cosine of x
- **`atanh(x)`** - Returns inverse hyperbolic tangent of x

### Exponential and Logarithmic Functions

**Exponential Functions:**

- **`exp(x)`** - Returns e raised to power x
- **`exp2(x)`** - Returns 2 raised to power x
- **`expm1(x)`** - Returns e^x - 1 (more accurate for small x)

**Logarithmic Functions:**

- **`log(x)`** - Returns natural logarithm of x
- **`log10(x)`** - Returns base-10 logarithm of x
- **`log2(x)`** - Returns base-2 logarithm of x
- **`log1p(x)`** - Returns ln(1 + x) (more accurate for small x)

### Power Functions

- **`pow(x, y)`** - Returns x raised to power y
- **`sqrt(x)`** - Returns square root of x
- **`cbrt(x)`** - Returns cube root of x
- **`hypot(x, y)`** - Returns sqrt(x² + y²) without intermediate overflow

### Rounding and Remainder Functions

**Rounding Functions:**

- **`ceil(x)`** - Returns smallest integer ≥ x
- **`floor(x)`** - Returns largest integer ≤ x
- **`round(x)`** - Returns nearest integer, halfway cases away from zero
- **`trunc(x)`** - Returns integer part (truncates toward zero)
- **`rint(x)`** - Returns nearest integer using current rounding mode
- **`nearbyint(x)`** - Like rint() but doesn't raise inexact exception

**Integer Conversion Functions:**

- **`lrint(x)`** - Returns nearest integer as long int
- **`llrint(x)`** - Returns nearest integer as long long int
- **`lround(x)`** - Returns nearest integer as long int (halfway away from zero)
- **`llround(x)`** - Returns nearest integer as long long int

**Remainder Functions:**

- **`fmod(x, y)`** - Returns floating-point remainder of x/y
- **`remainder(x, y)`** - Returns IEEE remainder of x/y
- **`remquo(x, y, quo)`** - Returns IEEE remainder and stores quotient

### Floating Point Manipulation Functions

- **`frexp(x, exp)`** - Breaks x into normalized fraction and exponent
- **`ldexp(x, exp)`** - Returns x × 2^exp
- **`modf(x, iptr)`** - Breaks x into fractional and integer parts
- **`scalbn(x, n)`** - Returns x × FLT_RADIX^n efficiently
- **`scalbln(x, n)`** - Like scalbn but with long exponent
- **`ilogb(x)`** - Returns exponent of x as signed integer
- **`logb(x)`** - Returns exponent of x as floating-point

### Maximum, Minimum and Comparison Functions

- **`fmax(x, y)`** - Returns maximum of x and y (NaN-aware)
- **`fmin(x, y)`** - Returns minimum of x and y (NaN-aware)
- **`fdim(x, y)`** - Returns positive difference of x and y
- **`fabs(x)`** - Returns absolute value of x

### Classification Functions

- **`fpclassify(x)`** - Classifies floating-point value (returns FP_NAN, FP_INFINITE, etc.)
- **`isnan(x)`** - Tests if x is NaN
- **`isinf(x)`** - Tests if x is infinite
- **`isfinite(x)`** - Tests if x is finite
- **`isnormal(x)`** - Tests if x is normal (not zero, subnormal, infinite, or NaN)
- **`signbit(x)`** - Tests if sign bit is set

### Error and Gamma Functions

- **`erf(x)`** - Returns error function of x
- **`erfc(x)`** - Returns complementary error function (1 - erf(x))
- **`tgamma(x)`** - Returns gamma function of x
- **`lgamma(x)`** - Returns natural logarithm of absolute value of gamma function

### Next Representable Value Functions

- **`nextafter(x, y)`** - Returns next representable value after x in direction of y
- **`nexttoward(x, y)`** - Like nextafter but with long double y parameter

### Other Utility Functions

- **`copysign(x, y)`** - Returns x with sign of y
- **`nan(tagp)`** - Returns quiet NaN with optional tag
- **`fma(x, y, z)`** - Returns (x × y) + z with single rounding

## Mathematical Constants and Macros

The `math.h` header defines numerous mathematical constants as macros. On many systems, these require defining `_USE_MATH_DEFINES` before including the header:

### Essential Mathematical Constants

| Macro | Description | Approximate Value |
| :-- | :-- | :-- |
| **M_PI** | π (pi) | 3.14159265358979323846 |
| **M_E** | e (Euler's number) | 2.7182818284590452354 |
| **M_PI_2** | π/2 | 1.57079632679489661923 |
| **M_PI_4** | π/4 | 0.78539816339744830962 |
| **M_1_PI** | 1/π | 0.31830988618379067154 |
| **M_2_PI** | 2/π | 0.63661977236758134308 |
| **M_SQRT2** | √2 | 1.41421356237309504880 |
| **M_SQRT1_2** | 1/√2 | 0.70710678118654752440 |

### Logarithmic Constants

| Macro | Description | Value |
| :-- | :-- | :-- |
| **M_LOG2E** | log₂(e) | 1.4426950408889634074 |
| **M_LOG10E** | log₁₀(e) | 0.43429448190325182765 |
| **M_LN2** | ln(2) | 0.69314718055994530942 |
| **M_LN10** | ln(10) | 2.30258509299404568402 |
| **M_2_SQRTPI** | 2/√π | 1.12837916709551257390 |

### Special Values and Error Handling

| Macro | Description | Usage |
| :-- | :-- | :-- |
| **HUGE_VAL** | Positive infinity (double) | Error return value |
| **HUGE_VALF** | Positive infinity (float) | C99 standard |
| **HUGE_VALL** | Positive infinity (long double) | C99 standard |
| **INFINITY** | Positive infinity | C99 standard |
| **NAN** | Quiet NaN (Not a Number) | C99 standard |

### Classification Macros

Used with `fpclassify()` function:

| Macro | Description |
| :-- | :-- |
| **FP_NAN** | Classification for NaN values |
| **FP_INFINITE** | Classification for infinite values |
| **FP_ZERO** | Classification for zero values |
| **FP_SUBNORMAL** | Classification for subnormal values |
| **FP_NORMAL** | Classification for normal values |

## Function Variants by Data Type

Most math.h functions have three variants for different floating-point types:

- **Standard functions** (e.g., `sin`) - work with `double`
- **'f' suffix functions** (e.g., `sinf`) - work with `float`
- **'l' suffix functions** (e.g., `sinl`) - work with `long double`

## Key Programming Considerations

### Compilation Requirements

To use math.h functions, you must:

1. Include the header: `#include <math.h>`
2. Link with the math library: `gcc program.c -lm`

### Error Handling

Math functions can return special values:

- **HUGE_VAL** (or ±∞) for overflow
- **NaN** for invalid operations (e.g., `sqrt(-1)`)
- Set `errno` to **EDOM** (domain error) or **ERANGE** (range error)

### Precision Considerations

- Functions like `log1p()` and `expm1()` provide better accuracy for small values
- IEEE 754 standard compliance ensures consistent behavior across platforms
- All functions typically accept and return `double` precision values

## Comprehensive Example Programs

This comprehensive reference covers all standard C math.h functions, macros, and constants with their complete specifications, usage examples, and practical considerations for mathematical programming in C
