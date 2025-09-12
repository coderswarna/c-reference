# **Reference for `<stdalign.h>` in C**

**Key Takeaway:** The `<stdalign.h>` header provides four macros and two C11 alignment keywords to query and specify object alignment. Use these macros and keywords to enforce or inspect alignment requirements for types and variables in portable C code.

## **Defined Macros in `<stdalign.h>`**

| Macro | Expands to | Description |
| :-- | :-- | :-- |
| **`alignas`** | `_Alignas` | Alignment specifier macro. Forces alignment of a declaration to a constant expression or type. |
| **`alignof`** | `_Alignof` | Alignment inquiry macro. Yields the alignment requirement of a type as a `size_t` integer. |
| **`__alignas_is_defined`** | — | Expands to the integer constant `1`. Indicates that `alignas` macro is available. |
| **`__alignof_is_defined`** | — | Expands to the integer constant `1`. Indicates that `alignof` macro is available. |

The availability macros are suitable for use in `#if` preprocessing checks.

## **C11 Alignment Keywords**

- **`_Alignas`:**
    A type specifier keyword. Applies to declarations to enforce a specified alignment (constant expression or type).
- **`_Alignof`:**
    An operator keyword. Yields the alignment requirement of a type or variable (constant integer expression).

The macro versions match these keywords for compatibility and readability.

## **Usage of `alignas`**

```c
#include <stdio.h>
#include <stdalign.h>

int main(void) {
    alignas(32) double x;         // Force 32-byte alignment
    alignas(int) char c;          // Align 'c' as if it were an 'int'
    int y;                        // Default alignment

    printf("Address of x: %p\n", (void*)&x);
    printf("Address of c: %p\n", (void*)&c);
    printf("Address of y: %p\n", (void*)&y);

    return 0;
}
```

**Sample Output**:

```c
Address of x: 0x7ffc12345000
Address of c: 0x7ffc12344ffc
Address of y: 0x7ffc12344ff8
```

>_Allocation addresses will vary; alignment of `x` is guaranteed on a 32-byte boundary_.

## **Usage of `alignof`**

```c
#include <stdio.h>
#include <stdalign.h>

int main(void) {
    printf("alignof(char)   = %zu\n", alignof(char));
    printf("alignof(int)    = %zu\n", alignof(int));
    printf("alignof(float)  = %zu\n", alignof(float));
    printf("alignof(double) = %zu\n", alignof(double));
    printf("alignof(void*)  = %zu\n", alignof(void*));
    return 0;
}
```

**Sample Output**:

```c
alignof(char)   = 1
alignof(int)    = 4
alignof(float)  = 4
alignof(double) = 8
alignof(void*)  = 8
```

>_Returns the required alignment in bytes as a `size_t` constant_.

## **Alignment Introspection Macros**

You may conditionally compile code based on support for these macros:

```c
#include <stdalign.h>

#if __alignas_is_defined
#pragma message("alignas macro is available")
#endif

#if __alignof_is_defined
#pragma message("alignof macro is available")
#endif
```

These macros expand to `1` and allow portable checks in preprocessor logic.

***
