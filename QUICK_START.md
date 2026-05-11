# 🚀 Fritzing Build - Quick Reference

## For Windows Users (.exe Output)

### Method 1: Fastest (Pre-built)
```
Visit: https://github.com/fritzing/fritzing-app/releases
Download Fritzing-x.x.x.windows.64.exe
Done in 2 minutes!
```

### Method 2: Modern Batch Script (30 minutes)
```batch
build-windows-modern.bat 64 2022
```
Requirements: Visual Studio 2022 + Qt 6.5.3

### Method 3: PowerShell (More User-Friendly)
```powershell
.\build-windows.ps1 -Architecture 64 -VisualStudioYear 2022
```

### Method 4: Traditional (Original Script)
```batch
cd tools
release_fritzing.bat 1.0.0 64 2022
```

**Output:** `release64\deploy\Fritzing.exe`

---

## For Linux Users

### Quick Build
```bash
chmod +x build-linux.sh
./build-linux.sh
```

### Install Dependencies (Ubuntu/Debian)
```bash
sudo apt-get install -y qt6-base-dev qt6-tools-dev libgit2-dev libquazip1-qt6-dev
```

**Output:** `build/src/Fritzing`

---

## Windows Installation Requirements

| Tool | Version | Download |
|------|---------|----------|
| Visual Studio | 2017+ | visualstudio.microsoft.com |
| Qt | 6.5.3+ | download.qt.io |
| Git | Latest | git-scm.com |

Update paths in build scripts if installed elsewhere.

---

## Common Issues

### "Qt version too old"
→ Install Qt 6.5.3+ from: download.qt.io

### "Visual Studio not found"
→ Edit path in script: `build-windows-modern.bat` line 35

### "Missing DLLs after build"
→ Batch script copies them. Check `release64\deploy\`

### Build takes forever
→ Use `-j4` flag to use 4 cores (slower: `-j1`, faster: `-j8`)

---

## Documentation

📖 **Main Guide:** [BUILD_GUIDE.md](BUILD_GUIDE.md)  
🪟 **Windows Details:** [WINDOWS_BUILD_INSTRUCTIONS.md](WINDOWS_BUILD_INSTRUCTIONS.md)  
🔀 **Cross-Compile:** [CROSS_COMPILE_WINDOWS.md](CROSS_COMPILE_WINDOWS.md)  
📋 **Summary:** [BUILD_SYSTEM_SUMMARY.md](BUILD_SYSTEM_SUMMARY.md)  

---

## Platform Comparison

```
╔════════════════╦═══════════╦══════════╦════════════════════╗
║ Platform       ║ Effort    ║ Time     ║ Output             ║
╠════════════════╬═══════════╬══════════╬════════════════════╣
║ Windows (pre)  ║ None ⭐   ║ 2 min    ║ Fritzing.exe       ║
║ Windows (build)║ Easy ⭐   ║ 30 min   ║ Fritzing.exe       ║
║ Linux          ║ Easy ⭐   ║ 15 min   ║ Fritzing binary    ║
║ Cross-compile  ║ Hard      ║ 2-4 hrs  ║ Fritzing.exe       ║
╚════════════════╩═══════════╩══════════╩════════════════════╝
```

---

## Scripts Included

| Script | Platform | Usage |
|--------|----------|-------|
| `build-windows-modern.bat` | Windows | `build-windows-modern.bat 64 2022` |
| `build-windows.ps1` | Windows | `.\build-windows.ps1` |
| `tools/release_fritzing.bat` | Windows | `cd tools && release_fritzing.bat 1.0.0 64 2022` |
| `build-linux.sh` | Linux | `./build-linux.sh` |
| `CMakeLists.txt` | All | `cmake -B build && cmake --build build` |

---

## Next Steps

1. **Choose your method** from section 1 or 2 above
2. **Read the full guide:** [BUILD_GUIDE.md](BUILD_GUIDE.md)
3. **Install prerequisites** (Qt 6.5.3, Visual Studio, git)
4. **Run the build script** for your platform
5. **Find your executable** in the output folder

---

**Need help?**
- Check: [BUILD_GUIDE.md](BUILD_GUIDE.md) → Troubleshooting
- Issues: github.com/fritzing/fritzing-app/issues
- Forum: forum.fritzing.org

Good luck building Fritzing! 🎉
