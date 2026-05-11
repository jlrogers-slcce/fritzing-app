# Fritzing Build Guide

This repository now includes comprehensive build instructions for multiple platforms.

## Quick Start

Choose your platform:

### Windows (Recommended for .exe output)

**Fastest way to get Fritzing.exe:**

#### Option A: Use Pre-Built Release
Download from: https://github.com/fritzing/fritzing-app/releases

#### Option B: Build on Windows
Requires: Visual Studio 2017+ and Qt 6.5.3+

```batch
cd tools
release_fritzing.bat 1.0.0 64 2022
```

See [WINDOWS_BUILD_INSTRUCTIONS.md](WINDOWS_BUILD_INSTRUCTIONS.md) for details.

#### Option C: Modern Build Script (Easier Setup)
```batch
build-windows-modern.bat 64 2022
```

### Linux

Build for Linux (outputs Fritzing executable):

```bash
chmod +x build-linux.sh
./build-linux.sh
```

Requirements (Ubuntu/Debian):
```bash
sudo apt-get install -y qt6-base-dev qt6-tools-dev libgit2-dev libquazip1-qt6-dev
```

### Linux to Windows Cross-Compile

Advanced: Cross-compile Fritzing for Windows on Linux.

See [CROSS_COMPILE_WINDOWS.md](CROSS_COMPILE_WINDOWS.md)

Requires:
- MXE (M Cross Environment) or manual toolchain setup
- 2-4 hours build time
- Not recommended unless you're a CI/CD professional

## Platform-Specific Details

| Platform | Method | Difficulty | Time | Output |
|----------|--------|------------|------|--------|
| Windows | Native build | Easy | 30 min | Fritzing.exe |
| Windows | Pre-built | Trivial | 2 min | Fritzing.exe |
| Linux | Native build | Medium | 10-20 min | Fritzing |
| Linux→Windows | MXE | Hard | 2-3 hrs | Fritzing.exe |
| Linux→Windows | Manual | Very Hard | 4+ hrs | Fritzing.exe |

## Build Requirements Summary

### Windows Build
- Visual Studio 2017 or later
- Qt 6.5.3+ (MSVC build)
- Git for Windows
- libgit2
- quazip
- OpenSSL

### Linux Build
- GCC/Clang C++17 compatible
- Qt6 base development files
- libgit2-dev
- libquazip1-qt6-dev
- Standard build tools (make, pkg-config)

### Windows Cross-Compile (from Linux)
- MinGW-w64 toolchain
- MXE (or manually built Windows libraries)
- All Windows dependencies above, cross-compiled

## Advanced Options

### CMake Build (Modern Alternative to qmake)

A CMakeLists.txt is included for modern CMake-based builds:

```bash
mkdir build
cd build
cmake -DCMAKE_PREFIX_PATH=/path/to/Qt6 ..
make
```

Benefits:
- Works with more IDEs
- Better cross-platform support
- Modern build system

### Docker Build

For reproducible Linux builds:

```bash
cd docker
./build-linux.sh fedora-30 release-1.0.0
```

## Troubleshooting

### Qt version too old
```
ERROR: Use at least Qt version 6.5.3
```

**Solution:** Update Qt to 6.5.3 or later
- Windows: Download from https://download.qt.io/official_releases/qt/6.5/
- Linux: Check WINDOWS_BUILD_INSTRUCTIONS.md section on Qt 6.5.3

### Missing dependencies
```
error: ... header not found
```

**Solution:** Install development packages:

Ubuntu/Debian:
```bash
sudo apt-get install -y \
  qt6-base-dev \
  qt6-tools-dev \
  libgit2-dev \
  libquazip1-qt6-dev
```

Fedora/RHEL:
```bash
sudo dnf install -y \
  qt6-qtbase-devel \
  libgit2-devel \
  quazip-qt6-devel
```

### Build fails on Windows

Check Visual Studio environment:
```batch
"C:\Program Files\...\VC\Auxiliary\Build\vcvars64.bat"
```

Then try building again.

## Output Locations

After successful build:

- **Windows:** `release64\deploy\Fritzing.exe` or `release32\deploy\Fritzing.exe`
- **Linux:** `build\src\Fritzing`
- **Cross-compiled .exe on Linux:** `build-win64\src\Fritzing.exe`

## Next Steps

1. Choose your build method above
2. Install required dependencies for your platform
3. Run the build script for your platform
4. Find your compiled executable in the output locations above

## Additional Resources

- Fritzing official site: http://fritzing.org
- Development wiki: https://github.com/fritzing/fritzing-app/wiki
- Issue tracker: https://github.com/fritzing/fritzing-app/issues
- Qt documentation: https://doc.qt.io/qt-6/

## Contributing

If you improve the build system, please submit a pull request!
