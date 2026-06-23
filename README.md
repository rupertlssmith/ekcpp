# eco-kernel-cpp

The Eco kernel package: Elm-facing IO modules paired with their native C++
implementations. This is the `eco/kernel` Elm package, providing the low-level
IO operations that Eco programs build on (console, files, environment, HTTP,
processes, concurrency, and the native runtime driver).

## Layout

- `src/Eco/*.elm` — the Elm side: exposed modules (`Eco.Console`, `Eco.File`,
  `Eco.Http`, `Eco.Env`, `Eco.MVar`, `Eco.Process`, `Eco.NativeDriver`,
  `Eco.Runtime`, `Eco.Crash`, and their `*.Error` submodules). These are the
  public API listed in `elm.json`.
- `src/Eco/Kernel/*.js` — JS kernel implementations, used when compiling to the
  JavaScript backend.
- `src/eco/*.{cpp,hpp}` — native C++ implementations for the LLVM/native
  backend. Each module has a `*.cpp` implementation, a `*.hpp` header, and a
  `*Exports.cpp` file that exposes C-linkage entry points for the LLVM JIT to
  call.
- `CMakeLists.txt` — builds the native kernel, linking against the runtime
  (`../runtime/src`) and external libraries (OpenSSL, libcurl, libzip) for the
  HTTP module.

## How it fits

Each exposed Elm module is backed by a native implementation that the runtime
links in. Elm signatures must stay in sync across the `.elm` declarations and
their C++ exports — see the project-level `CLAUDE.md` and `design_docs/` for the
representation invariants the C++ side must honor.

## Building

The kernel is built as part of the top-level CMake build; see the repository
root `CLAUDE.md` for build and test commands.
