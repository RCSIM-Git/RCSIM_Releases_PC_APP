# 💻 RCSIM - Ground Control Station (GCS)

Ground Control Station (GCS) for the **RCSIM)** RC racing project. This application provides full control, telemetry visualization, real-time video streaming, and configuration options for the RC vehicle.

---

## 🚀 How to Download & Run

The application is distributed as a pre-compiled standalone package. You do **not** need to install Python or compile the source code!

1. Go to the **[Releases](https://github.com/RCSIM-Git/RCSIM_Releases_PC_APP/releases)** section on the right side of this repository.
2. Download the latest release package (e.g., `RCSIM_GCS_vX.Y.Z.zip`).
3. Extract the downloaded archive to any folder on your computer.
4. Run the application using the executable:
   * **Windows:** Double-click `RCSIM_GCS.exe` (or use the provided startup batch script).

---

## 🛠️ Key Features

* **Real-time Telemetry Visualization (100 Hz):** View live charts of speed, acceleration, wheel steering angles, battery status.
* **Low-Latency Video Streaming with OSD (WebRTC):** Receive real-time video feeds from the USB Grabber/ESP32/Raspberry Pi 5 camera on the vehicle with dynamic overlay HUD.
* **SLAM Mapping & Localization:** Seamless integration with SLAM engines, projecting LiDAR point clouds onto a live 2D map.
* **Motion Platform Integration (SIMHUB):** Supports dynamic motion rigs using low-latency binary packet streaming at 100 Hz.
* **Force Feedback Support (Moza):** Deep integration with Moza FFB wheelbases for realistic simulation driving physics.
* **Built-in 2D Simulator (SITL):** Test autopilot logic, controls, and map creation locally without needing the physical vehicle.

---

## 💻 System Requirements

* **OS:** Windows 10 / 11 (64-bit).
* **Network:** Stable Wi-Fi / VPN connection (such as Tailscale) when communicating with the physical Raspberry Pi RC car.
* **Graphics:** DirectX 11 / OpenGL compatible GPU for smooth GUI and map rendering.

---

## ⚙️ Quick Setup

1. Enter the vehicle's IP address (Raspberry Pi 5/ESP32) and communication ports.
2. Available Execution Modes:
   * **Real World**: Requires the autonomous program to be running on the physical RC vehicle.
   * **SITL**: Fully local simulation mode (no external hardware required).
   * **DonkeyCar Sim**: Integrates with the DonkeyCar simulation game.

---
