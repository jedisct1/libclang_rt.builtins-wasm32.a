# libclang_rt.builtins-wasm32.a

Note: as of 2023, you should probably use the [Zig](https://ziglang.org) SDK instead of this.

The `zig cc` command supports cross-compilation to WASI out of the box, without having to install these
builtins or the WASI SDK.

```sh
zig cc --target=wasm32-wasi -rdynamic -Os -s -o module.wasm module.c
```

--

Starting with version 8 (and version 9 if you need WASI), LLVM can compile to WebAssembly out of the box.

If you are using macOS with Homebrew, or any operating system with up-to-date LLVM packages, you're all set.

Almost.

Unless your WebAssembly application is compute-only, you still need some kind of interface with the runtime.

In order to do so, you may want to download and extract [`wasi-sysroot`](https://github.com/WebAssembly/wasi-sdk/releases). Note that you don't need `wasi-sdk` if you already have LLVM, only `wasi-sysroot`.

*Now* you should be all set. Just replace `/opt/wasi-sysroot` with the location you extracted `wasi-sysroot` to.

```sh
clang --target=wasm32-wasi --sysroot=/opt/wasi-sysroot -O2 test.c
```

Dang. You may see `clang` now complaining about a missing file:

```text
wasm-ld: error: cannot open .../lib/wasi/libclang_rt.builtins-wasm32.a: No such file or directory
```

The missing `libclang_rt.builtins-wasm32.a` can be obtained by recompiling `compiler_rt` from LLVM.

Or directly download that file here: [libclang_rt.builtins-wasm32.a](precompiled/).

It is now also distributed [with the WASI SDK releases](https://github.com/WebAssembly/wasi-sdk/releases), as a separate tarball.

Copy `libclang_rt.builtins-wasm32.a` from the tarball into the path expected by `clang`, creating the `lib` and `wasi` directories if necessary, and you'll be all set!

This has been tested with:
- LLVM 10, 11, 12, 13, 14 (`llvm-10-to-14` folder)
- LLVM 15, 16
- LLVM 17
- LLVM 18
- LLVM 19
