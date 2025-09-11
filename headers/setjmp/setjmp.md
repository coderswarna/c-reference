# Non-Local Jumps with setjmp.h

**Key Insight:** The `<setjmp.h>` header defines the ability to perform **non-local jumps** in C by saving execution context via a `jmp_buf` and later restoring it with `longjmp`. This mechanism enables you to “jump back” to an earlier point in the call stack, bypassing normal function return paths.

***

## 1. Core Types and Macros

**`jmp_buf`**
A typedef for an array type capable of holding information needed to restore a program’s calling environment.

```c
#include <setjmp.h>
jmp_buf env;
```

**`setjmp(env)`**

- Category: Function-like macro (often implemented as a macro)
- Prototype: `int setjmp(jmp_buf env);`
- Use: Saves the current calling environment (stack context, registers) into `env`.
- Return values:
    - **0** when saving the environment initially
    - **nonzero** when returning via `longjmp` (the value passed to `longjmp`, or 1 if that value is 0)

**`longjmp(env, val)`**

- Category: Function (not a macro)
- Prototype: `void longjmp(jmp_buf env, int val);`
- Use: Restores the environment previously saved by `setjmp(env)`, causing `setjmp` to return with `val`.
- Behavior:
    - Does not return to the call of `longjmp`; instead, execution resumes at the corresponding `setjmp`.
    - If `val` is 0, `setjmp` returns 1. Otherwise, it returns `val`.

***

## 2. Example: Basic Usage

```c
#include <stdio.h>
#include <setjmp.h>

jmp_buf env;

void second(void) {
    printf("Inside second(), calling longjmp\n");
    longjmp(env, 42);
}

void first(void) {
    second();
    printf("This line is never reached\n");
}

int main(void) {
    int ret = setjmp(env);
    if (ret == 0) {
        printf("setjmp returned %d initially\n", ret);
        first();
    } else {
        printf("Returned to main via longjmp with value %d\n", ret);
    }
    return 0;
}
```

**Expected Output:**

```
setjmp returned 0 initially
Inside second(), calling longjmp
Returned to main via longjmp with value 42
```


***

## 3. Deep Dive: Behavior and Return Values

1. **Initial Call**
    - `setjmp(env)` saves the context and returns `0`.
    - The program proceeds normally.
2. **Long Jump**
    - `longjmp(env, val)` unwinds the call stack to the point of the matching `setjmp`.
    - Control transfers back, and `setjmp(env)` now returns `val` (or `1` if `val == 0`).
3. **Multiple Invocations**
    - You may call `longjmp` multiple times on the same `env`; each call causes `setjmp` to return again with the new value.

***

## 4. POSIX Extensions (Optional)

On POSIX-compliant systems, `<setjmp.h>` may also provide:

- **`sigjmp_buf`**: Like `jmp_buf`, but also saves signal mask.
- **`sigsetjmp(env, savesigs)`**
    - Prototype: `int sigsetjmp(sigjmp_buf env, int savesigs);`
    - Saves calling environment and, if `savesigs != 0`, the current signal mask.
- **`siglongjmp(env, val)`**
    - Prototype: `void siglongjmp(sigjmp_buf env, int val);`
    - Restores environment and optionally restores saved signal mask.

**Example Snippet:**

```c
#include <stdio.h>
#include <signal.h>
#include <setjmp.h>

sigjmp_buf env;

void handler(int sig) {
    printf("Signal %d caught, jumping back\n", sig);
    siglongjmp(env, sig);
}

int main(void) {
    signal(SIGINT, handler);
    int result = sigsetjmp(env, 1);
    if (result == 0) {
        printf("Waiting for Ctrl-C...\n");
        pause();  // wait for signal
    } else {
        printf("Returned from signal handler with %d\n", result);
    }
    return 0;
}
```


***

## 5. Best Practices and Caveats

- **Resource Cleanup:** `longjmp` bypasses normal stack unwinding—destructors in C++ or cleanup code in C won’t run. Use carefully to avoid resource leaks.
- **Volatile Variables:** Local variables modified after `setjmp` but needed after `longjmp` should be declared `volatile` to ensure proper values.
- **Signal Safety:** Only call async-signal-safe functions between `sigsetjmp` and `siglongjmp` when saving/restoring signal masks.

***

By leveraging `setjmp` and `longjmp`, developers can implement error recovery, coroutines, or lightweight backtracking. However, due to their non-local nature, they should be used judiciously and with full awareness of stack state and resource management.

