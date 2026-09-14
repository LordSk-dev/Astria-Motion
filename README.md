<p align="center">
  <img src="motion_logo.png" alt="Astria Motion Logo" width="180">
</p>

<h1 align="center">Astria Motion</h1>

<p align="center">
  <strong>Native, hardware-accelerated live wallpaper engine and terminal manager for Windows 10 & 11.</strong><br>
  ⚡ Zero Chromium &bull; ⚡ Zero Electron &bull; ⚡ Zero WebView Overhead<br>
  Built with C++17, Direct3D 11, and Microsoft Media Foundation.<br>
  Developed by <strong>Varq</strong> (<a href="https://github.com/LordSk-dev">LordSk-dev</a>).
</p>

<p align="center">
  <a href="https://discord.gg/astria"><img src="https://img.shields.io/badge/Discord-Join%20Community-7289DA?style=for-the-badge&logo=discord&logoColor=white" alt="Discord"></a>
  <img src="https://img.shields.io/badge/Version-v1.3.6-007ACC?style=for-the-badge&logo=windows&logoColor=white" alt="Version">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%20%7C%2011-0078D6?style=for-the-badge&logo=windows11&logoColor=white" alt="Platform">
  <img src="https://img.shields.io/badge/License-MIT-44CC11?style=for-the-badge" alt="License">
</p>

---

## 💎 Overview

**Astria Motion** is a ultra-lightweight, keyboard-driven live wallpaper application engineered to replace resource-heavy web/electron-based wallpaper engines. It runs as a detached native Windows process that renders directly to the desktop background behind your icons using the Windows `WorkerW` hierarchy and hardware GPU video decoding.

---

## 📦 Downloads & Pre-Compiled Executables

| Distribution Type | Executable File | Description |
| :--- | :--- | :--- |
| 📦 **Installer Package** | [**`AstriaMotion-Installer.exe`**](AstriaMotion-Installer.exe) | Standard Windows installer with Start Menu & Desktop shortcuts |
| 🚀 **Standalone Portable** | [**`AstriaMotion-Portable.exe`**](AstriaMotion-Portable.exe) | Self-contained single `.exe` file — zero installation required |

> [!TIP]
> Both compiled executables are included directly in this repository for instant download and execution!

---

## 📊 Benchmarks & Resource Footprint

*Measurements taken during active 1080p60 H.264 video wallpaper playback on Windows 11 (Intel Core i7 / NVIDIA RTX 3060):*

| Engine | Technology Stack | Working Set (RAM) | Active GPU Usage | Paused GPU / CPU |
| :--- | :--- | :--- | :--- | :--- |
| **Wallpaper Engine** | Chromium Embedded Framework (CEF) | ~200 – 450 MB | ~5% – 12% | Idle webview overhead |
| **Lively Wallpaper** | .NET 8 / C# + WebView2 | ~180 – 350 MB | ~6% – 15% | Runtime memory retention |
| 🚀 **Astria Motion** | **Native C++17 + Direct3D 11 / MediaEngine** | **~35 – 70 MB** | **< 1% (NVDEC / VCN)** | **0% CPU / 0% GPU** |

* Video frames decode directly into DXGI swap chain surfaces via native hardware decoders (NVDEC, Intel QuickSync, AMD VCN).
* Zero per-frame memory allocations or JavaScript runtime garbage collection pauses.

---

## 🔥 Key Features

- **DirectX 11 Desktop Canvas**: Attaches directly to the Windows `Progman` / `WorkerW` split window layer via Win32 messaging (`0x052C`), placing fluid video rendering behind desktop icons.
- **Hardware Acceleration**: Utilizes Windows Media Foundation (`IMFMediaEngine`) with DXGI device management for direct GPU video frame presentation.
- **Smart Auto-Pause Engine**: Dynamic window occlusion checks monitor foreground processes. Automatically pauses playback when games or apps are maximized, fullscreen, or focused to conserve 100% GPU/CPU power.
- **Multi-Monitor Support**: Spans displays or assigns independent per-monitor wallpapers with individual controls.
- **Terminal UI (TUI)**: Navigation via WASD or arrow keys with real-time ASCII/ANSI previews, search filtering, and configuration menus.
- **System Tray Integration**: Lightweight Win32 system tray notification icon with instant pause/resume, audio muting, auto-pause mode switching, and double-click TUI summoning.
- **Background Autostart**: Single-click registry autostart toggle (`HKCU\Software\Microsoft\Windows\CurrentVersion\Run`).

---

## ⚡ Quick Start & CLI Usage

Launch via Command Prompt, PowerShell, or Windows Terminal:

```cmd
# Launch interactive terminal UI
AstriaMotion-Portable.exe

# Run detached headless engine mode
AstriaMotion-Portable.exe --render
```

### Key Controls
| Key | Action |
| :--- | :--- |
| `↑` / `W` | Move selection up |
| `↓` / `S` | Move selection down |
| `Enter` / `Space` | Select / toggle / apply |
| `Esc` / `Q` | Back / Cancel / Exit |
| `/` | Filter search queries |

---

## 🏗 Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        Astria Motion                        │
│  ┌──────────────────────┐         ┌──────────────────────┐  │
│  │   Interactive TUI    │         │  Tray Menu Listener  │  │
│  │  (WASD / Arrow Keys) │         │   (Shell_NotifyIcon) │  │
│  └──────────┬───────────┘         └──────────┬───────────┘  │
│             │                                │              │
│             ▼                                ▼              │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                   Config & State                      │  │
│  │   (config.json · library.json · Win32 Named Mutex)    │  │
│  └──────────────────────────┬────────────────────────────┘  │
└─────────────────────────────┼───────────────────────────────┘
                              │ IPC / Event Flags
                              ▼
┌─────────────────────────────────────────────────────────────┐
│             astria_motion --render (Detached)               │
│                                                             │
│   Windows Desktop (Progman) ──> Spawn WorkerW Layer         │
│                                           │                 │
│   IMFMediaEngine ──(Hardware Decoded)───> │ DXGI Swapchain  │
│   (NVDEC / QuickSync / VCN)               ▼ Render Output   │
│                                      Behind Desktop Icons   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🌐 Community & Links

- **Discord**: Join our community at [https://discord.gg/astria](https://discord.gg/astria)
- **Developer**: [LordSk-dev on GitHub](https://github.com/LordSk-dev)

---

## 📄 License

Distributed under the [MIT License](LICENSE). Copyright © 2026 Varq (LordSk-dev).
