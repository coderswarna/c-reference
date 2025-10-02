# **Comprehensive Reference for `<time.h>` in C**

## **Overview**

The **`<time.h>`** header provides facilities for date and time manipulation. It defines types, macros, functions, and objects to represent and convert calendar time, CPU time, and high-resolution timestamps.

***

## **Types and Structures**

**`time_t`**

- **Definition**: Arithmetic type capable of representing times.
- **Use**: Holds seconds since the Epoch (1970-01-01 UTC).

**`clock_t`**

- **Definition**: Arithmetic type representing processor time.
- **Use**: Returned by `clock()` to measure CPU time.

**`size_t`**

- **Definition**: Unsigned integer type for object sizes.
- **Use**: Length arguments (e.g., in `strftime`).

**`struct tm`**

```c
struct tm {
    int tm_sec;    /* seconds [0,59] */
    int tm_min;    /* minutes [0,59] */
    int tm_hour;   /* hours   [0,23] */
    int tm_mday;   /* day     [1,31] */
    int tm_mon;    /* month   [0,11] */
    int tm_year;   /* years since 1900 */
    int tm_wday;   /* weekday [0,6] (Sun=0) */
    int tm_yday;   /* day of year [0,365] */
    int tm_isdst;  /* DST flag (>0 DST in effect) */
};
```

- **Use**: Holds broken-down time components for conversion functions.

**`struct timespec`**

```c
struct timespec {
    time_t tv_sec;   /* seconds */
    long   tv_nsec;  /* nanoseconds */
};
```

- **Use**: High-resolution timestamp for `clock_gettime`, `nanosleep`, etc.

**`struct itimerspec`**

```c
struct itimerspec {
    struct timespec it_interval; /* timer period */
    struct timespec it_value;    /* timer expiration */
};
```

- **Use**: Timer configuration for POSIX timers.

***

## **Macros and Manifest Constants**

- **`NULL`**: Null pointer constant.
- **`CLOCKS_PER_SEC`**: Number of clock ticks per second for `clock()`.
- **Clock IDs (for POSIX clocks)**
  - `CLOCK_REALTIME`
  - `CLOCK_MONOTONIC`
  - `CLOCK_PROCESS_CPUTIME_ID`
  - `CLOCK_THREAD_CPUTIME_ID`
- **Timer Flags**
  - `TIMER_ABSTIME` — indicates absolute time in timer functions.
- **Time Base**
  - `TIME_UTC` — for `timespec_get` (ISO C11).

***

## **Variables**

- **`extern int daylight;`**: Nonzero if Daylight Savings Time is in effect.
- **`extern long timezone;`**: Seconds difference between UTC and local standard time.
- **`extern char *tzname;`**: Standard (tzname) and DST (tzname) timezone abbreviations.

***

## **Functions**

### 1. **Calendar Time Conversion**

| Function | Prototype | Return Type | Description |
| :-- | :-- | :-- | :-- |
| `time` | `time_t time(time_t *tloc);` | `time_t` | Get current calendar time. |
| `difftime` | `double difftime(time_t end, time_t start);` | `double` | Difference in seconds between two `time_t`s. |
| `mktime` | `time_t mktime(struct tm *timeptr);` | `time_t` | Convert `struct tm` to `time_t`. |
| `asctime` | `char *asctime(const struct tm *timeptr);` | `char *` | Convert `struct tm` to human-readable string. |
| `ctime` | `char *ctime(const time_t *timer);` | `char *` | Convert `time_t` to human-readable string. |
| `gmtime` | `struct tm *gmtime(const time_t *timer);` | `struct tm *` | Convert `time_t` to UTC broken-down time. |
| `localtime` | `struct tm *localtime(const time_t *timer);` | `struct tm *` | Convert `time_t` to local broken-down time. |
| `strftime` | `size_t strftime(char *s, size_t max, const char *fmt, const struct tm *tm);` | `size_t` | Format `struct tm` into string by format specifier. |
| `strptime` *(XSI)* | `char *strptime(const char *s, const char *fmt, struct tm *tm);` | `char *` | Parse string to `struct tm` by format specifier. |

### 2. **CPU Time and Clocks**

| Function | Prototype | Return Type | Description |
| :-- | :-- | :-- | :-- |
| `clock` | `clock_t clock(void);` | `clock_t` | Processor time used by program. |
| `clock_getres` | `int clock_getres(clockid_t clk_id, struct timespec *res);` | `int` | Get resolution of specified clock. |
| `clock_gettime` | `int clock_gettime(clockid_t clk_id, struct timespec *tp);` | `int` | Get current value of specified clock. |
| `clock_settime` | `int clock_settime(clockid_t clk_id, const struct timespec *tp);` | `int` | Set specified clock (requires privileges). |
| `clock_nanosleep` | `int clock_nanosleep(clockid_t clk_id, int flags, const struct timespec *rqtp, struct timespec *rmtp);` | `int` | High-resolution sleep on specified clock. |
| `nanosleep` | `int nanosleep(const struct timespec *rqtp, struct timespec *rmtp);` | `int` | Sleep for high-resolution interval. |

### 3. **POSIX Timers *(TMR option)***

| Function | Prototype | Return Type | Description |
| :-- | :-- | :-- | :-- |
| `timer_create` | `int timer_create(clockid_t clk_id, struct sigevent *evp, timer_t *timerid);` | `int` | Create a POSIX per-process timer. |
| `timer_delete` | `int timer_delete(timer_t timerid);` | `int` | Delete a POSIX timer. |
| `timer_settime` | `int timer_settime(timer_t timerid, int flags, const struct itimerspec *new, struct itimerspec *old);` | `int` | Arm or disarm a POSIX timer. |
| `timer_gettime` | `int timer_gettime(timer_t timerid, struct itimerspec *curr);` | `int` | Get remaining time until timer expiration. |
| `timer_getoverrun` | `int timer_getoverrun(timer_t timerid);` | `int` | Number of overruns for a timer expiring late. |

### 4. **Extended and Thread-Safe Variants *(TSF, XSI, CX)***

| Function | Prototype | Return Type | Notes |
| :-- | :-- | :-- | :-- |
| `asctime_r` | `char *asctime_r(const struct tm *restrict tm, char *restrict buf);` | `char *` | Reentrant `asctime`. |
| `ctime_r` | `char *ctime_r(const time_t *restrict timer, char *restrict buf);` | `char *` | Reentrant `ctime`. |
| `gmtime_r` | `struct tm *gmtime_r(const time_t *restrict timer, struct tm *restrict tm);` | `struct tm *` | Reentrant `gmtime`. |
| `localtime_r` | `struct tm *localtime_r(const time_t *restrict timer, struct tm *restrict tm);` | `struct tm *` | Reentrant `localtime`. |
| `getdate` *(XSI)* | `struct tm *getdate(const char *string);` | `struct tm *` | Parse date-spec via `DATEMSK`. |
| `getdate_err` *(XSI)* | `int getdate_err;` | `int` | Error code from `getdate`. |
| `tzset` *(CX)* | `void tzset(void);` | `void` | Initializes `timezone`, `tzname`, `daylight`. |
| `clock_getcpuclockid` | `int clock_getcpuclockid(pid_t pid, clockid_t *clk_id);` | `int` | Get CPU-time clock ID for process. |

***

## **Usage Examples**

### 1. **Get Current Time and Convert to Local String**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    printf("Epoch seconds: %ld\n", (long)now);
    printf("Local time: %s", ctime(&now));
    return 0;
}
```

**Output**:

```c
Epoch seconds: 1736687340
Local time: Fri Sep 12 18:02:20 2025
```

### 2. **Measure CPU Time with `clock()`**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    clock_t start = clock();
    for (volatile long i=0; i<100000000; ++i);
    clock_t end = clock();
    double secs = (double)(end - start) / CLOCKS_PER_SEC;
    printf("Elapsed CPU time: %.3f seconds\n", secs);
    return 0;
}
```

**Output** (example)

```c
Elapsed CPU time: 0.450 seconds
```

### 3. **High-Resolution Sleep with `nanosleep()`**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    struct timespec req = { .tv_sec = 1, .tv_nsec = 500000000 }; // 1.5 seconds
    nanosleep(&req, NULL);
    printf("Slept for 1.5 seconds\n");
    return 0;
}
```

**Output**:

```c
Slept for 1.5 seconds
```

### 4. **Format Date with `strftime()`**

```c
#include <time.h>
#include <stdio.h>

int main(void) {
    time_t now = time(NULL);
    struct tm *tm = localtime(&now);
    char buf;
    strftime(buf, sizeof buf, "%Y-%m-%d %H:%M:%S", tm);
    printf("Formatted date: %s\n", buf);
    return 0;
}
```

**Output**:

```c
Formatted date: 2025-09-12 18:05:00
```

***

## **Notes and Best Practices**

- Use `*_r` variants in multithreaded contexts to avoid data races.
- Prefer `clock_gettime(CLOCK_MONOTONIC,…)` over `time()` for measuring intervals, as it is immune to system clock changes.
- Call `tzset()` if you modify the `TZ` environment variable at runtime.
- Always check return values of timer and clock functions for error handling.

This reference covers all standard and POSIX-specified symbols in `<time.h>`, including ISO C11 extensions.

***
