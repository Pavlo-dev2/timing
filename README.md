# timing

A small, lightweight **C timing utility library** based on POSIX clocks. It provides simple functions to:

* get the current monotonic time
* calculate time differences
* sleep for fractional seconds with nanosecond precision

The library is designed to be minimal, easy to embed, and suitable for performance measurements and precise delays.

---

## Features

* Uses `CLOCK_MONOTONIC` (not affected by system time changes)
* High‑resolution time measurement
* Fractional second sleep using `nanosleep`
* No dynamic memory allocation
* Simple API, easy to integrate

---

## API Overview

```c
long double rettime(void);
long double timediff(long double time1, long double time2);
int sleepsec(long double time);
```

### `long double rettime(void)`

Returns the current monotonic time in **seconds** since an unspecified starting point.

> Suitable for measuring time intervals, **not** for wall‑clock time.

---

### `long double timediff(long double time1, long double time2)`

Returns the difference between two timestamps:

```c
time2 - time1
```

---

### `int sleepsec(long double time)`

Suspends execution for the given number of seconds.

* Supports fractional seconds (e.g. `0.25`, `1.5`)
* Internally uses `nanosleep`

Always returns `0`.

---

## Example Usage

```c
#include <stdio.h>
#include "timing.h"

int main(void)
{
    long double start = rettime();

    sleepsec(1.5);

    long double end = rettime();
    long double diff = timediff(start, end);

    printf("Elapsed time: %.6Lf seconds\n", diff);
    return 0;
}
```

---

## Build & Compile

### Linux / POSIX

```bash
gcc main.c timing.c -o example
```

> No extra libraries are required. `time.h` is part of the standard C library on POSIX systems.

---

## Platform Support

* ✅ Linux
* ✅ BSD
* ✅ macOS (POSIX compliant)
* ❌ Windows (requires POSIX compatibility layer or replacement)

---

## Notes

* `CLOCK_MONOTONIC` is used instead of `CLOCK_REALTIME` to avoid issues with system time changes.
* Precision depends on the underlying system and kernel.
* Intended for benchmarking, profiling, and timing control in low‑level programs.

---

## License

MIT License — free to use, modify, and distribute.

---

## Author

Created as a minimal timing helper library in C.
