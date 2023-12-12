# `posix_predefined.x` not found

This repo should be cloned using `git clone --recursive`.

Demonstrates a bug in pFUnit that causes `posix_predefined.x` to be built in the wrong
build directory if `CMAKE_RUNTIME_OUTPUT_DIRECTORY` is set.

To reproduce the bug:

```bash
cmake -Bbuild . [-DUSE_FETCHCONTENT=ON]
cmake --build build --parallel
```

To use the fix:

```bash
cmake -Bbuild . -DUSE_PLASMAFAIR=ON [-DUSE_FETCHCONTENT=ON]
cmake --build build --parallel
```
