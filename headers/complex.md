# C Standard Library `<complex.h>` — Complete Documentation

## Overview

The `complex.h` header, available since C99, enables **complex number arithmetic** in the C programming language. It provides:

- Complex types (float, double, long double, `_Complex`)
- Construction macros and imaginary unit constants
- Arithmetic, trigonometric, exponential, and hyperbolic functions
- A pragma to control implementation details

---

## Table of Contents

- [Macros](#macros)
- [Pragma](#pragma-directive)
- [Complex Types](#complex-types)
- [Function Reference](#function-categories)
  - [Basic Operations](#1-basic-operations)
  - [Exponential Functions](#2-exponential-functions)
  - [Trigonometric Functions](#3-trigonometric-functions)
  - [Inverse Trigonometric Functions](#4-inverse-trigonometric-functions)
  - [Hyperbolic Functions](#5-hyperbolic-functions)
  - [Inverse Hyperbolic Functions](#6-inverse-hyperbolic-functions)
- [Type-Generic Math](#type-generic-math-support)
- [Usage Notes](#usage-notes)

---

## Macros

### 1. Type Definition Macros

**A. `complex`**

- **Expands to**: `_Complex`
- **Usage**: Used to declare complex variables more conveniently
- **Example**: `complex double z = 3.0 + 4.0*I;`

**B. `imaginary`** (rarely supported)

- **Expands to**: `_Imaginary`
- **Usage**: Used for pure imaginary types (only if implementation supports C99 Annex G)
- **Note**: Most implementations don't support this due to mathematical incorrectness of Annex G

### 2. Imaginary Unit Constants

**A. `I`**

- **Expands to**: `_Imaginary_I` if available, otherwise `_Complex_I`
- **Usage**: The primary imaginary unit constant
- **Example**: `double complex z = 3.2 + 4.1*I;`

**B. `_Complex_I`**

- **Type**: `const float _Complex`
- **Value**: Imaginary unit where I² = -1
- **Usage**: Internal constant for complex arithmetic

**C. `_Imaginary_I`** (if supported)

- **Type**: `const float _Imaginary`
- **Value**: Pure imaginary unit
- **Usage**: Used when implementation supports pure imaginary types

### 3. Complex Construction Macros

**A. `CMPLX(x, y)`**

- **Return Type**: `double complex`
- **Description**: Creates a complex number from real and imaginary parts
- **Example**:

```c
#include <complex.h>
#include <stdio.h>

int main() {
    double complex z = CMPLX(3.0, 4.0);
    printf("z = %.1f + %.1fi\n", creal(z), cimag(z));
    return 0;
}
```

- **Output**: `z = 3.0 + 4.0i`

**B. `CMPLXF(x, y)`** and **`CMPLXL(x, y)`**

- **Return Type**: `float complex` and `long double complex` respectively
- **Usage**: Single and extended precision versions of CMPLX
- **Example**:

  - `float complex z = CMPLXF(3.0f, 4.0f);`
  - `long double complex z = CMPLXL(3.0L, 4.0L);`

## Pragma Directive

**`CX_LIMITED_RANGE`**

- **Syntax**: `#pragma STDC CX_LIMITED_RANGE ON/OFF/DEFAULT`
- **Default**: OFF
- **Purpose**: Allows compiler to use conventional complex arithmetic formulas
- **Formulas enabled**:
    1. `(x+iy)×(u+iv) = (xu-yv)+i(yu+xv)`
    2. `(x+iy)/(u+iv) = [(xu+yv)+i(yu-xv)]/(u²+v²)`
    3. `|x+iy| = √(x²+y²)`
- **Warning**: May cause intermediate overflow issues
- **Usage Example:**

```c
#pragma STDC CX_LIMITED_RANGE ON
```

## Complex Types

The header supports three complex number types:

| Type                      | Description                    | Example                  |
|---------------------------|--------------------------------|--------------------------|
| `float _Complex`          | Single precision complex type  | `float _Complex z;`      |
| `double _Complex`         | Double precision complex type  | `double _Complex z;`     |
| `long double _Complex`    | Extended precision complex type| `long double _Complex z;`|

## Function Categories

Each function in `complex.h` comes in three variants (float, double, long double) with suffixes `f`, (none), and `l` respectively.

### 1. Basic Operations

**`cabs, cabsf, cabsl`**

- **Syntax**: `double cabs(double complex z)`
- **Return Type**: Real floating-point type
- **Description**: Computes absolute value (magnitude) of complex number
- **Formula**: `|z| = √(real² + imag²)`
- **Example**:

```c
double complex z = 3.0 + 4.0*I;
double magnitude = cabs(z);  // Returns 5.0
```

**`carg, cargf, cargl`**

- **Syntax**: `double carg(double complex z)`
- **Return Type**: Real floating-point type
- **Description**: Computes phase angle (argument) in radians
- **Formula**: `arg(z) = atan2(imag, real)`
- **Range**: `(-π, π]`
- **Example**:

```c
double complex z = 1.0 + 1.0*I;
double angle = carg(z);  // Returns π/4 ≈ 0.7854
```

**`creal, crealf, creall`**

- **Syntax**: `double creal(double complex z)`
- **Return Type**: Real floating-point type
- **Description**: Extracts real part of complex number
- **Example**:

```c
double complex z = 3.5 - 2.7*I;
double real_part = creal(z);  // Returns 3.5
```

**`cimag, cimagf, cimagl`**

- **Syntax**: `double cimag(double complex z)`
- **Return Type**: Real floating-point type
- **Description**: Extracts imaginary part of complex number
- **Example**:

```c
double complex z = 3.5 - 2.7*I;
double imag_part = cimag(z);  // Returns -2.7
```

**`conj, conjf, conjl`**

- **Syntax**: `double complex conj(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex conjugate
- **Formula**: For `z = a + bi`, returns `a - bi`
- **Example**:

```c
double complex z = 3.0 + 4.0*I;
double complex z_conj = conj(z);  // Returns 3.0 - 4.0*I
```

**`cproj, cprojf, cprojl`**

- **Syntax**: `double complex cproj(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Projects complex number onto Riemann sphere
- **Behavior**: Maps complex infinities to positive infinity on real axis
- **Formula**: For infinite parts: `INFINITY + I * copysign(0.0, cimag(z))`
- **Example**:

```c
double complex z1 = 1.0 + 2.0*I;
double complex proj1 = cproj(z1);  // Returns 1.0 + 2.0*I

double complex z2 = INFINITY + 2.0*I;  
double complex proj2 = cproj(z2);  // Returns inf + 0.0*I
```

### 2. Exponential Functions

**`cexp, cexpf, cexpl`**

- **Syntax**: `double complex cexp(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex exponential `e^z`
- **Formula**: `e^(a+bi) = e^a × (cos(b) + i×sin(b))`
- **Example**:

```c
double complex z = 1.0 + M_PI*I;
double complex result = cexp(z);  // Returns -2.7183 + 0.0000*I
```

**`clog, clogf, clogl`**

- **Syntax**: `double complex clog(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex natural logarithm
- **Formula**: `log(z) = log|z| + i×arg(z)`
- **Branch Cut**: Along negative real axis
- **Example**:

```c
double complex z = 1.0 + 1.0*I;
double complex result = clog(z);  // Returns 0.3466 + 0.7854*I
```

**`cpow, cpowf, cpowl`**

- **Syntax**: `double complex cpow(double complex x, y)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex power `x^y`
- **Formula**: `x^y = exp(y × log(x))`
- **Example**:

```c
double complex result = cpow(2.0 + 0.0*I, 3.0 + 0.0*I);  // Returns 8.0 + 0.0*I
```

**`csqrt, csqrtf, csqrtl`**

- **Syntax**: `double complex csqrt(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex square root
- **Formula**: `√z = √|z| × exp(i×arg(z)/2)`
- **Branch Cut**: Along negative real axis
- **Example**:

```c
double complex z = -1.0 + 0.0*I;
double complex result = csqrt(z);  // Returns 0.0 + 1.0*I
```

### 3. Trigonometric Functions

**`ccos, ccosf, ccosl`**

- **Syntax**: `double complex ccos(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex cosine
- **Formula**: `cos(z) = (e^(iz) + e^(-iz))/2`
- **Identity**: `cos(a+bi) = cos(a)cosh(b) - i×sin(a)sinh(b)`

**`csin, csinf, csinl`**

- **Syntax**: `double complex csin(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex sine
- **Formula**: `sin(z) = (e^(iz) - e^(-iz))/(2i)`
- **Identity**: `sin(a+bi) = sin(a)cosh(b) + i×cos(a)sinh(b)`

**`ctan, ctanf, ctanl`**

- **Syntax**: `double complex ctan(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex tangent
- **Formula**: `tan(z) = sin(z)/cos(z)`
- **Branch Cuts**: Along imaginary axis at `±(π/2 + nπ)i`

### 4. Inverse Trigonometric Functions

**`cacos, cacosf, cacosl`**

- **Syntax**: `double complex cacos(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex arc cosine
- **Formula**: `acos(z) = -i × log(z + i×√(1-z²))`
- **Range**: Real part `[0, π]`, Imaginary part `(-∞, ∞)`
- **Branch Cuts**: `(-∞, -1)` and `(1, ∞)` on real axis

**`casin, casinf, casinl`**

- **Syntax**: `double complex casin(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex arc sine
- **Formula**: `asin(z) = -i × log(i×z + √(1-z²))`
- **Range**: Real part `[-π/2, π/2]`, Imaginary part `(-∞, ∞)`

**`catan, catanf, catanl`**

- **Syntax**: `double complex catan(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex arc tangent
- **Formula**: `atan(z) = (i/2) × log((i+z)/(i-z))`
- **Range**: Real part `[-π/2, π/2]`, Imaginary part `(-∞, ∞)`
- **Branch Cuts**: `(-∞i, -i]` and `

### 5. Hyperbolic Functions

**`ccosh, ccoshf, ccoshl`**

- **Syntax**: `double complex ccosh(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex hyperbolic cosine
- **Formula**: `cosh(z) = (e^z + e^(-z))/2`
- **Identity**: `cosh(a+bi) = cosh(a)cos(b) + i×sinh(a)sin(b)`

**`csinh, csinhf, csinhl`**

- **Syntax**: `double complex csinh(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex hyperbolic sine
- **Formula**: `sinh(z) = (e^z - e^(-z))/2`
- **Identity**: `sinh(a+bi) = sinh(a)cos(b) + i×cosh(a)sin(b)`

**`ctanh, ctanhf, ctanhl`**

- **Syntax**: `double complex ctanh(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex hyperbolic tangent
- **Formula**: `tanh(z) = sinh(z)/cosh(z)`

### 6. Inverse Hyperbolic Functions

**`cacosh, cacoshf, cacoshl`**

- **Syntax**: `double complex cacosh(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex hyperbolic arc cosine
- **Formula**: `acosh(z) = log(z + √(z²-1))`
- **Range**: Real part `[0, ∞)`, Imaginary part `[-π, π]`
- **Branch Cut**: `(-∞, 1)` on real axis

**`casinh, casinhf, casinhl`**

- **Syntax**: `double complex casinh(double complex z)`
- **Return Type**: Complex type matching input
- **Description**: Computes complex hyperbolic arc sine
- **Formula**: `asinh(z) = log(z + √(z²+1))`
- **Range**: Real part `(-∞, ∞)`, Imaginary part `[-π/2, π/2]`

**`catanh, catanhf, catanhl`**

- **Return Type**: Complex type matching input
- **Syntax**: `double complex catanh(double complex z)`
- **Description**: Computes complex hyperbolic arc tangent
- **Formula**: `atanh(z) = (1/2) × log((1+z)/(1-z))`
- **Range**: Real part `(-∞, ∞)`, Imaginary part `[-π/2, π/2]`
- **Branch Cuts**: `(-∞, -1]` and `

## Type-Generic Math Support

When `<tgmath.h>` is included, type-generic macros are available that automatically select the appropriate function variant based on argument types. For example:

- `sqrt(4.0f)` calls `csqrtf()` for float complex
- `sqrt(4.0)` calls `csqrt()` for double complex
- `sqrt(4.0L)` calls `csqrtl()` for long double complex

```c
#include <tgmath.h>

double complex z = 3.0 + 4.0*I;
double magnitude = cabs(z);  // Calls cabs()
float complex zf = 3.0f + 4.0f*I;
float magnitude_f = cabs(zf);  // Calls cabsf()
```

## Usage Notes

### Angular Measurements

All angular values in complex functions are measured in **radians**.

### Branch Cuts

Many complex functions have **branch cuts** - discontinuities where the function value jumps. The sign of zero can indicate which side of the cut you're on in implementations supporting signed zeros.

### Compilation Requirements

- **C99 or later**: Required for complex number support
- **Compiler flags**: Use `-std=c99` or later standards
- **Headers**: Include `<complex.h>` for complex functions
- **Confilcts**: `<complex.h>` does not conflict with C++ `<complex>`, which is a template class.

### Example Complete Program

```c
#include <complex.h>
#include <stdio.h>
#include <math.h>

int main() {
    // Create complex numbers
    double complex z1 = CMPLX(3.0, 4.0);
    double complex z2 = 1.0 + 2.0*I;
    
    // Basic operations
    printf("z1 = %.1f + %.1fi\n", creal(z1), cimag(z1));
    printf("|z1| = %.2f\n", cabs(z1));
    printf("arg(z1) = %.4f rad\n", carg(z1));
    printf("conj(z1) = %.1f + %.1fi\n", creal(conj(z1)), cimag(conj(z1)));
    
    // Arithmetic
    double complex sum = z1 + z2;
    double complex product = z1 * z2;
    printf("z1 + z2 = %.1f + %.1fi\n", creal(sum), cimag(sum));
    printf("z1 * z2 = %.1f + %.1fi\n", creal(product), cimag(product));
    
    // Complex functions
    double complex exp_result = cexp(z2);
    double complex log_result = clog(z1);
    printf("exp(z2) = %.4f + %.4fi\n", creal(exp_result), cimag(exp_result));
    printf("log(z1) = %.4f + %.4fi\n", creal(log_result), cimag(log_result));
    
    return 0;
}
```

This comprehensive reference covers all **66 functions**, **8 macros**, **1 pragma directive**, and **6 data types** available in the `complex.h` header, providing you with complete coverage of C's complex number arithmetic capabilities as introduced in C99 and maintained through current standards including C23.
