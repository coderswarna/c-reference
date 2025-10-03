# **Type-Generic Mathematical Macros in `<tgmath.h>`**

## **Introduction**

The **`<tgmath.h>`** header defines ***type-generic macros*** that dispatch at compile time to the appropriate floating-point or complex math function based on the types of their arguments. This enables writing one invocation (e.g., `sin(x)`) that works seamlessly for `float`, `double`, `long double`, and their complex counterparts.

***

## **Overview of Categories**

1. **Trigonometric functions**
2. **Hyperbolic functions**
3. **Exponential and logarithmic functions**
4. **Power and root functions**
5. **Nearest integer, rounding, and remainder functions**
6. **Floating-point environment and miscellaneous**
7. **Complex-specific operations**

Each macro below selects among the corresponding suffixed functions:

- `foo(x)` → calls `foof(x)` if `x` is `float`, `fool(x)` if `long double`, `fooc(x)` if complex `double`, etc.

***

## **1. Trigonometric Functions**

| Macro | Meaning | Return Type | Example | Sample Output |
| :-- | :-- | :-- | :-- | :-- |
| **`acos(x)`** | Arc cosine | same as `x` | `acos(0.5)` | `1.0471975512` |
| **`asin(x)`** | Arc sine | same as `x` | `asin(0.5)` | `0.5235987756` |
| **`atan(x)`** | Arc tangent | same as `x` | `atan(1.0)` | `0.7853981634` |
| **`atan2(y,x)`** | Two-argument atan | same as arguments’ type | `atan2(1.0,1.0)` | `0.7853981634` |
| **`cos(x)`** | Cosine | same as `x` | `cos(0.5)` | `0.877582562` |
| **`sin(x)`** | Sine | same as `x` | `sin(0.5)` | `0.479425539` |
| **`tan(x)`** | Tangent | same as `x` | `tan(0.5)` | `0.5463024898` |

***

## **2. Hyperbolic Functions**

| Macro | Meaning | Return Type | Example | Sample Output |
| :-- | :-- | :-- | :-- | :-- |
| **`cosh(x)`** | Hyperbolic cos | same as `x` | `cosh(1.0)` | `1.5430806348` |
| **`sinh(x)`** | Hyperbolic sin | same as `x` | `sinh(1.0)` | `1.175201194` |
| **`tanh(x)`** | Hyperbolic tan | same as `x` | `tanh(1.0)` | `0.7615941559` |

***

## **3. Exponential and Logarithmic Functions**

| Macro | Meaning | Return Type | Example | Sample Output |
| :-- | :-- | :-- | :-- | :-- |
| **`exp(x)`** | eˣ | same as `x` | `exp(1.0)` | `2.718281828` |
| **`exp2(x)`** | 2ˣ | same as `x` | `exp2(3.0)` | `8.0` |
| **`expm1(x)`** | eˣ − 1 | same as `x` | `expm1(1.0)` | `1.718281828` |
| **`log(x)`** | Natural logarithm | same as `x` | `log(2.718281828)` | `1.0` |
| **`log10(x)`** | Base-10 logarithm | same as `x` | `log10(100.0)` | `2.0` |
| **`log1p(x)`** | ln(1 + x) | same as `x` | `log1p(0.001)` | `0.0009995003` |
| **`log2(x)`** | Base-2 logarithm | same as `x` | `log2(8.0)` | `3.0` |
| **`logb(x)`** | Exponent of x in scientific notation | same as `x` | `logb(123.0)` | `2.0` |

***

## **4. Power and Root Functions**

| Macro | Meaning | Return Type | Example | Sample Output |
| :-- | :-- | :-- | :-- | :-- |
| **`pow(x,y)`** | xʸ | same as arguments | `pow(2.0,3.0)` | `8.0` |
| **`sqrt(x)`** | Square root | same as `x` | `sqrt(2.0)` | `1.414213562` |
| **`cbrt(x)`** | Cube root | same as `x` | `cbrt(8.0)` | `2.0` |

***

## **5. Nearest Integer, Rounding, and Remainder**

| Macro | Meaning | Return Type | Example | Sample Output |
| :-- | :-- | :-- | :-- | :-- |
| **`ceil(x)`** | Smallest integer ≥ x | same as `x` | `ceil(2.3)` | `3.0` |
| **`floor(x)`** | Largest integer ≤ x | same as `x` | `floor(2.7)` | `2.0` |
| **`fabs(x)`** | Absolute value | same as `x` | `fabs(-2.5)` | `2.5` |
| **`fmod(x,y)`** | Floating-point remainder | same as `x` | `fmod(5.3,2.0)` | `1.3` |
| **`remainder(x,y)`** | IEEE remainder | same as `x` | `remainder(5.3,2.0)` | `−0.7` |
| **`remquo(x,y,quo)`** | Remainder + quotient bits | same as `x`, `quo` is `int*` | `int q; remquo(5.3,2.0,&q)` → `1.3`, q=2 |  |
| **`nearbyint(x)`** | Rounds to nearest integer | same as `x` | `nearbyint(2.5)` | `2.0` |
| **`rint(x)`** | Rounds to nearest integer | same as `x` | `rint(2.5)` | `2.0` |
| **`round(x)`** | Rounds halfway away from zero | same as `x` | `round(2.5)` | `3.0` |
| **`trunc(x)`** | Truncate toward zero | same as `x` | `trunc(2.9)` | `2.0` |
| **`lrint(x)`** | Rounds to long int | `long int` | `lrint(2.5)` | `2` |
| **`llrint(x)`** | Rounds to long long int | `long long int` | `llrint(2.5)` | `2` |
| **`lround(x)`** | Rounds away from zero to `long` | `long int` | `lround(2.5)` | `3` |
| **`llround(x)`** | Rounds away from zero to `llong` | `long long int` | `llround(2.5)` | `3` |

***

## **6. Floating-Point Environment \& Miscellaneous**

| Macro | Meaning | Return Type | Example | Sample Output |
| :-- | :-- | :-- | :-- | :-- |
| **`frexp(x,\&exp)`** | Break into mantissa and exponent | same as `x`, `int*` for exp | `int e; frexp(8.0,&e)` → `0.5`, e=4 |  |
| **`ldexp(x,exp)`** | Compute x × 2^exp | same as `x` | `ldexp(0.5,4)` | `8.0` |
| **`modf(x,\&ip)`** | Break into frac and integer part | same as `x`, `double*` for ip | `double ip; modf(2.3,&ip)` → `0.3`, ip=2.0 |  |
| **`fma(x,y,z)`** | x·y + z in one rounding step | same as `x` | `fma(2.0,3.0,4.0)` | `10.0` |
| **`ilogb(x)`** | Exponent of x as integer | `int` | `ilogb(8.0)` | `3` |
| **`scalbn(x,n)`** | x × 2ⁿ | same as `x` | `scalbn(0.5,4)` | `8.0` |
| **`scalbln(x,n)`** | x × 2ⁿ (n is `long`) | same as `x` | `scalbln(0.5,4L)` | `8.0` |
| **`nextafter(x,y)`** | Next representable toward y | same as `x` | `nextafter(1.0,2.0)` | `1.0000000000000002` |
| **`nexttoward(x,y)`** | Next representable toward y (long double) | same as `x` | `nexttoward(1.0,2.0L)` | `1.0000000000000002` |

***

## **7. Complex-Specific Operations**

| Macro | Meaning | Return Type | Example | Sample Output |
| :-- | :-- | :-- | :-- | :-- |
| **`cabs(z)`** | Complex magnitude | same as `z` | `cabs(1.0+I*1.0)` | `1.414213562` |
| **`carg(z)`** | Complex argument (phase) | same as `z` | `carg(1.0+I*1.0)` | `0.7853981634` |
| **`conj(z)`** | Complex conjugate | same as `z` | `conj(1.0+I*1.0)` | `1.0−I*1.0` |
| **`atan(z)`** | Complex arc tangent | same as `z` | `atan(1.0+I*1.0)` | `1.017221967`−I*0.402359478 |
| **`log(z)`** | Complex natural log | same as `z` | `log(1.0+I*1.0)` | `0.3465735903+I*0.7853981634` |
| **`exp(z)`** | Complex exponential | same as `z` | `exp(1.0+I*1.0)` | `1.4686939399+I*2.2873552872` |

> **Note:** For *every* real‐valued macro above, invoking with complex arguments automatically dispatches to the complex counterpart.

***

Each of these macros is specified by the C11 (and later) standard. They allow writing *one* call regardless of whether arguments are `float`, `double`, `long double`, or complex variants, improving code clarity and maintainability.
