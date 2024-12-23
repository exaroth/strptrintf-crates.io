# Strprintf (part of Libnewsboat library)

__Disclaimer:__ This project is part of [Newsboat](https://github.com/newsboat/newsboat) Rust libraries, I'm not it's author - merely maintaining up to date versions on Crates.io.

## Description

Problem statement for `strprintf` crate: provide a way to interpolate printf-style format
strings using native Rust types. For example, it should be possible to format a string
"%i %.2f %x" using values 42u32, 3.1415f64, and 255u8, and get a `std::string::String` "42 3.14
ff".

This is the same as strprintf module we already have in C++.

The problem can be solved by wrapping `libc::snprintf`, which is what we do both here and in
C++. However, our experience with C++ showed that we should constrain what types can be
formatted. Otherwise, complex objects like `String` will be passed over FFI and lead to
unexpected results (e.g. garbage strings).

To achieve that, we provide a `Printfable` trait that's implemented only for types that our
formatting macro accepts. Everything else will result in a compile-time error. See the docs in
`trait` module for more on that.
