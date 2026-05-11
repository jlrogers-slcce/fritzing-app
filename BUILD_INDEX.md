# Fritzing Build System Documentation Index

Welcome! I've created a complete build system for compiling Fritzing on Windows, Linux, and for cross-compilation. Use this page to find the right guide for your needs.

## 🎯 Quick Navigation

### I want to build Fritzing.exe on Windows
1. Start here: **[QUICK_START.md](QUICK_START.md)** - 2 minute overview
2. Then: **[WINDOWS_BUILD_INSTRUCTIONS.md](WINDOWS_BUILD_INSTRUCTIONS.md)** - Detailed steps
3. Use script: **[build-windows-modern.bat](build-windows-modern.bat)** (recommended)
4. Alt script: **[build-windows.ps1](build-windows.ps1)** (user-friendly)

### I want to build Fritzing on Linux
1. Start here: **[QUICK_START.md](QUICK_START.md)** - 2 minute overview
2. Then: **[BUILD_GUIDE.md](BUILD_GUIDE.md)** - Linux section
3. Use script: **[build-linux.sh](build-linux.sh)**
4. Commands: Just run `./build-linux.sh`

### I want the fastest setup possible
→ **[QUICK_START.md](QUICK_START.md)** - Shows all options ranked by speed

### I want to understand the full build system
→ **[BUILD_GUIDE.md](BUILD_GUIDE.md)** - Comprehensive reference for all platforms

### I want to cross-compile for Windows on Linux
→ **[CROSS_COMPILE_WINDOWS.md](CROSS_COMPILE_WINDOWS.md)** - Advanced guide using MXE

### I want a summary of what's new
→ **[BUILD_SYSTEM_SUMMARY.md](BUILD_SYSTEM_SUMMARY.md)** - Overview of all new files and tools

---

## 📚 Documentation Files

### Quick References
| File | Purpose | Best for |
|------|---------|----------|
| [QUICK_START.md](QUICK_START.md) | 2-minute overview and cheat sheet | Everyone - start here! |
| [BUILD_GUIDE.md](BUILD_GUIDE.md) | Comprehensive platform guide | All users |
| [BUILD_SYSTEM_SUMMARY.md](BUILD_SYSTEM_SUMMARY.md) | Summary of new build tools | Understanding the system |

### Platform-Specific Guides
| File | Purpose | Best for |
|------|---------|----------|
| [WINDOWS_BUILD_INSTRUCTIONS.md](WINDOWS_BUILD_INSTRUCTIONS.md) | Detailed Windows native build | Windows users |
| [CROSS_COMPILE_WINDOWS.md](CROSS_COMPILE_WINDOWS.md) | Linux→Windows cross-compilation | Linux developers, CI/CD |

---

## 🛠️ Build Scripts

### Windows
| Script | Type | Features |
|--------|------|----------|
| [build-windows-modern.bat](build-windows-modern.bat) | Batch | ⭐ **Recommended** - Modern, user-friendly, supports VS 2017-2022 |
| [build-windows.ps1](build-windows.ps1) | PowerShell | User-friendly, better error messages, modern syntax |
| [tools/release_fritzing.bat](tools/release_fritzing.bat) | Batch | Original official script, still supported |

### Linux
| Script | Type | Features |
|--------|------|----------|
| [build-linux.sh](build-linux.sh) | Bash | Simple, reliable, auto-detects Qt, parallel build support |

### Configuration
| File | Purpose |
|------|---------|
| [CMakeLists.txt](CMakeLists.txt) | Modern CMake build (alternative to qmake) |
| [phoenix.pro](phoenix.pro) | Original Qt qmake project file (unchanged) |

---

## ⚡ Quick Commands

### Windows
```batch
# Method 1: Modern batch (simplest)
build-windows-modern.bat 64 2022

# Method 2: PowerShell version
.\build-windows.ps1 -Architecture 64 -VisualStudioYear 2022

# Method 3: Traditional
cd tools && release_fritzing.bat 1.0.0 64 2022
```

### Linux
```bash
# Simple one-liner
chmod +x build-linux.sh && ./build-linux.sh
```

### CMake (Any Platform)
```bash
cmake -B build -DCMAKE_PREFIX_PATH=/path/to/Qt6
cmake --build build --config Release
```

---

## 📋 Requirements Summary

### Windows Build
- Visual Studio 2017 or later
- Qt 6.5.3 or later (MSVC build)
- 4 GB free disk space
- 30-45 minutes

### Linux Build
- GCC/Clang with C++17
- Qt6 development packages
- 1-2 GB free disk space
- 10-20 minutes

### Cross-Compile (Linux→Windows)
- MinGW-w64 toolchain
- MXE (M Cross Environment)
- 10+ GB free disk space
- 2-4 hours initial setup

---

## 🎯 Decision Tree

```
Q: What do you want to build?
├─ Windows .exe
│  ├─ Don't want to install Qt locally?
│  │  └─ → Use pre-built releases (github.com/fritzing/fritzing-app/releases)
│  ├─ Have Windows + Visual Studio?
│  │  ├─ Like simple batch scripts?
│  │  │  └─ → Use build-windows-modern.bat ⭐
│  │  ├─ Like PowerShell / more control?
│  │  │  └─ → Use build-windows.ps1
│  │  └─ Want traditional method?
│  │     └─ → Use tools/release_fritzing.bat
│  └─ On Linux?
│     └─ → CROSS_COMPILE_WINDOWS.md (advanced, time-intensive)
│
├─ Linux binary
│  ├─ Want easiest method?
│  │  └─ → ./build-linux.sh ⭐
│  ├─ Want more control?
│  │  └─ → Use CMakeLists.txt with cmake
│  └─ Want Docker (reproducible)?
│     └─ → docker/build-linux.sh
│
└─ Understand the full system?
   └─ → BUILD_GUIDE.md
```

---

## 🔧 Troubleshooting Quick Links

**Errors?** → See [BUILD_GUIDE.md](BUILD_GUIDE.md#troubleshooting)

**Qt version too old?** → [How to fix](WINDOWS_BUILD_INSTRUCTIONS.md#qt-version-requirements)

**Missing dependencies?** → [Install guide](BUILD_GUIDE.md#platform-specific-details)

**Build fails on clean install?** → [Debug guide](BUILD_GUIDE.md#troubleshooting)

---

## 📊 Performance Comparison

| Method | Setup Time | Build Time | Total | Difficulty |
|--------|-----------|-----------|-------|------------|
| Pre-built download | 0 min | 2 min | 2 min | ⭐ None |
| Windows batch build | 30 min* | 15 min | 45 min | ⭐ Easy |
| Windows PowerShell | 30 min* | 15 min | 45 min | ⭐ Easy |
| Linux build | 10 min* | 15 min | 25 min | ⭐ Easy |
| Cross-compile (Linux) | 120 min | 30 min | 150 min | ⭐⭐⭐ Hard |

*Setup time = installing dependencies (Qt, Visual Studio, build tools)

---

## ✨ What's New

This build system adds:

✅ Modern batch script for Windows (`build-windows-modern.bat`)  
✅ PowerShell script for easier debugging (`build-windows.ps1`)  
✅ Simple Linux build script (`build-linux.sh`)  
✅ CMake configuration for modern build systems (`CMakeLists.txt`)  
✅ Comprehensive documentation (5 markdown guides)  
✅ Cross-compilation guide for Linux→Windows  
✅ Platform-specific troubleshooting  
✅ Quick start guide  

---

## 📖 Reading Order

1. **First time?** → Start with [QUICK_START.md](QUICK_START.md)
2. **Platform-specific details?** → [BUILD_GUIDE.md](BUILD_GUIDE.md)
3. **Your platform guide:** → [WINDOWS_BUILD_INSTRUCTIONS.md](WINDOWS_BUILD_INSTRUCTIONS.md) or BUILD_GUIDE.md
4. **Having issues?** → Troubleshooting section in the relevant guide
5. **Want deep dive?** → [BUILD_SYSTEM_SUMMARY.md](BUILD_SYSTEM_SUMMARY.md)

---

## 🤔 FAQ

**Q: I just want Fritzing.exe, where do I start?**
A: [QUICK_START.md](QUICK_START.md) - it shows the 4 fastest ways to get one.

**Q: What's the easiest way to build?**
A: Download pre-built from GitHub releases (2 minutes). If you must build: `build-windows-modern.bat 64 2022` on Windows.

**Q: Does this work on Mac?**
A: The Docker build system supports Linux. For Mac, see the original Fritzing wiki: github.com/fritzing/fritzing-app/wiki

**Q: How long does building take?**
A: Windows build: 30-45 min. Linux build: 15-25 min. Download pre-built: 2 min.

**Q: Do I need Visual Studio?**
A: For Windows build, yes (Community Edition is free). Alternatives: MinGW (complex setup).

**Q: What version of Qt do I need?**
A: Exactly 6.5.3 or newer (verified in phoenix.pro). 6.4.2 is too old.

---

## 🎓 Learning Resources

- **Qt Documentation:** https://doc.qt.io/qt-6/
- **CMake Guide:** https://cmake.org/cmake/help/latest/
- **Fritzing Wiki:** https://github.com/fritzing/fritzing-app/wiki
- **Cross-Compilation:** https://www.mxe.cc/

---

## 💬 Support

- **Issues:** https://github.com/fritzing/fritzing-app/issues
- **Forum:** https://forum.fritzing.org/
- **Build Help:** See troubleshooting in the relevant guide

---

**Last Updated:** May 11, 2026  
**Build System Version:** 1.0  
**Status:** Complete ✅

---

## 📍 File Locations in Repository

```
fritzing-app/
├── README.md                          (original project readme)
├── QUICK_START.md                     ⭐ START HERE
├── BUILD_GUIDE.md                     Main guide
├── BUILD_INDEX.md                     This file (navigation hub)
├── BUILD_SYSTEM_SUMMARY.md            Summary of changes
├── WINDOWS_BUILD_INSTRUCTIONS.md      Windows details
├── CROSS_COMPILE_WINDOWS.md           Advanced cross-compile
│
├── build-windows-modern.bat           ⭐ Windows recommended
├── build-windows.ps1                  Windows (PowerShell)
├── build-linux.sh                     Linux build
├── CMakeLists.txt                     Modern CMake config
│
├── tools/
│   ├── release_fritzing.bat           Original Windows script
│   └── ... (other original tools)
│
└── ... (rest of fritzing-app)
```

---

**Happy building! 🎉**
