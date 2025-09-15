# **Comprehensive Reference: `<threads.h>` in C11**

## **Introduction**

The C11 **`<threads.h>`** header provides a minimal, portable threading API, including thread management, mutexes, condition variables, thread‐specific storage, and one‐time initialization.

***

## **1. Types and Macros**

| Identifier | Category | Description |
| :-- | :-- | :-- |
| **`thrd_t`** | Type | Opaque thread handle |
| **`mtx_t`** | Type | Opaque mutex object |
| **`cnd_t`** | Type | Opaque condition variable |
| **`tss_t`** | Type | Thread-specific storage key |
| **`once_flag`** | Type | One-time initialization flag |
| **`mtx_plain`** | Macro (mutex type) | Default mutex type |
| **`mtx_recursive`** | Macro (mutex type) | Recursive mutex |
| **`mtx_timed`** | Macro (mutex type) | Timed mutex |
| **`thrd_success`** | Macro (return code) | Operation succeeded (value == 0) |
| **`thrd_nomem`** | Macro (return code) | Insufficient memory to create thread |
| **`thrd_timedout`** | Macro (return code) | Timed operation timed out |
| **`thrd_busy`** | Macro (return code) | Mutex is already locked |
| **`TSS_DTOR_ITERATIONS`** | Macro | Max destructor invocations per thread exit (>=4) |
| **`ONCE_FLAG_INIT`** | Macro | Static initializer for `once_flag` |
| **`_Thread_local`** | Keyword | Storage‐class specifier for thread‐local variables |

***

## **2. Thread Management**

### **`thrd_create`**

- **Signature:** **`int thrd_create(thrd_t *thr, thrd_start_t func, void *arg);`**
- **Use:** Create a new thread running `func(arg)`.
- **Return:**
  - `thrd_success` if thread created
  - `thrd_nomem` if insufficient resources
- **Example:**

```c
#include <stdio.h>
#include <threads.h>

int worker(void *arg) {
    printf("Hello from thread: %s\n", (char*)arg);
    return 42;
}

int main(void) {
    thrd_t thr;
    if (thrd_create(&thr, worker, "C11") != thrd_success) {
        return 1;
    }
    int res;
    thrd_join(thr, &res);
    printf("Thread returned %d\n", res);
    return 0;
}
```

- **Output:**

```txt
Hello from thread: C11
Thread returned 42
```

### **`thrd_exit`**

- **Signature: `void thrd_exit(int res);`**
- **Use:** Terminate calling thread with result **`res`**.
- **Return:** Does not return.
- **Example (inside thread):**

```c
thrd_exit(7);
```

### **`thrd_current`**

- **Signature: `thrd_t thrd_current(void);`**
- **Use:** Obtain handle of calling thread.
- **Return:** Current thread’s **`thrd_t`**.
- **Example:**

```c
thrd_t self = thrd_current();
```

### **`thrd_equal`**

- **Signature: `int thrd_equal(thrd_t t1, thrd_t t2);`**
- **Use:** Compare two thread handles.
- **Return:** Non-zero if equal, zero otherwise.
- **Example:**

```c
if (thrd_equal(t1, t2)) { /* same thread */ }
```

### **`thrd_detach`**

- **Signature: `int thrd_detach(thrd_t thr);`**
- **Use:** Mark thread resources reclaimable on exit.
- **Return:** **`thrd_success`** or error.
- **Example:**

```c
thrd_detach(thr);
```

### **`thrd_join`**

- **Signature: `int thrd_join(thrd_t thr, int *res);`**
- **Use:** Wait for **`thr`** to finish; store its return value in **`*res`**.
- **Return:** `thrd_success` or error.
- **Example:** See **`thrd_create`** example above.

***

## **3. Mutexes**

### **`mtx_init / mtx_destroy`**

- **Signatures:**
  - **`int mtx_init(mtx_t *mtx, int type);`**
  - **`void mtx_destroy(mtx_t *mtx);`**
- **Use:** Initialize/destroy a mutex of specified **`type` (`mtx_plain`, `mtx_recursive`, `mtx_timed`)**.
- **Return:**
  - `thrd_success` on `mtx_init` success, or `thrd_nomem`
- **Example:**

```c
mtx_t mtx;
mtx_init(&mtx, mtx_plain);
/* ... */
mtx_destroy(&mtx);
```

### **`mtx_lock / mtx_trylock / mtx_timedlock / mtx_unlock`**

- **Signatures:**
  - **`int mtx_lock(mtx_t *mtx);`**
  - **`int mtx_trylock(mtx_t *mtx);`**
  - **`int mtx_timedlock(mtx_t *mtx, const struct timespec *ts);`**
  - **`int mtx_unlock(mtx_t *mtx);`**
- **Use:** Acquire/release mutex; `trylock` returns immediately if busy; `timedlock` waits until `ts`.
- **Return:**
  - `thrd_success` on lock/unlock success
  - `thrd_busy` if `trylock` finds it locked
  - `thrd_timedout` if `timedlock` times out
- **Example:**

```c
if (mtx_trylock(&mtx)==thrd_success) {
    /* critical section */
    mtx_unlock(&mtx);
}
```

***

## **4. Condition Variables**

### **`cnd_init / cnd_destroy`**

- **Signatures:**
  - **`int cnd_init(cnd_t *cv);`**
  - **`void cnd_destroy(cnd_t *cv);`**
- **Use:** Initialize/destroy condition variable.
- **Return:** `thrd_success` or `thrd_nomem`.
- **Example:**

```c
cnd_t cv;
cnd_init(&cv);
/* ... */
cnd_destroy(&cv);
```

### **`cnd_wait / cnd_timedwait / cnd_signal / cnd_broadcast`**

- **Signatures:**
  - **`int cnd_wait(cnd_t *cv, mtx_t *mtx);`**
  - **`int cnd_timedwait(cnd_t *cv, mtx_t *mtx, const struct timespec *ts);`**
  - **`int cnd_signal(cnd_t *cv);`**
  - **`int cnd_broadcast(cnd_t *cv);`**
- **Use:**
  - `cnd_wait`: block until signaled, atomically unlocking and relocking `mtx`.
  - `cnd_timedwait`: as above, but with timeout.
  - `cnd_signal`: unblock one waiter.
  - `cnd_broadcast`: unblock all waiters.
- **Return:**
  - `thrd_success` on wake
  - `thrd_timedout` if timed wait times out
- **Example:**

```c
mtx_lock(&mtx);
while (!condition) {
    cnd_wait(&cv, &mtx);
}
mtx_unlock(&mtx);
```

***

## **5. Thread-Specific Storage (TSS)**

### **``tss_create / tss_set / tss_get / tss_delete``**

- **Signatures:**
  - **`int tss_create(tss_t *key, tss_dtor_t dtor);`**
  - **`int tss_set(tss_t key, void *val);`**
  - **`void *tss_get(tss_t key);`**
  - **`void tss_delete(tss_t key);`**
- **Use:**
  - Create per-thread key with optional destructor `dtor`.
  - Store/retrieve thread-specific `val`.
  - Delete key (invokes destructors on current thread).
- **Return:** `thrd_success` or `thrd_nomem`.
- **Example:**

```c
_Thread_local int counter;
tss_t key;
tss_create(&key, NULL);
tss_set(key, &counter);
int *p = tss_get(key);
```

***

## **6. One-Time Initialization**

### **`call_once`**

- **Signature: `void call_once(once_flag *flag, void (*func)(void));`**
- **Use:** Ensure `func()` is called exactly once across all threads.
- **Example:**

```c
once_flag initFlag = ONCE_FLAG_INIT;
void initialize(void) { /* setup code */ }
/* in threads: */
call_once(&initFlag, initialize);
```

***

## **7. Thread-Local Storage Keyword**

- **`_Thread_local`**: Specifies that each thread has its own instance of a variable. Equivalent to `thread_local` in C11.

***

### **Example: Producer–Consumer with Mutex and Condition Variable**

```c
#include <stdio.h>
#include <stdlib.h>
#include <threads.h>

mtx_t mtx;
cnd_t cv;
int data_ready = 0;

int producer(void *arg) {
    mtx_lock(&mtx);
    data_ready = 1;
    printf("Producer: data ready\n");
    cnd_signal(&cv);
    mtx_unlock(&mtx);
    return 0;
}

int consumer(void *arg) {
    mtx_lock(&mtx);
    while (!data_ready) {
        cnd_wait(&cv, &mtx);
    }
    printf("Consumer: received data\n");
    mtx_unlock(&mtx);
    return 0;
}

int main(void) {
    thrd_t prod, cons;
    mtx_init(&mtx, mtx_plain);
    cnd_init(&cv);

    thrd_create(&cons, consumer, NULL);
    thrd_create(&prod, producer, NULL);

    thrd_join(prod, NULL);
    thrd_join(cons, NULL);

    cnd_destroy(&cv);
    mtx_destroy(&mtx);
    return 0;
}
```

**Output:**

```txt
Producer: data ready
Consumer: received data
```

***

## **Summary**

This reference covers **all** types, functions, macros, and the `_Thread_local` keyword defined in `<threads.h>`, along with their uses, return values, and illustrative examples.
