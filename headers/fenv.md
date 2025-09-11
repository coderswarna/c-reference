# Complete `fenv.h` reference

## Introduction

The C standard library `<fenv.h>` header provides comprehensive support for managing and querying the **floating-point environment**—covering exception flags, rounding modes, and their manipulation. Below is an extensive and meticulous list of **functions, types, macros, and examples** associated with `<fenv.h>`, focusing on accuracy and completeness for modern (C99 and later, including C23) standards.

## Functions in `<fenv.h>`

| Function | Use | Return Type | Return Value(s) | Example | Output |
| :-- | :-- | :-- | :-- | :-- | :-- |
| `feclearexcept(int excepts)` | Clears specified floating-point exceptions | int | 0 on success | `feclearexcept(FE_DIVBYZERO)`; | 0 |
| `fegetenv(fenv_t *envp)` | Stores current floating-point environment | int | 0 on success | `fegetenv(&env);` | 0 |
| `fegetexceptflag(fexcept_t *flagp, int excepts)` | Stores status flags | int | 0 on success | `fegetexceptflag(&flag, FE_OVERFLOW);` | 0 |
| `fegetround(void)` | Gets rounding direction | int | One of FE_* rounding macros | `fegetround();` | e.g., 0 (FE_TONEAREST) |
| `feholdexcept(fenv_t *envp)` | Saves environment \& clears all status flags | int | 0 on success | `feholdexcept(&env);` | 0 |
| `feraiseexcept(int excepts)` | Raises specified exceptions | int | 0 on success | `feraiseexcept(FE_OVERFLOW);` | 0 |
| `fesetenv(const fenv_t *envp)` | Sets floating-point environment | int | 0 on success | `fesetenv(FE_DFL_ENV);` | 0 |
| `fesetexceptflag(const fexcept_t *flagp, int excepts)` | Sets status flags | int | 0 on success | `fesetexceptflag(&flag, FE_DIVBYZERO);` | 0 |
| `fesetround(int round)` | Sets rounding direction | int | 0 on success | `fesetround(FE_UPWARD);` | 0 |
| `fetestexcept(int excepts)` | Tests which exceptions are set | int | Bitmask of exceptions | `fetestexcept(FE_INVALID);` | 0 or FE_INVALID |
| `feupdateenv(const fenv_t *envp)` | Restores environment, raises exceptions | int | 0 on success | `feupdateenv(&env);` | 0 |

> Note: Return values are zero for success and nonzero otherwise, unless stated (e.g., `fegetround` returns an int encoding the rounding mode).

## Key Macros in `<fenv.h>`

All exceptions and rounding mode macros are named as follows:

- **Exception Macros**:

  - `FE_DIVBYZERO` — Divide-by-zero flag
  - `FE_INEXACT` — Inexact result flag
  - `FE_INVALID` — Invalid operation flag
  - `FE_OVERFLOW` — Overflow flag
  - `FE_UNDERFLOW` — Underflow flag
  - `FE_ALL_EXCEPT` — Bitwise OR of all above

- **Rounding Macros**:

  - `FE_TONEAREST`
  - `FE_DOWNWARD`
  - `FE_UPWARD`
  - `FE_TOWARDZERO`
  - Some platforms: `FE_DEC_*` for decimal floating-point

- **Default Environment**:

  - `FE_DFL_ENV` — default environment at program startup

## Pragma Directive

- **`#pragma STDC FENV_ACCESS`**

This pragma informs the compiler that the program may access the floating-point environment to test status flags or change control modes. It must be set to ON before using any fenv.h functions safely.

**Syntax:**

- **`#pragma STDC FENV_ACCESS ON`**: Enables floating-point environment access
- **`#pragma STDC FENV_ACCESS OFF`**: Disables floating-point environment access (default)

**Important Notes:**

1. When `ON`, the compiler disables certain floating-point optimizations that could interfere with environment access
2. Default state is implementation-defined but typically `OFF`
3. Required for portable, reliable use of `fenv.h` functions

**Example:**

```c
#include <fenv.h>
#include <stdio.h>

int main() {
    // This pragma is REQUIRED before using fenv.h functions
    #pragma STDC FENV_ACCESS ON
    
    // Now it's safe to use floating-point environment functions
    feclearexcept(FE_ALL_EXCEPT);
    fesetround(FE_UPWARD);
    
    printf("Environment access enabled\n");
    return 0;
}
```

## Types

- `fenv_t` — Represents the entire floating-point environment, including all status flags and control modes. This type is used to save and restore the complete floating-point state.
- `fexcept_t` — Represents floating-point status flags collectively. This type stores the state of exception flags for later restoration.

## Example Code and Outputs

### Raising, Checking, and Clearing Exceptions

```c
#include <stdio.h>
#include <fenv.h>
#pragma STDC FENV_ACCESS ON

int main(void) {
    feraiseexcept(FE_DIVBYZERO);
    if (fetestexcept(FE_DIVBYZERO)) {
        printf("FE_DIVBYZERO is set\n");
    }
    feclearexcept(FE_ALL_EXCEPT);
    if (!fetestexcept(FE_DIVBYZERO)) {
        printf("FE_DIVBYZERO is cleared\n");
    }
    return 0;
}
```

**Output:**

```c
FE_DIVBYZERO is set
FE_DIVBYZERO is cleared
```

### Saving and Restoring the Environment, Changing Rounding

```c
#include <stdio.h>
#include <fenv.h>
#pragma STDC FENV_ACCESS ON

void show_status() {
    printf("Rounding mode is FE_TOWARDZERO: %d\n", fegetround() == FE_TOWARDZERO);
    printf("FE_DIVBYZERO is set: %d\n", fetestexcept(FE_DIVBYZERO) != 0);
}

int main(void) {
    fenv_t env;
    fegetenv(&env);
    fesetround(FE_TOWARDZERO);
    feraiseexcept(FE_DIVBYZERO);
    show_status();
    fesetenv(&env);
    show_status();
    return 0;
}
```

**Output:**

```c
Rounding mode is FE_TOWARDZERO: 1
FE_DIVBYZERO is set: 1
Rounding mode is FE_TOWARDZERO: 0
FE_DIVBYZERO is set: 0
```

### Examples for function types

- **`feclearexcept()`**

```c
#include <fenv.h>
#include <stdio.h>

int main() {
    #pragma STDC FENV_ACCESS ON
    
    // Clear all exceptions
    feclearexcept(FE_ALL_EXCEPT);
    
    // Clear specific exceptions  
    feclearexcept(FE_DIVBYZERO | FE_OVERFLOW);
    
    printf("Exceptions cleared\n");
    return 0;
}
```

**Output:**

```txt
Exceptions cleared
```

- **`fetestexcept()`**

```c
#include <fenv.h>
#include <stdio.h>
#include <math.h>

int main() {
    #pragma STDC FENV_ACCESS ON
    double result;
    int flags;
    
    feclearexcept(FE_ALL_EXCEPT);
    result = 1.0 / 0.0;  // This will set FE_DIVBYZERO
    
    flags = fetestexcept(FE_DIVBYZERO | FE_OVERFLOW);
    if (flags & FE_DIVBYZERO) {
        printf("Division by zero detected\n");
    }
    if (flags & FE_OVERFLOW) {
        printf("Overflow detected\n");
    }
    
    return 0;
}
```

```txt
Division by zero detected
```

- **`feraiseexcept()`**

```c
#include <fenv.h>
#include <stdio.h>

int main() {
    #pragma STDC FENV_ACCESS ON
    
    // Manually raise division by zero exception
    feraiseexcept(FE_DIVBYZERO);
    
    if (fetestexcept(FE_DIVBYZERO) == FE_DIVBYZERO) {
        printf("Division by zero exception raised\n");
    }
    
    return 0;
}
```

```txt
Division by zero exception raised
```

- **`fegetexceptflag() and fesetexceptflag()`**

```c
#include <fenv.h>
#include <stdio.h>

int main() {
    #pragma STDC FENV_ACCESS ON
    fexcept_t saved_flags;
    
    // Save current exception flags
    fegetexceptflag(&saved_flags, FE_ALL_EXCEPT);
    
    // Raise some exceptions
    feraiseexcept(FE_OVERFLOW | FE_INVALID);
    printf("Exceptions raised\n");
    
    // Restore previous flag state
    fesetexceptflag(&saved_flags, FE_ALL_EXCEPT);
    printf("Exception flags restored\n");
    
    return 0;
}
```

```txt
Exceptions raised
Exception flags restored
```

- **`fegetround()`**
- **`fesetround()`**

```c
#include <fenv.h>
#include <stdio.h>
#include <math.h>

int main() {
  #pragma STDC FENV_ACCESS ON
    double value = 2.5;
    
    // Test different rounding modes
    fesetround(FE_TONEAREST);
    printf("FE_TONEAREST: %.1f -> %.0f\n", value, nearbyint(value));
    
    fesetround(FE_UPWARD);
    printf("FE_UPWARD: %.1f -> %.0f\n", value, nearbyint(value));
    
    fesetround(FE_DOWNWARD);
    printf("FE_DOWNWARD: %.1f -> %.0f\n", value, nearbyint(value));
    
    fesetround(FE_TOWARDZERO);
    printf("FE_TOWARDZERO: %.1f -> %.0f\n", value, nearbyint(value));
    
    return 0;
}
```

```txt
FE_TONEAREST: 2.5 -> 2
FE_UPWARD: 2.5 -> 3
FE_DOWNWARD: 2.5 -> 2
FE_TOWARDZERO: 2.5 -> 2
```

- **`fegetenv() and fesetenv()`**
- **`feholdexcept()`**
- **`feupdateenv()`**

```c
#include <fenv.h>
#include <stdio.h>
#include <math.h>

void show_status(void) {
    printf("Rounding is FE_TOWARDZERO: %d\n", 
           fegetround() == FE_TOWARDZERO);
    printf("FE_DIVBYZERO is set: %d\n", 
           fetestexcept(FE_DIVBYZERO) != 0);
}

int main() {
    #pragma STDC FENV_ACCESS ON
    fenv_t env;
    
    fegetenv(&env);  // Save environment
    fesetround(FE_TOWARDZERO);  // Change rounding
    feraiseexcept(FE_DIVBYZERO);  // Raise exception
    
    show_status();
    
    fesetenv(&env);  // Restore environment
    show_status();
    
    return 0;
}
```

```txt
Rounding is FE_TOWARDZERO: 1
FE_DIVBYZERO is set: 1
Rounding is FE_TOWARDZERO: 0
FE_DIVBYZERO is set: 0
```

### Examples for Macro types

- **Exception Flag Macros**

```c
#include <fenv.h>
#include <stdio.h>
#include <math.h>

int main() {
    #pragma STDC FENV_ACCESS ON
    
    feclearexcept(FE_ALL_EXCEPT);
    
    double result1 = 1.0 / 0.0;      // FE_DIVBYZERO
    double result2 = sqrt(-1.0);     // FE_INVALID  
    double result3 = exp(1000);      // FE_OVERFLOW
    double result4 = 1.0 / exp(1000); // FE_UNDERFLOW
    double result5 = 1.0 / 3.0;      // FE_INEXACT
    
    printf("FE_DIVBYZERO: %s\n", 
           fetestexcept(FE_DIVBYZERO) ? "SET" : "CLEAR");
    printf("FE_INVALID: %s\n", 
           fetestexcept(FE_INVALID) ? "SET" : "CLEAR");
    printf("FE_OVERFLOW: %s\n", 
           fetestexcept(FE_OVERFLOW) ? "SET" : "CLEAR");
    printf("FE_UNDERFLOW: %s\n", 
           fetestexcept(FE_UNDERFLOW) ? "SET" : "CLEAR");
    printf("FE_INEXACT: %s\n", 
           fetestexcept(FE_INEXACT) ? "SET" : "CLEAR");
    
    return 0;
}
```

- **`Rounding Mode Macros`**

```c
#include <fenv.h>
#include <stdio.h>
#include <math.h>

int main() {
    #pragma STDC FENV_ACCESS ON
    double values[] = {2.3, 2.7, -2.3, -2.7, 2.5, -2.5};
    int modes[] = {FE_TONEAREST, FE_UPWARD, FE_DOWNWARD, FE_TOWARDZERO};
    char* mode_names[] = {"FE_TONEAREST", "FE_UPWARD", "FE_DOWNWARD", "FE_TOWARDZERO"};
    
    for (int m = 0; m < 4; m++) {
        printf("\n=== %s ===\n", mode_names[m]);
        fesetround(modes[m]);
        
        for (int i = 0; i < 6; i++) {
            printf("%.1f -> %.0f\n", values[i], nearbyint(values[i]));
        }
    }
    
    return 0;
}
```

**Output:**

```txt
=== FE_TONEAREST ===
2.3 -> 2
2.7 -> 3
-2.3 -> -2
-2.7 -> -3
2.5 -> 2
-2.5 -> -2

=== FE_UPWARD ===
2.3 -> 3
2.7 -> 3
-2.3 -> -2
-2.7 -> -2
2.5 -> 3
-2.5 -> -2

=== FE_DOWNWARD ===
2.3 -> 2
2.7 -> 2
-2.3 -> -3
-2.7 -> -3
2.5 -> 2
-2.5 -> -3

=== FE_TOWARDZERO ===
2.3 -> 2
2.7 -> 2
-2.3 -> -2
-2.7 -> -2
2.5 -> 2
-2.5 -> -2
```

- **Environment Macros**

```c
#include <fenv.h>
#include <stdio.h>

int main() {
    #pragma STDC FENV_ACCESS ON
    
    // Modify environment
    fesetround(FE_UPWARD);
    feraiseexcept(FE_OVERFLOW);
    
    printf("Modified state:\n");
    printf("Rounding mode FE_UPWARD: %s\n", 
           fegetround() == FE_UPWARD ? "YES" : "NO");
    printf("FE_OVERFLOW set: %s\n", 
           fetestexcept(FE_OVERFLOW) ? "YES" : "NO");
    
    // Restore default environment
    fesetenv(FE_DFL_ENV);
    
    printf("\nAfter fesetenv(FE_DFL_ENV):\n");
    printf("Rounding mode FE_TONEAREST: %s\n", 
           fegetround() == FE_TONEAREST ? "YES" : "NO");
    printf("FE_OVERFLOW set: %s\n", 
           fetestexcept(FE_OVERFLOW) ? "YES" : "NO");
    
    return 0;
}
```txt
Modified state:
Rounding mode FE_UPWARD: YES
FE_OVERFLOW set: YES

After fesetenv(FE_DFL_ENV):
Rounding mode FE_TONEAREST: YES
FE_OVERFLOW set: NO
```

## C23 and Beyond

C23 does not substantially alter the API, but may include additional implementation-defined macros for future extensions.[^3]

***

## Summary Table

| Feature | Macro/Function | Description | Example |
| :-- | :-- | :-- | :-- |
| Exception test/set | `fetestexcept`, `feraiseexcept`, macros | Test/raise exception flags | See above |
| Rounding management | `fegetround`, `fesetround`, macros | Get/set rounding mode | See above |
| Env. save/restore | `fegetenv`, `fesetenv`, `fenv_t` | Manipulate env. snapshots | See above |
| Default environment | `FE_DFL_ENV` | Program-start floating environment | `fesetenv(FE_DFL_ENV);` |

## Key Usage Guidelines

1. Always use `#pragma STDC FENV_ACCESS ON` before calling fenv.h functions
2. **Thread safety**: The floating-point environment has thread storage duration
3. **Function behavior**: Most functions return 0 on success, non-zero on failure
4. **Exception persistence**: Exception flags remain set until explicitly cleared
5. **Rounding mode scope**: Changes to rounding mode persist until explicitly changed
6. **Environment restoration**: Use fegetenv/fesetenv or `FE_DFL_ENV` to restore known states

The `fenv.h` header provides essential functionality for applications requiring precise control over floating-point arithmetic behavior, making it invaluable for numerical computing, financial applications, and any code where floating-point precision and exception handling are critical.

***
