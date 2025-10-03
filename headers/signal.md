# **Comprehensive Reference for `<signal.h>` in C**

**Key Takeaway:** The `<signal.h>` header provides facilities for handling asynchronous events (“signals”) such as program interrupts and arithmetic errors. Its core components are two functions (`signal` and `raise`), a set of signal‐handling macros, an integer type for safe flag access, and a list of predefined signal constants.

***

## **1. Fundamental Types and Macros**

Type

- **`sig_atomic_t`**

  Atomic integer type that can be accessed safely in a signal handler.

Handler Macros

- **`SIG_DFL`** – Default handling for the signal.
- **`SIG_IGN`** – Ignore the signal.
- **`SIG_ERR`** – Error return from `signal()`.

Signal Constants (Required by the C Standard)

- **`SIGABRT`** – Abnormal termination (abort).
- **`SIGFPE`**  – Erroneous arithmetic operation (e.g., divide by zero).
- **`SIGILL`**  – Illegal instruction.
- **`SIGINT`**  – Interactive attention request (interrupt from keyboard).
- **`SIGSEGV`** – Invalid storage access (segmentation fault).
- **`SIGTERM`** – Program termination request.

> Note: Some implementations define additional signals (e.g., `SIGUSR1`, `SIGUSR2`, `SIGPIPE`, `SIGCHLD`), but these are not mandated by the ISO C standard.

***

## **2. Functions**

### 2.1 **`signal`**

Prototype

```c
#include <signal.h>
void (*signal(int signum, void (*handler)(int)))(int);
```

- **Purpose:** Install a handler for `signum`.
- **Parameters:**
  - `signum` – the signal number (e.g., `SIGINT`).
  - `handler` – pointer to function taking an `int` (signal number), or one of the macros `SIG_DFL`, `SIG_IGN`.
- **Return Value:**
  - On success: the previous handler (pointer to function or `SIG_DFL`/`SIG_IGN`).
  - On error: `SIG_ERR`.

#### Example

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf("Caught signal %d\n", sig);
}

int main(void) {
    if (signal(SIGINT, handler) == SIG_ERR) {
        perror("signal");
        return 1;
    }
    printf("Press Ctrl+C to trigger SIGINT\n");
    while(1) {
        pause();  // Wait for signal
    }
    return 0;
}
```

**Expected Output (upon Ctrl+C):**

```txt
Press Ctrl+C to trigger SIGINT
Caught signal 2
```

### 2.2 **`raise`**

Prototype

```c
#include <signal.h>
int raise(int sig);
```

- **Purpose:** Send signal `sig` to the calling process.
- **Parameters:**
  - `sig` – signal number.
- **Return Value:**
  - Zero on success.
  - Nonzero on error.

#### **Example for `raise`**

```c
#include <stdio.h>
#include <signal.h>

int main(void) {
    printf("Raising SIGABRT...\n");
    if (raise(SIGABRT) != 0) {
        perror("raise");
        return 1;
    }
    printf("This line is not reached if abort occurs\n");
    return 0;
}
```

**Expected Behavior:**

- The program aborts and generates a core dump (default action for `SIGABRT`).

***

## **3. Usage Patterns and Notes**

- **Atomic Variables in Handlers:** Only objects of type `sig_atomic_t` may be accessed safely within a signal handler without invoking undefined behavior.
- **Reentrancy:** Signal handlers must be minimalist—avoid non‐reentrant functions (e.g., `malloc`, `printf` may be unsafe in some environments).
- **Restoring Handlers:** Some environments reset the handler to `SIG_DFL` after a signal is caught; to maintain custom handling, reinstall the handler inside itself.
- **Portable Signals:** Stick to the six standard signals for maximal portability across C implementations.

***

## **4. Complete List at a Glance**

| Category | Name | Description |
| :-- | :-- | :-- |
| Type | `sig_atomic_t` | Atomic integer type safe in handlers |
| Handler Macros | `SIG_DFL` | Default signal handling |
|  | `SIG_IGN` | Ignore the signal |
|  | `SIG_ERR` | Indicates error from `signal()` |
| Required Signals | `SIGABRT` | Abort signal |
|  | `SIGFPE` | Floating‐point exception |
|  | `SIGILL` | Illegal instruction |
|  | `SIGINT` | Interrupt from keyboard |
|  | `SIGSEGV` | Segmentation fault |
|  | `SIGTERM` | Termination request |
| Functions | `signal()` | Install a signal handler |
|  | `raise()` | Send a signal to the calling process |

***

## **5. Practical Example: Graceful Shutdown**

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdlib.h>

volatile sig_atomic_t keep_running = 1;

void shutdown_handler(int sig) {
    keep_running = 0;
    printf("\nReceived %d, shutting down...\n", sig);
}

int main(void) {
    if (signal(SIGTERM, shutdown_handler) == SIG_ERR ||
        signal(SIGINT, shutdown_handler) == SIG_ERR) {
        perror("signal");
        exit(EXIT_FAILURE);
    }

    printf("Running... (send SIGINT or SIGTERM to stop)\n");
    while (keep_running) {
        sleep(1);
    }
    printf("Cleanup and exit\n");
    return 0;
}
```

**Output Sample:**

```txt
Running... (send SIGINT or SIGTERM to stop)
^C
Received 2, shutting down...
Cleanup and exit
```

This program uses `sig_atomic_t` to safely flag termination, installs handlers for both `SIGINT` and `SIGTERM`, and exits gracefully upon receiving either signal.

***
