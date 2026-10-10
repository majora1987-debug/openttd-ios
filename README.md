# OpenTTD for iOS & Mobile (WebAssembly / PWA)

[![OpenTTD](https://img.shields.io/badge/OpenTTD-master-blue.svg)](https://www.openttd.org)
[![Platform](https://img.shields.io/badge/Platform-iOS%20%7C%20iPadOS%20%7C%20Safari-orange.svg)](https://apple.com)
[![WebAssembly](https://img.shields.io/badge/Built%20with-Emscripten%20WASM-purple.svg)](https://emscripten.org)
[![PWA Ready](https://img.shields.io/badge/PWA-Installable%20%26%20Offline-success.svg)](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)

An optimized mobile and iOS port of **OpenTTD**, compiled to WebAssembly via Emscripten and packaged as an installable Progressive Web App (PWA). Designed specifically for touch ergonomics, notch/Dynamic Island containment, and smooth landscape & portrait play.

🎮 **Live Web App**: [https://majora1987-debug.github.io/openttd-ios/](https://majora1987-debug.github.io/openttd-ios/)

---

## 📱 Quick Start on iPhone & iPad

1. Open **[https://majora1987-debug.github.io/openttd-ios/](https://majora1987-debug.github.io/openttd-ios/)** in **Safari**.
2. Tap the **Share** button (box with an arrow pointing up) in Safari.
3. Tap **Add to Home Screen**, then tap **Add**.
4. Launch **OpenTTD** from your Home Screen for a dedicated, full-screen standalone experience with sound and offline support!

---

## ✨ Features & Mobile Ergonomics

### 1. Safe-Area Containment & No Overlap
- **Hardware Notch & Dynamic Island Aware**: Uses CSS `env(safe-area-inset-*)` so UI elements and game windows never render beneath the camera cutout or Home Indicator.
- **Dedicated Game Canvas Viewport**: In-game windows (e.g. World Generation, Build Railway, vehicle lists) are fully contained within the playable area with guaranteed clearance margins from the docked controls.

### 2. Docked Mobile HUD (Two-Column Landscape & Portrait Bar)
- **Landscape Mode**: Docked on the right side as a **two-column vertical rail** (110px width) perfectly fitting within screen height (~308px total) without scrolling or clipping.
  - **Block 1 (Interaction & Modifiers)**: `[🖱️ R-Clk]` `[⎋ Esc]` / `[Ctrl]` `[⇧ Shift]`
  - **Block 2 (Game Speed & Zoom)**: `[+ Zoom In]` `[− Zoom Out]` / `[⏸ Pause]` `[⏩ Fast Fwd]`
  - **Block 3 (System & Tools)**: `[🧹 Close All]` `[⌨️ Keyboard]` / `[💾 Saves]` `[✕ Collapse]`
- **Portrait Mode**: Docked bottom bar resting above the iOS home indicator.
- **Collapsible HUD**: Tap `✕` to hide controls; a floating **🎮** button lets you restore them at any time (vertically centered in landscape so it never blocks the toolbar or status bar).

### 3. Touch Gestures & Mouse Emulation
- **Tap**: Left-click (select, build, interact).
- **Drag**: Pan viewport or drag-place tracks, roads, and stations.
- **Pinch-to-Zoom**: Two-finger pinch to zoom in/out smoothly.
- **Two-Finger Drag**: Pan the map viewport without placing objects.
- **Long-Press**: Triggers right-click with animated touch ring feedback (cancels placement / closes windows).
- **R-Clk Toggle**: Toggles one-tap right-click mode for rapid tool closing and route deletion.

### 4. Audio Unlocking for iOS Safari
- Includes an audio unlock overlay that safely resumes the Web Audio context on the first user interaction, complying with Apple's autoplay policies.

### 5. Persistent Saves & File Export/Import
- Savegames are stored persistently in IndexedDB via Emscripten's IDBFS.
- Built-in **Savegame Manager** (`💾 Saves` button in the HUD) allows exporting `.sav` files to iOS Files / iCloud Drive and importing `.sav` files back into the game.

### 6. Offline PWA & Anti-Flicker Architecture
- Pre-bundled with free **OpenGFX** and **OpenSFX** assets in the virtual filesystem package.
- **Network-First PWA Service Worker**: Checks for new builds automatically on launch and updates caches while maintaining offline capability.
- **Anti-Blink Dimension Guard**: Eliminates recursive SDL2 canvas recreation loops for stable 60 FPS rendering.

---

## 🛠️ Building from Source

### Prerequisites
- [Emscripten SDK (emsdk)](https://emscripten.org/docs/getting_started/downloads.html)
- CMake 3.16+
- Host compiler (clang or gcc)
- Python 3

### Compiling with CMake and Emscripten
```bash
# 1. Activate emsdk environment
source /path/to/emsdk/emsdk_env.sh

# 2. Configure build
mkdir build && cd build
emcmake cmake .. \
  -DCMAKE_BUILD_TYPE=Release \
  -DOPTION_DEDICATED=OFF \
  -DGLOBAL_DIR="."

# 3. Build WASM package and web shell
emmake make -j$(nproc)
```

The build generates:
- `openttd.html` / `index.html`: The HTML shell with mobile ergonomics HUD.
- `openttd.js` & `openttd.wasm`: Compiled OpenTTD engine.
- `openttd.data`: Pre-bundled OpenGFX graphics, OpenSFX sounds, and game assets.
- `sw.js` & `manifest.json`: Offline PWA service worker and app metadata.

---

## 📄 License & Credits

OpenTTD is licensed under the **GNU General Public License version 2.0 (GPLv2)**. See [COPYING.md](COPYING.md) for full license details.

- Original game by Chris Sawyer.
- OpenTTD is developed by the [OpenTTD team and community](https://www.openttd.org/).
- OpenGFX graphics and OpenSFX audio are licensed under GPLv2 and CC-BY-SA 3.0 respectively.
- Mobile layout, touch controls, and iOS ergonomics adaptations by majora1987.
