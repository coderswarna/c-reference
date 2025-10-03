# Complete Reference Guide for `stdatomic.h` in C

## Overview

The `stdatomic.h` header, introduced in C11, provides atomic operations for thread-safe programming. This header defines atomic types, functions, and macros that enable lock-free synchronization between threads, preventing data races and ensuring memory consistency.

## Keywords and Language Features

**`_Atomic` Keyword**: The primary language construct for declaring atomic types:

- `_Atomic(int)` or `_Atomic int` - declares an atomic integer
- `_Atomic(struct point)` - declares an atomic structure
- Can be applied to most types except arrays, functions, or existing atomic types

## Atomic Types

The header defines 37 atomic type aliases corresponding to standard integer and pointer types:

### Basic Atomic Types

| Name | Signature/Declaration | Description | Return_Type |
|:--|:--|:--|:--|
|**`atomic_flag`**| **`_Atomic struct`** | Lock-free boolean flag (guaranteed lock-free) | Two states: set and clear |
|**`atomic_bool`**| **`_Atomic _Bool`** | Atomic boolean type | _Bool |
|**`atomic_char`**, **`atomic_schar`**, **`atomic_uchar`** | **`_Atomic char`**, `schar`, `uchar`|Atomic character type|char, signed char, unsigned char|
|**`atomic_short`**,**`atomic_ushort`**| **`_Atomic short`** |Atomic short integer type|short|
|**`atomic_int`**, **`atomic_uint`**|**`_Atomic int`**|Atomic integer type|int|
|**`atomic_long`**, **`atomic_ulong`**|**`_Atomic long`**|Atomic long integer type|long|
|**`atomic_llong`**, **`atomic_ullong`**|**`_Atomic long`**|Atomic long integer type|long and unsigned long|

### Extended Atomic Types

| Name | Signature/Declaration | Description | Return_Type |
|:--|:--|:--|:--|
|**`atomic_char16_t`, `atomic_char32_t`, `atomic_wchar_t`**|**`_Atomic char16_t, _Atomic char32_t, _Atomic wchar_t`**|Atomic 16-bit, 32-bit, wide character type|char16_t, char32_t, wchar_t|
|**`atomic_int_least8_t`**, **`atomic_uint_least8_t`** | **`_Atomic int_least8_t`**, **`_Atomic uint_least8_t`**|Minimum-width types till 64-bit|int_least8_t and uint_least8_t|
|**`atomic_int_fast8_t`**, **`atomic_uint_fast8_t`**|**`_Atomic int_fast8_t`**, **`_Atomic uint_fast8_t`**|Fastest types till 64-bit|int_fast8_t and uint_fast8_t|
|**`atomic_intptr_t`**, **`atomic_uintptr_t`**|**`_Atomic intptr_t`**, **`_Atomic intptr_t`**|Pointer-sized integers|intptr_t and uintptr_t|
|**`atomic_size_t`**, **`atomic_ptrdiff_t`**|**`_Atomic size_t`**, **`_Atomic ptrdiff_t`**|Atomic Size and pointer difference types|size_t and ptrdiff_t|
|**`atomic_intmax_t`**, **`atomic_uintmax_t`**|**`_Atomic intmax_t`**, **`_Atomic uintmax_t`**|Maximum-width types|intmax_t and uintmax_t|

### Memory Order Enumeration

`memory_order` enumeration with constants:

- `memory_order_relaxed` - No synchronization constraints
- `memory_order_consume` - Dependency-ordered synchronization
- `memory_order_acquire` - Acquire synchronization
- `memory_order_release` - Release synchronization
- `memory_order_acq_rel` - Both acquire and release
- `memory_order_seq_cst` - Sequential consistency (default)

## Macros and Constants

### Initialization Macros

- `ATOMIC_FLAG_INIT`
  - Initializes `atomic_flag` to clear state
  - Must be used to initialize atomic_flag
- `ATOMIC_VAR_INIT(value)`
  - Initializes atomic variables (obsolescent)
  -"Obsolescent in C11, deprecated in later standards"

### Lock-Free Property Macros

Ten macros indicate lock-free capabilities:

1. **`ATOMIC_BOOL_LOCK_FREE`**
2. **`ATOMIC_CHAR_LOCK_FREE`**
3. **`ATOMIC_CHAR16_T_LOCK_FREE`**
4. **`ATOMIC_CHAR32_T_LOCK_FREE`**
5. **`ATOMIC_WCHAR_T_LOCK_FREE`**
6. **`ATOMIC_SHORT_LOCK_FREE`**
7. **`ATOMIC_INT_LOCK_FREE`**
8. **`ATOMIC_LONG_LOCK_FREE`**
9. **`ATOMIC_LLONG_LOCK_FREE`**
10. **`ATOMIC_POINTER_LOCK_FREE`**

- Values: 0 (never), 1 (sometimes), 2 (always lock-free)

### Utility Macros

- `kill_dependency(y)` - Breaks dependency chains for consume ordering

## Functions

### Atomic Flag Operations

```c
_Bool atomic_flag_test_and_set(volatile atomic_flag *obj);
_Bool atomic_flag_test_and_set_explicit(volatile atomic_flag *obj, memory_order order);
void atomic_flag_clear(volatile atomic_flag *obj);
void atomic_flag_clear_explicit(volatile atomic_flag *obj, memory_order order);
```

**Usage**:

- `atomic_flag_test_and_set`: Atomically sets the flag and returns the previous value. Returns `true` if flag was previously set, `false` if clear.
- `atomic_flag_test_and_set_explicit`: Atomically sets flag to true with specified memory order. Returns previous state with memory ordering
- `atomic_flag_clear`: Atomically sets flag to false. Clears flag, always lock-free
- `atomic_flag_clear_explicit`: Atomically sets flag to false with specified memory order. Clears flag with memory ordering

### Initialization and Properties

```c
void atomic_init(volatile A *obj, C value);
_Bool atomic_is_lock_free(const volatile A *obj);
```

**Usage**:

- `atomic_init`: Initializes atomic object with value. Non-atomic initialization, obj must not be accessed during init
- `atomic_is_lock_free`: Determines if atomic type is lock-free. Returns `true` if the atomic type is lock-free.

### Load and Store Operations

```c
C atomic_load(const volatile A *obj);
C atomic_load_explicit(const volatile A *obj, memory_order order);
void atomic_store(volatile A *obj, C desired);
void atomic_store_explicit(volatile A *obj, C desired, memory_order order);
```

**Usage**:

- Basic atomic read/write operations. Default versions use sequential consistency.
- `atomic_load` and `ātomic_store` uses `memory_order_seq_cst` by default

### Exchange Operations

```c
C atomic_exchange(volatile A *obj, C desired);
C atomic_exchange_explicit(volatile A *obj, C desired, memory_order order);
```

**Usage**: Atomically replaces the value and returns the previous value.

### Compare and Exchange Operations

```c
_Bool atomic_compare_exchange_weak(volatile A *obj, C *expected, C desired);
_Bool atomic_compare_exchange_strong(volatile A *obj, C *expected, C desired);
_Bool atomic_compare_exchange_weak_explicit(volatile A *obj, C *expected, C desired, memory_order success, memory_order failure);
_Bool atomic_compare_exchange_strong_explicit(volatile A *obj, C *expected, C desired, memory_order success, memory_order failure);
```

**Key Differences**:

- **Weak**: May fail spuriously, efficient in loops
- **Strong**: Never fails spuriously, preferred when not in loops

**Usage**: Returns `true` if exchange succeeded. Updates `expected` with actual value if exchange fails.

### Arithmetic Fetch Operations

```c
C atomic_fetch_add(volatile A *obj, M operand);
C atomic_fetch_add_explicit(volatile A *obj, M operand, memory_order order);
C atomic_fetch_sub(volatile A *obj, M operand);
C atomic_fetch_sub_explicit(volatile A *obj, M operand, memory_order order);
```

**Usage**:

- Atomically performs arithmetic operation and returns the **previous** value. Works on integer and pointer types.
- `atomic_fetch_add_explicit` and `atomic_fetch_sub_explicit` works with specified memory order

### Bitwise Fetch Operations

```c
C atomic_fetch_or(volatile A *obj, M operand);
C atomic_fetch_xor(volatile A *obj, M operand);  
C atomic_fetch_and(volatile A *obj, M operand);
C atomic_fetch_or_explicit(volatile A *obj, M operand, memory_order order);
C atomic_fetch_xor_explicit(volatile A *obj, M operand, memory_order order);
C atomic_fetch_and_explicit(volatile A *obj, M operand, memory_order order);
```

**Usage**:

- Atomically performs bitwise operation and returns the **previous** value. Only available for integer types.
- The ***`explicit`*** types performs the operation with custom memory order

### Fence Operations

```c
void atomic_thread_fence(memory_order order);
void atomic_signal_fence(memory_order order);
```

**Usage**: `atomic_thread_fence` establishes memory ordering between threads without atomic variables. `atomic_signal_fence` provides lighter synchronization within the same thread for signal handlers.

## Memory Ordering Details

Memory ordering specifies synchronization and ordering constraints:

- **Relaxed**: No synchronization, only atomicity guaranteed
- **Acquire**: Prevents reordering of loads before the atomic operation
- **Release**: Prevents reordering of stores after the atomic operation
- **Acquire-Release**: Combines acquire and release semantics
- **Sequential Consistency**: Global ordering of all atomic operations (default)

## Return Values and Behavior

### Function Return Patterns

- **Load operations**: Return the loaded value
- **Store operations**: Return `void`
- **Exchange operations**: Return previous value
- **Fetch operations**: Return value **before** the operation
- **Compare-exchange**: Return `_Bool` (success/failure)
- **Flag operations**: `test_and_set` returns previous state, `clear` returns `void`

### Lock-Free Guarantees

- `atomic_flag` operations are **always** lock-free
- Other atomic types **may** be lock-free depending on implementation
- Use `atomic_is_lock_free()` to check at runtime

## Compilation Requirements

**Compiler Support**:

- GCC 4.9+ (full C11 atomics support)
- Clang 3.6+ (full C11 atomics support)
- Earlier versions (GCC 4.7-4.8, Clang 3.2-3.5) have partial support

**Compilation**: Use `-std=c11` and link with `-lpthread` for threading.

## Common Usage Patterns

### Spinlock Implementation

```c
atomic_flag lock = ATOMIC_FLAG_INIT;
while (atomic_flag_test_and_set(&lock)) { /* spin */ }
// Critical section
atomic_flag_clear(&lock);
```

### Reference Counting

```c
atomic_int ref_count = ATOMIC_VAR_INIT(1);
int old_count = atomic_fetch_sub(&ref_count, 1);
if (old_count == 1) {
    // Last reference, cleanup
}
```

### Producer-Consumer Synchronization

```c
atomic_store_explicit(&ready, true, memory_order_release);  // Producer
while (!atomic_load_explicit(&ready, memory_order_acquire)) { } // Consumer
```

This comprehensive reference covers all functions, macros, types, and keywords defined in `stdatomic.h`, providing the complete toolkit for atomic operations in C programming.
