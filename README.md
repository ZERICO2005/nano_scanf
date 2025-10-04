nano-scanf provides an implementation of C99/C23 `(v)sscanf`

Originally based off of `bscanf` https://github.com/tusharjois/bscanf

# Requirements

nano-scanf targets C99 or later, and requires the following headers:
* `<ctype.h>` for `isspace` and `isdigit`.
* `<inttypes.h>` for `strtoimax` and `stroumax` along with `intmax_t` and `uintmax_t`.
* `<limits.h>` for checking that `sizeof(intmax_t) >= sizeof(long long)`.
* `<stdarg.h>` for varargs.
* `<stdbool.h>` for boolean data type.
* `<stddef.h>` for `size_t` and `ptrdiff_t`.
* `<string.h>` for `memcpy`, `strchr`, and `strspn`/`strcspn`.
* `<stdio.h>` for `EOF`.
* `<stdlib.h>` for `strto(f/d/ld)` when floats are enabled, and possibly `strtol` and `stroul`.

nano-scanf may not work properly if libc is not implemented correctly:
- `memcpy(dst, src, 0)` should work.
- `strspn` and `strcspn` should work correctly when given an empty string `""` for one or both of its arguments.
- `strtol`, `strtoul`, `strtoimax`, `strtoumax`, and `strtof`/`strtod`/`strtold` should set `endptr` correctly so invalid conversions can be detected. Correct overflow/underflow handling from these functions is not required for nano-scanf to function correctly.

nano-scanf also assumes little-endian. This allows allows integer truncation to be performed with `memcpy`.

# Supported Features

Conversion format support:
- length: `l`, `h`, `ll`, `hh`, `t`, `z`, `j`, `L`
- integer: `d`, `i`, `u`, `o`, `x`, `X`, `b`
- float: `a`, `A`, `e`, `E`, `f`, `F`, `g`, `G`
- characters: `n` (`%*n` will cause nano-scanf to return)
- pointer: Assumes `p` is the same as `x`
- strings: (See below for limitations) `c`, `s`, and `[`

All non-suppressed character sequence types must have a maximum field width:
 - Valid   : `"%*3c %*8s %*12[^abc]"`
 - Valid   : `"%3c  %8s  %12[^abc]"`
 - Valid   : `"%*c  %*s  %*[^abc]"`
 - Invalid : `"%c   %s   %[^abc]"` (This is unsafe as `gets`)

Not supported:
- `wchar_t` (such as `%3lc`)
- `%wN` formats (such as `%w48d` for `__int48`)
- Ranges in `[` (such as `%5[0-9]` or `%5[^a-z]`)

# Configuration

nano-scanf has a few configurable macros to enable/disable features.

# Testing and validity

nano-scanf appears to work in the tests I've come up with, and format strings such as `"%9[^]]%1[]]%*[]]%3c%9[^^]%9[^]^]%3[]^]%*4[]^]%tn%4[^]^]%jn"` appear to be handled okay.

Most of the current testing for nano-scanf was done with Clang 15.0.0 targeting the 24-bit eZ80.

# Contributing

Contributions, feature requests, and bug fixes are welcome. You can either create a GitHub issue or create a PR.
