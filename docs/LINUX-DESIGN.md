# Skales Linux Desktop - Design Specification

## 1. Overview

This document specifies the design for adding Linux desktop support to Skales, an Electron-based AI assistant application. The implementation extends the existing cross-platform Electron architecture to include Linux-specific packaging (AppImage, deb, rpm) and system integration (tray, autostart, updates).

## 2. Current Architecture

### Existing Platform Support
- **Windows**: NSIS installer, node_modules.tar.gz extraction
- **macOS**: DMG with Helper executable for background processing

### Existing Code Structure
```
skales/
├── electron/
│   ├── main.js          # Main process (platform-specific: win32, darwin)
│   ├── preload.js       # Preload script
│   ├── tray.js          # System tray (platform-specific)
│   ├── updater.js       # Auto-updater
│   ├── splash.html      # Splash screen
│   └── icons/           # Platform icons (icon.ico, icon.icns)
├── electron-builder.yml # Build configuration
└── package.json         # Build scripts
```

## 3. Linux-Specific Requirements

### 3.1 Package Formats
| Format | Target | Use Case |
|--------|--------|----------|
| AppImage | Universal | Primary distribution - single portable file |
| deb | Debian/Ubuntu | apt-get install support |
| rpm | Fedora/RHEL | yum/dnf install support |

### 3.2 Features
- System tray with context menu
- Auto-launch at login (XDG autostart)
- Auto-updates via electron-updater
- Native file associations (optional future)

### 3.3 Build Commands
```bash
# Local build commands to add
npm run build:linux       # Build all Linux formats
npm run build:appimage   # AppImage only
npm run build:deb        # Debian package only
npm run build:rpm        # RPM package only
```

## 4. Design: electron-builder.yml Changes

### 4.1 Linux Configuration Section

```yaml
# ── Linux ───────────────────────────────────────────────────────────────────────
linux:
  target:
    - target: AppImage
      arch: [x64]
    - target: deb
      arch: [x64, arm64]
    - target: rpm
      arch: [x64]
  icon: electron/icons  # Searches for icon.png, icon.icns in this dir
  category: Utility
  desktop:
    # XDG desktop entry
    MimeType: x-scheme-handler/skales
    StartupNotify: true
    # Categories for menu integration
    Categories: Network;InstantMessaging;AI;
  # Executable name in the package
  executableName: skales
```

### 4.2 Linux-specific asarUnpack
No additional unpack required - Linux uses native node_modules folder like macOS.

### 4.3 Publish Configuration
```yaml
# Add Linux to existing publish config (auto-updater)
publish:
  provider: generic
  url: https://skales.app/updates/
  channel: latest  # Single channel works for all platforms
```

## 5. Design: main.js Modifications

### 5.1 Platform-Specific Icon Path
**Current:**
```javascript
// Line 321-325 in main.js
icon: path.join(
  __dirname,
  'icons',
  process.platform === 'darwin' ? 'icon.icns' : 'icon.ico'
)
```

**Proposed Change:**
```javascript
icon: (() => {
  const iconDir = path.join(__dirname, 'icons');
  if (process.platform === 'darwin') return path.join(iconDir, 'icon.icns');
  if (process.platform === 'win32') return path.join(iconDir, 'icon.ico');
  // Linux: use PNG (electron-builder converts to multiple sizes)
  return path.join(iconDir, 'icon.png');
})()
```

### 5.2 Linux Auto-Launch Implementation

**New IPC handler:**
```javascript
// In app.on('ready', ...) after existing auto-launch handlers
if (process.platform === 'linux') {
  // Check current autostart setting
  ipcMain.handle('get-auto-launch', () => {
    const autostartConfig = app.getLoginItemSettings({
      path: '/usr/bin/skales',  // Linux executable path
      args: ['--hidden']
    });
    return autostartConfig.openAtLogin;
  });

  ipcMain.on('set-auto-launch', (_event, enabled) => {
    app.setLoginItemSettings({
      openAtLogin: Boolean(enabled),
      path: process.execPath,
      args: ['--hidden']  // Start minimized to tray
    });
    console.log(`[Skales] Linux auto-launch ${enabled ? 'enabled' : 'disabled'}.`);
  });
}
```

### 5.3 Linux Desktop Entry (XDG)

Electron's `app.setLoginItemSettings` handles XDG autostart automatically on Linux. No manual .desktop file creation needed.

### 5.4 Linux Tray Behavior

**Current tray.js analysis:**
- Uses `Tray` API which is cross-platform compatible
- No changes expected - verify on test

**Potential considerations:**
- Some Linux distros (GNOME) require `libappindicator` for tray icons
- Add as package dependency: `libappindicator1` (deb) / `libappindicator` (rpm)

## 6. Design: package.json Updates

### 6.1 New Build Scripts
```json
{
  "scripts": {
    "build:linux": "npm run bundle:bots && npm run build:web && electron-builder --linux",
    "build:appimage": "npm run bundle:bots && npm run build:web && electron-builder --linux AppImage",
    "build:deb": "npm run bundle:bots && npm run build:web && electron-builder --linux deb",
    "build:rpm": "npm run bundle:bots && npm run build:web && electron-builder --linux rpm",
    "build:all": "npm run bundle:bots && npm run build:web && electron-builder --win --mac --linux"
  }
}
```

### 6.2 Optional Dependencies
```json
{
  "optionalDependencies": {
    "electron-builder-squirrel-windows": "^24.9.0"
  }
}
```

## 7. Icon Requirements

### Current Assets
- `electron/icons/icon.ico` (Windows)
- `electron/icons/icon.icns` (macOS)

### Required for Linux
- `electron/icons/icon.png` (256x256 minimum, 512x512 recommended)

**electron-builder will auto-generate:**
- Multiple PNG sizes from the 512x512 source
- .desktop icon references

## 8. Build Process

### 8.1 Pre-requisites
```bash
# Install Linux build dependencies (Ubuntu/Debian)
sudo apt-get install -y \
  build-essential \
  libgtk-3-0 \
  libnotify4 \
  libnss3 \
  libxss1 \
  libxtst6 \
  xdg-utils \
  libatspi2.0-0 \
  libappindicator1 \
  libsecret-1-0
```

### 8.2 Local Build Command
```bash
# Full build for Linux (all formats)
npm run build:linux

# Output location: dist/
# - Skales-6.2.0.AppImage
# - skales_6.2.0_amd64.deb
# - skales-6.2.0-1.x86_64.rpm
```

## 9. Auto-Update Behavior

### 9.1 Current Configuration
- Uses `electron-updater` with generic provider
- URL: `https://skales.app/updates/`

### 9.2 Linux Behavior
electron-builder generates:
- `latest-linux.yml` for AppImage
- `latest-linux.yml` for deb
- `latest-linux.yml` for rpm

All platforms can share the same update URL since electron-updater detects the platform.

### 9.3 Update Flow
1. App launches → checks `latest.yml` at update URL
2. If newer version found → download in background
3. Notify user → restart to apply

## 10. Testing Checklist

| Test Case | Expected Result |
|-----------|-----------------|
| Run `npm run build:linux` | Builds AppImage, deb, rpm without errors |
| Run built AppImage | Opens Skales window, tray icon visible |
| Install deb on Ubuntu | App appears in Applications menu |
| Toggle auto-launch on Linux | Creates ~/.config/autostart/skales.desktop |
| Test system tray menu | Shows expected menu items |
| Trigger auto-update check | Downloads and applies update |

## 11. Summary of Changes

### Files to Modify
1. **`electron-builder.yml`** - Add Linux configuration section
2. **`package.json`** - Add Linux build scripts
3. **`electron/main.js`** - Add Linux icon path, verify tray/autostart

### Files to Add
1. **`electron/icons/icon.png`** - 512x512 PNG icon

### No Changes Required
- `electron/tray.js` - Cross-platform compatible
- `electron/updater.js` - Already handles Linux

## 12. Implementation Priority

1. **P0 (Critical)** - Add Linux config to electron-builder.yml
2. **P0 (Critical)** - Add icon.png asset
3. **P0 (Critical)** - Add build scripts to package.json
4. **P1 (Important)** - Update icon path in main.js
5. **P2 (Nice to have)** - Test and verify tray behavior

---

**Design Approval**: Pending user review

**Next Step**: Proceed to `/sc:implement` for implementation