# 🏎️ RCSIM — PC Release Notes & Changelogs

> 🌐 **Language / Język:** **English** | [Polski](README_PL.md)

This repository contains official release notes, changelogs, and integrity checksums for the **RCSIM GCS (Ground Control Station)** application for Windows 10 / 11. Official distribution, installation, and automatic updates are handled through Steam.

---

## 📜 Complete Changelogs

Detailed records of all software updates, technical improvements, optimizations, and bug fixes are maintained in dedicated changelog documents:

- 🇬🇧 **[CHANGELOG_EN.md (English Version)](CHANGELOG_EN.md)** — full changelog and release notes in English.
- 🇵🇱 **[CHANGELOG.md (Polish Version)](CHANGELOG.md)** — complete change history from v1.0.0 to the latest release in Polish.

---

## 🚀 PC Application on Steam

| Parameter | Value |
|---|---|
| **Current Version** | **v1.4.12** |
| **Release Date** | 2026-10-19 |
| **Steam App ID** | `5383000` |
| **Steam Store Page** | [RCSIM on Steam](https://store.steampowered.com/app/5383000/) |
| **Official Website** | [https://rcsim.org](https://rcsim.org) |

Wishlist or purchase the application on Steam. Steam handles verified installation, entitlement checks, and automatic delta updates.

---

## 🌟 Highlights in v1.4.12

- **🎮 Force Feedback (FFB) Dependency Integration (PySDL2 & SDL2 DLL):**
  - Bundled `sdl2` and `sdl2dll` binary runtimes into the distribution executable with explicit dependency on `PySDL2==0.9.17`.
  - Added build assertions guaranteeing seamless Force Feedback operation across Direct Drive steering wheel bases.
- **🧭 IMU Device Profiles & MSP Flight Controller Integration:**
  - Implemented configurable IMU sensor profiles, native MSP polling for Betaflight/INAV flight controllers, and optimized FPV minimap rendering with virtual track gates.
- **⚡ Performance & Stability:**
  - Low-latency control loop tuning (100–250 Hz) and enhanced failsafe handling for RC vehicle safety.

---

## 💻 System Requirements

- **Operating System:** Windows 10 / Windows 11 (64-bit).
- **RAM:** Minimum 4 GB RAM (8 GB+ recommended).
- **Processor:** Intel / AMD 64-bit Dual-Core 2.0 GHz+.
- **Connectivity:** USB 2.0/3.0 port (for ExpressLRS/CRSF USB transceiver or Tier 0 3.5mm DSC audio PPM cable), Wi-Fi adapter or VPN (Tailscale).
- **Supported Controllers:** DirectInput steering wheels (Fanatec, Moza, Simagic, Logitech, Thrustmaster) or standard Xbox/XInput gamepads.

---

© 2026 RCSIM Ecosystem. Open-Core SimRacing & Autonomous RC Control Station.
