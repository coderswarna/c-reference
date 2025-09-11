# Complete `fenv.h` reference

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

## Types

- `fenv_t` — Represents the complete floating-point environment.
- `fexcept_t` — Represents the floating-point exception flags.

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

***
