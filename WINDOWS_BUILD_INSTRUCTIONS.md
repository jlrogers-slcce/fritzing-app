# Building Fritzing for Windows

## Prerequisites

Building Fritzing for Windows requires:
- Windows 10/11 or Visual Studio in a VM
- Visual Studio 2017 or later with C++ build tools
- Qt 6.5.3+ (must match the version in phoenix.pro)
- Git for Windows
- libgit2
- quazip
- OpenSSL

## Option 1: Build on Windows Using Visual Studio (Recommended)

### Steps:

1. **Install Visual Studio Build Tools**
   - Download from: https://visualstudio.microsoft.com/downloads/
   - Install with "Desktop development with C++" workload

2. **Install Qt 6.5.3+ for Windows**
   - Download: https://download.qt.io/official_releases/qt/6.5/6.5.3/
   - Use the online installer and select MSVC 2022 x64 (or 2019/2017 as needed)
   - Note the installation path (e.g., `C:\Qt\6.5.3\msvc2022_64`)

3. **Clone and Setup**
   ```batch
   git clone https://github.com/fritzing/fritzing-app.git
   cd fritzing-app
   ```

4. **Run Release Build Script**
   ```batch
   cd tools
   release_fritzing.bat 1.0.0 64 2022
   ```
   - First parameter: version number (e.g., 1.0.0)
   - Second parameter: architecture (64 for x64, 32 for x86)
   - Third parameter: Visual Studio year (2022, 2019, or 2017)

5. **The output .exe will be in**
   ```
   ..\release64\deploy\Fritzing.exe
   ```

### What the build script does:
- Uses qmake to generate Visual Studio project files
- Compiles with nmake
- Copies Qt DLLs and plugins
- Creates a deployable package with all dependencies

## Option 2: Cross-Compile from Linux (Advanced)

Cross-compiling Fritzing for Windows on Linux requires:
- Qt 6.5.3+ built for Windows (via MXE or manual build)
- MinGW-w64 toolchain
- All dependencies (libgit2, quazip, OpenSSL) built for Windows

This is extremely complex and not recommended unless you're familiar with cross-compilation.

## Option 3: Use GitHub Releases (Easiest)

Download pre-built Windows executables from:
https://github.com/fritzing/fritzing-app/releases

## Building on Linux (Creates Linux Binary)

If you want to build for Linux on your current system:

```bash
cd /workspaces/fritzing-app

# Install dependencies
sudo apt-get install -y qt6-base-dev qt6-tools-dev libgit2-dev libquazip1-qt6-dev

# Note: Current Ubuntu 24.04 has Qt 6.4.2, but Fritzing requires 6.5.3+
# You would need to either:
# 1. Download Qt 6.5.3 pre-built binaries, or
# 2. Build Qt 6.5.3 from source (very time-consuming, ~1-2 hours)

# Once you have Qt 6.5.3 installed in /opt/Qt653 (or similar):
export PATH=/opt/Qt653/bin:$PATH
mkdir build && cd build
qmake ../phoenix.pro
make -j$(nproc)

# Binary will be in ./src/Fritzing
```

## Troubleshooting

### "Project ERROR: Use at least Qt version 6.5.3"
- Your Qt installation is too old
- Update to Qt 6.5.3 or later
- Verify qmake version: `qmake --version`

### Missing DLLs when running on Windows
- The release_fritzing.bat script copies all required DLLs
- Ensure the deploy folder has all Qt and system DLLs

### libgit2 not found
- Windows: Build from https://github.com/libgit2/libgit2 or download pre-built
- Update LIBGIT2 path in release_fritzing.bat (line ~68)

## Summary

| Platform | Difficulty | Time | Recommended |
|----------|------------|------|-------------|
| Windows with MSVC | Easy | 30 min | ✅ YES |
| Linux to Windows cross-compile | Hard | 2-4 hrs | ❌ NO |
| Linux native | Medium | 1-2 hrs | ✅ For testing |
| Download pre-built | Trivial | 2 min | ✅ FASTEST |
