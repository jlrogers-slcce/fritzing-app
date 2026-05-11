# Cross-Compile Fritzing for Windows on Linux

This guide explains how to cross-compile Fritzing for Windows from a Linux system.

**Difficulty Level:** Advanced  
**Time Required:** 2-4 hours  
**Recommended:** Only if you cannot build on Windows

## Why Cross-Compilation is Complex

Fritzing depends on many libraries that must be built for Windows:
- Qt 6.5.3+ (with plugins)
- libgit2
- quazip
- OpenSSL
- System libraries

Each must be compiled with MinGW toolchain specifically.

## Two Approaches

### Approach 1: Using MXE (M Cross Environment) - RECOMMENDED

MXE provides pre-built Windows libraries for Linux→Windows cross-compilation.

#### Installation (Ubuntu/Debian):

```bash
# Install MXE (large download ~5GB)
git clone https://github.com/mxe/mxe.git /opt/mxe
cd /opt/mxe

# Build only what Fritzing needs (reduces build time)
make qt6 libgit2 quazip openssl -j4

# Add to PATH
export PATH=/opt/mxe/usr/bin:$PATH
```

#### Build Fritzing:

```bash
cd /workspaces/fritzing-app
mkdir build-win64 && cd build-win64

# Configure for Windows
x86_64-w64-mingw32.static-qmake6 ../phoenix.pro

# Build
make -j4

# Output will be src/Fritzing.exe (or src/Release/Fritzing.exe)
```

**Time to complete:** 1-2 hours for MXE setup, 15-30 min for Fritzing build

### Approach 2: Manual Cross-Compilation

This requires building each dependency separately. Not recommended.

```bash
# 1. Set up MinGW prefix
export PREFIX=/opt/mingw-w64
mkdir -p $PREFIX

# 2. Build OpenSSL for Windows
git clone https://github.com/openssl/openssl.git
cd openssl
./Configure mingw64 --prefix=$PREFIX --cross-compile-prefix=x86_64-w64-mingw32-
make
make install
cd ..

# 3. Build libgit2
git clone https://github.com/libgit2/libgit2.git
cd libgit2
mkdir build && cd build
cmake -DCMAKE_TOOLCHAIN_FILE=../cmake/Modules/WindowsToolchain.cmake \
       -DCMAKE_INSTALL_PREFIX=$PREFIX \
       ..
make install
cd ../..

# 4. Build quazip
git clone https://github.com/stachenov/quazip.git
cd quazip
mkdir build && cd build
cmake -DCMAKE_TOOLCHAIN_FILE=../cmake/cross-toolchain.cmake \
      -DCMAKE_INSTALL_PREFIX=$PREFIX \
      ..
make install
cd ../..

# 5. Build Fritzing
cd fritzing-app
mkdir build-win && cd build-win
x86_64-w64-mingw32-qmake ../phoenix.pro \
  INCLUDEPATH+=$PREFIX/include \
  LIBS+=-L$PREFIX/lib
make
```

**Time to complete:** 3-4 hours

## Using Docker (Alternative)

Fritzing provides Docker containers for Linux builds. For Windows, you could:

1. Create a Windows base Docker image with Visual Studio and Qt installed
2. Copy fritzing sources
3. Run the build inside Docker
4. Extract the .exe output

This is complex but ensures reproducible builds.

## Testing the Cross-Compiled Binary

Install Wine and test:

```bash
wine Fritzing.exe
```

## Troubleshooting

### MinGW libraries not found
```bash
# Install full MinGW-w64 suite
sudo apt-get install mingw-w64 mingw-w64-tools mingw-w64-x86-64-dev
```

### CMake toolchain not found
- Some projects need explicit toolchain files
- See [cmake-mingw-toolchain](https://github.com/mati865/cmake-toolchain/blob/master/mingw.cmake)

### DLL dependencies at runtime
- Use `objdump -p` or `ldd` on Windows to inspect dependencies
- Copy all required DLLs to the same directory as Fritzing.exe

### qmake not working with cross-compile
```bash
# Either:
# 1. Use the MXE version: x86_64-w64-mingw32.static-qmake6
# 2. Or build Qt for Windows manually (see Approach 2)
```

## Verification

After building, verify the executable:

```bash
# Check architecture
file Fritzing.exe
# Expected: PE32+ executable (console) x86-64

# Check dependencies
x86_64-w64-mingw32-objdump -p Fritzing.exe | grep NEEDED
```

## Recommended Solution

For most users:
- ✅ **Build on Windows with Visual Studio** (fastest, easiest)
- ✅ **Download pre-built from GitHub** (zero setup)
- ❌ **Cross-compile on Linux** (unless you're a CI/CD professional)

## References

- MXE: https://mxe.cc/
- MinGW-w64: https://www.mingw-w64.org/
- Qt Cross-Compilation: https://doc.qt.io/qt-6/windows-building.html
- Fritzing Build: https://github.com/fritzing/fritzing-app/wiki
