---
title: Building from source
nav_order: 3
---

If you want the fastest performance on your machine or plan to modify the code, please clone the repository and build it yourself from the command line. Below are simple example commands for **Linux** and **macOS**.

> **Requirements**
> - A C++11-compatible compiler (`GCC ≥ 4.8.1` or `Clang ≥ 3.3`)
> - Optional: OpenMP for multithreading (default on Linux; not included with Apple Clang)

### Linux

```bash
git clone https://github.com/light156/multi-ancestry-COJO.git
cd multi-ancestry-COJO

g++ -std=c++11 -O3 -march=native -DNDEBUG -fopenmp -pthread \
    -I include -I external -I external/Eigen src/*.cpp -o manc_cojo
```

### macOS
macOS’s default compiler (Apple Clang) does not include OpenMP support. Although you can install LLVM via Homebrew for full OpenMP functionality, using one thread is already fast enough as shown above.
So it is fine to just build a single-threaded version for simplicity:

```bash
git clone https://github.com/light156/multi-ancestry-COJO.git
cd multi-ancestry-COJO

clang++ -std=c++11 -O3 -march=native -DNDEBUG \
    -I include -I external -I external/Eigen src/*.cpp -o manc_cojo
```

### Windows
Since large computing clusters primarily run on Linux, we do not specifically target Windows.
However, if there is a real need to run on Windows, you can install [mingw-w64](https://www.msys2.org/) by following the official guide.
After successful installation, you can compile the source code with GCC using the same commands as on Linux to obtain the executable file.
