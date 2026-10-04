# Changelog

All notable changes to the RCSIM project are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### CI/CD

- GCS packaging includes AHRS resources (including `WMM2025/WMM.COF`). Added full Windows/Linux test discovery and a packaged EXE startup-import/Qt event-loop gate before nightly publication. Full collection exposed 19 legacy test errors; complete suite validation remains pending.

- Restored complete submodule declarations for the PC release application and Radiomaster game scripts. GitHub Actions can now initialize every tracked gitlink instead of failing because `.gitmodules` lacks a URL. GCS now has a separate safe Cloudflare R2 nightly pipeline that publishes only versioned archives under `nightly/`.

## [v1.4.04] - 2026-10-03

### ⚖️ Legal Compliance, License Audit & LGPL Packaging
- **LGPL Compliance in PyInstaller:** Configured `module_collection_mode={'paramiko': 'py', 'pymavlink': 'py'}` in build specifications, ensuring unbundled readable `.py` sources in `_internal/` for LGPL components.
- **Complete Legal Suite in 8 Languages:** Deployed comprehensive EULA and Privacy Policy in 8 languages (PL, EN, DE, ES, FR, IT, CS, ZH), compliance statements, `PYTHON_LICENSES.txt` audit manifest, and full SPDX license texts in installer.
- **Intellectual Property & Repository Hygiene:** Explicitly excluded proprietary vendor SDK binaries from public repository tracking.

### 🔊 Realtime Audio Engine & Sound Synthesis (Combustion & Turbo)
- **Acoustic Combustion Engine Synthesis:** Integrated procedural ICE audio synthesis with dynamic load, RPM, harmonic simulation, and cylinder firing profiles (`combustion_profiles.py`, `realtime_engine.py`).
- **Turbo Blow-Off Valve & Wastegate:** Implemented boost pressure physics model and blow-off valve sound generator upon rapid throttle release (`turbo_blowoff.py`).
- **Induction Layers, Starter & Supercharger Sounds:** Enhanced the audio engine with procedural engine starter acoustics (`engine_starter.py`), intake manifold induction layers (`induction_layers.py`), and mechanical supercharger whine with complete GUI volume adjustment.

### 🧭 CI/CD & AHRS Packaging
- GCS packaging now bundles complete AHRS geomagnetic models (`WMM2025/WMM.COF`).
- Completed 100% translation coverage across all 8 languages (0 unfinished).

## [v1.4.03] - 2026-09-30

### 📦 Modular DLC Architecture & Dual Release

- Introduced dual release channels: Modular Edition (Alfa Multi-Installer ~500 MB) and Full Suite (~2.01 GB).
- Implemented Slim Core architecture with hot-pluggable DLC extensions (`dlc_ai_studio`, `dlc_racing_multiplayer`, `dlc_slam_robotics`).
- Enforced graceful degradation: GCS runs reliably with classical OpenCV vision even without PyTorch installed.

### Disabling AI detection in FPV

- Turning AI Vision off clears previous boxes and lines and rejects late worker results. FPV layers respect Hidden and SSD/OpenCV display modes. The worker checks current detector switches for each frame.

### Restored extension panels

- Loading SLAM Robotics hides the duplicate map tab in Diagnostics. The existing diagnostics map remains available without the extension.

- AI Studio, SLAM Robotics and Multiplayer open their functional panels instead of empty views. Panels load on first selection; loading failures show a message and retry button.
- The lobby shares one MQTT session with application telemetry. SLAM uses the existing service to update its map and performance statistics. Loading messages are translated into eight languages.

### 🖥️ OSD telemetry

- Altitude and vertical-speed instruments use fresh FC measurements. A configurable telemetry widget displays vertical speed, current, consumed capacity, flight mode, link quality, both antenna RSSI readings, TX power, and satellites.
- Aircraft, offroad, and racing presets include additional separate telemetry fields. The editor offers a measurement selector; missing or expired readings display a dash while valid zero readings remain visible. New labels are translated into all eight languages.

### 🏁 QR racing — complete sectors

- The finish line records a lap on a sectorized track only after every configured intermediate gate is detected. The rule supports enum and string marker definitions, while tracks without sectors keep their existing behavior.

### 🧭 SLAM — 0° heading

- SLAM telemetry preserves a valid `0°` top-level heading instead of replacing it with orientation, simulator, or GPS fallback data. Fallbacks are used only when the higher-priority source is absent.

### 🛠️ GCS — Moza H-shifter gear range

- Moza H-shifter codes are bounded to the configured number of forward gears: code `0` selects reverse, `8` neutral, and an excessive or malformed code safely selects neutral instead of creating a phantom gear or interrupting the control loop.

### 📡 Extended CRSF and MAVLink telemetry

- CRSF decodes altitude/vertical speed, barometer pressure and temperature, magnetometer, standard full 6DOF IMU, flight mode, separate voltage groups, and all ten link-statistics fields with TX power in mW. Battery current and consumed capacity survive the complete processing path.
- MAVLink exposes additional altitude, vertical-speed, pressure, battery, and IMU streams plus more GPS fields. Explicit vertical speed takes precedence over differentiated altitude; partial updates do not refresh old measurement ages.
- The inspector preserves a bounded inventory of recent CRSF frame types and MAVLink messages, including types unknown to the decoders, with counters and receipt timestamps. GPS altitude/course mapping and valid zero altitude are corrected without inventing a GPS fix.

### 🛠️ Motion — Thanos AMC and SimHub

- The Thanos watchdog monitors successful packet writes instead of requiring continuous controller replies; incomplete packet writes stop streaming.
- Thanos neutral positions match the midpoint of the configured motion range instead of sending zeros.
- Motion detects stale snapshots using arrival time, `timestamp`, or legacy `sys_time`, and returns to neutral after 500 ms without fresh data.
- The SimHub bridge distinguishes degrees/s from rad/s gyro inputs. Gravity compensation accounts for tilt after identifying the IMU convention from a stationary measurement; ambiguous sources retain the previous behavior.
- SimHub axis speed, scaling, and inversion settings remain unchanged. Software validation: 68 tests passed; physical Thanos AMC verification remains pending.

### 🎮 FFB — shared SDL/DirectInput support

- Attitude-only CRSF telemetry derives angular effects from consecutive actual samples: yaw-motion force and bounded roll/pitch-motion vibration. Angle wrapping and reception gaps are handled; combined force in this mode is capped at 20% before global gain.
- Added a manually triggered FFB test without telemetry (approximately 0.8 s, at most 5% force), preserving saved settings.
- FFB distinguishes missing speed measurements from a measured standstill, so missing GPS no longer suppresses lateral force. CRSF acceleration in g is converted to m/s²; synthetic gravity derived from FC attitude no longer generates false bump effects.
- Shared 6DOF FFB fusion estimates gravity from accelerometer and gyro measurements and removes it from motion forces without requiring GPS or a compass. A quiet initial measurement establishes the mounting baseline; current GPS speed can assist standstill detection.
- Active wheel FFB no longer also triggers Pygame rumble. Unchanged SDL effects avoid redundant per-tick updates while short force leases remain renewed.
- Actual IMU and accelerometer arrival times are tracked independently of battery and GPS packets; queued measurements retain their age. GPS speed expires after fix loss or three seconds without an update.
- Fixed variable Madgwick/Mahony time steps, Mahony tuning, and EKF operation without a magnetometer. MAVLink RAW_IMU exposes all six axes; queue pruning preserves independent IMU/GPS/battery streams, and invalid CRSF lengths no longer stall the receiver.
- SDL/DirectInput is the default and sole steering-force path; manufacturer SDK extensions for inputs, LEDs, and steering range are optional and disabled by default.
- Device capability detection replaces default brand exclusions. Supported effects include gain, constant force, spring, damper, friction, and hardware inertia where available.
- Effects expire after 250 ms without refresh; disabling FFB or receiving invalid force data stops effects. The GUI reports FFB device readiness in all eight languages.

### ⚙️ GCS configuration and telemetry

- The telemetry inspector merges partial packets before refreshing, displays actual processed data, and labels units according to the source. Removed duplicate signal connections and successive update-dropping layers; the CRSF receiver resynchronizes after CRC errors. Logs distinguish receiver, worker, and UI delays.
- Fixed saving and restoring connection mode, video source, USB camera selection, and disconnected COM ports and audio adapters. Custom video URLs stay synchronized between Connection Settings and Video; AUX changes trigger autosave.
- Restored dropdown selectors for connection mode and video source. Added diagnostic settings export with sensitive data redaction.
- The incoming telemetry queue is bounded to 256 packets, removes stale snapshots, and limits worker processing time. PyTorch inference loads only when AI is used.
- Updated telemetry processor tests to the current API and removed global mocks that interfered with other tests.

### 🛠️ GCS — split-axis throttle expo and smoothing
- Split-axis throttle PWM now applies expo and smoothing before the gearbox throttle limit, for both forward and reverse gears.

### 🛡️ GCS — safe PWM switch profiles
- Empty value lists for momentary and latch switches fall back to the configured minimum and maximum PWM values instead of interrupting the control loop with an index error.

### 🛡️ BioPilot — safe virtual-controller axes
- The BioPilot virtual controller rejects `NaN` and infinite values on every flight axis and immediately returns to neutral instead of sending an extreme XInput deflection.

### 🛡️ Telemetry — reliable map-chunk assembly
- The map assembler rejects fragments with an invalid size, index, or data and completes a map only after receiving the full index set. A changed declared map size resets the prior transfer.

### 🛠️ DonkeySim — locale decimal telemetry
- The Unity bridge normalizes locale decimal commas only in numeric JSON object values while preserving valid arrays and text, allowing telemetry from European system locales to be parsed correctly.

### 🛡️ Controls — connected-device isolation
- Action mapping ignores input until its assigned device is confirmed connected and after it disconnects; an axis from another controller cannot take over the action.

### 🛡️ MCS configuration — write consistency
- When atomic replacement of the configuration file fails, the in-memory MCS state remains consistent with the last saved configuration.

### 🛡️ GCS — resilient telemetry buffer
- The telemetry time-series buffer safely ignores malformed nested packet sections instead of interrupting an update on a `null` value.

### 🛠️ MAVLink — real-time telemetry without stale data
- The MAVLink strategy publishes a telemetry snapshot only after new sensor data arrives instead of repeatedly copying the same state every 50 ms.
- When the queue exceeds 50 packets, the GCS retains the five newest `telemetry` snapshots and preserves map and command packets.
- Disconnecting clears received packets so the GUI does not replay stale telemetry after a link loss.
- The map SLAM-layer visibility is persisted as `monaco_slam.slam_layer_visible`; it does not affect the SLAM engine.

### ⚙️ MAVLink FC parameter configuration
- Added an ArduPilot parameter editor available only on an active `MAVLINK_RF` connection: it loads FC values, filters them, and writes individual values after controller confirmation.
- The **RCSIM Rover + mLRS** preset changes only the documented safe parameters (`MAV_GCS_SYSID`, `MAV_OPTIONS`, `RC_OVERRIDE_TIME`, channel mapping, and `MODE_CH`); `SERIALn` port and `MAVx` stream settings remain operator decisions.
- The interface, status messages, and `.qm` files are available in all eight languages.

### 📡 ArduPilot/SIYI telemetry profile
- Added an optional `MAVLINK_RF` profile which, after detecting the FC, requests COMMAND_ACK-confirmed rates of 1 Hz HEARTBEAT, 2 Hz SYS_STATUS, 5 Hz GPS and position, and 10 Hz ATTITUDE.
- The profile is disabled by default. When selected, it replaces the recurring grouped stream request to reduce mLRS and SIYI link load without affecting RC control.

### ⚡ Faster startup
- Startup diagnostics no longer import PyTorch solely to log its version; package presence is checked without loading the module, and AI initializes it on first use.

### 👁️ Safe AI Vision default
- New configurations disable every detection overlay: SSDLite, OWL-ViT, OpenCV lines, and cones. Operators can enable selected features in AI Hub.

## [v1.4.01] - 2026-09-25

### 📦 Multi-Installer (Modular Component Setup)
- Introduced a modular Inno Setup Windows installer with installation profiles (`Standard / Sim-Racing`, `Full AI Studio`, `Custom`).
- The core component (Controls, FFB, FPV, Telemetry, ELRS/CRSF) is separated from heavy PyTorch libraries, dramatically speeding up installation and saving storage for sim-racing users.
- Implemented *Graceful Degradation* architecture in the vision engine (`vision_engine.py`): the application starts seamlessly without PyTorch, retaining classic OpenCV vision and informing users that the optional AI module can be installed via setup.

## [v1.3.41] - 2026-09-25

### 🛡️ Safe CUDA detection and crash prevention on AMD/Intel systems
- Added physical NVIDIA device verification (`cuDeviceGetCount`) via Driver API before probing PyTorch CUDA.
- Prevented the startup Access Violation (`0xc0000005`) crash on PCs with AMD Radeon or Intel GPUs where legacy `nvcuda.dll` remains in System32. AI modules now safely and immediately fall back to CPU mode.

## [v1.3.40] - 2026-09-24

### 🛠️ Qt and ICU compatibility fix
- Removed the incompatible ICU 78 library from the application directory so Windows uses its compatible system library.
- Fixed the `UCNV_TO_U_CALLBACK_SUBSTITUTE` entry-point error that prevented `QtWidgets` from importing.

## [v1.3.39] - 2026-09-24

### 🛡️ Qt startup fix on clean Windows systems
- Removed the conflict between two Microsoft C++ runtime versions bundled by PyInstaller and PySide6.
- `QtWidgets` now loads its matching PySide6 runtime, eliminating the “DLL load failed … specified procedure could not be found” error.

## [v1.3.38] - 2026-09-24

### 🛡️ AMD and Intel startup fix
- The application now checks for the NVIDIA driver (`nvcuda.dll`) before probing CUDA.
- On systems without an NVIDIA GPU, AI modules immediately use CPU mode and do not call `torch.cuda.is_available()`, preventing the startup Access Violation.

## [v1.3.37] - 2026-09-24

### 🏎️ World-AR Start Board & Automatic HUD Fallback (`RaceStartOverlay`)
- **Gantry Truss and 5-LED F1 Start Lights in 3D Space:**
  - Implemented a 3D Start Gantry Truss suspended in world space directly above the Start/Finish gate ($H=2.8\,\text{m}$), bridging the left and right gate posts.
  - Added perspective-scaled 5-LED F1 start board with realistic radial glow for energized lights and green "GO" flash. In classic mode, displays 3D perspective countdown 3–2–1 and "START!".
- **Intelligent Localization Reliability Filter & Seamless 2D HUD Fallback:**
  - Added confidence checks (`_localization_is_reliable`): requires GPS `fix >= 3` and `h_accuracy <= 3.0 m`, or SLAM `active=True` and `confidence >= 0.60`.
  - When localization precision degrades, if no Start/Finish gate is defined, or if the gate is behind the vehicle ($Z \le 0.5\,\text{m}$), the renderer instantly and seamlessly falls back to the clean 2D screen HUD overlay.
- **Direct Track Stream Integration with FPV OSD:**
  - Connected virtual track updates directly to `FPVWindow` and `OSDGraphicsItem`, delivering instant visual sync on video HUD without station restarts.

### 🏁 Interactive Virtual Track Map Editor (GPS / SLAM Virtual Track Editor)
- **Direct Gate Creation and Placement on Map (`VirtualTrackItem` & `MapWidget`):**
  - Implemented an intuitive on-map track editing workflow: operators can click and drag to place gates with explicit crossing direction and specified track width.
  - Added a real-time ghost gate preview (`ghost_gate`) while dragging, displaying left (L) and right (R) posts, width in meters, and directional crossing arrow.
  - Automated gate type assignment: the first gate placed becomes the Start/Finish gate with signature gold styling and checkered pattern, while subsequent gates become sectors (cyan) or checkpoints (orange).
- **Direction, Sequence, and Property Management (`VirtualTrackEditorWidget`):**
  - Added a dedicated map toolbar with toggle edit mode, 180° direction reversal `(⇄)`, and gate reordering `(▲/▼)` in the circuit sequence.
  - Implemented right-click context menu on gates for fast switching between Start/Finish, Sector, and Checkpoint types, reversing traversal direction, or deleting.
- **Streamlined Mission Editor Layout (`MissionEditorWindow`):**
  - Reorganized the creator dialog into two clean tabs: `Virtual Track (Gates / Sectors)` and `Navigation Mission (Waypoints)`.
- **Full 8-Language Localization (100% i18n):**
  - Translated all new UI elements across 8 languages (`pl`, `en`, `de`, `es`, `fr`, `it`, `cs`, `zh`) with 0 unfinished keys.

### 🛡️ PyTorch & CUDA Thread Initialization Guard (Access Violation Fix)
- **Eliminated PyTorch/CUDA Initialization Race in GCSVisionThread:**
  - Shifted `GCSVisionThread` startup to execute after the complete GUI build sequence in `controller_step_initializer.py`. Prevents background PyTorch/Torchvision meta-registration collisions with the main thread during widget creation.
- **Enforced Safe CUDA Availability Probing (NVML Guard):**
  - Set `PYTORCH_NVML_BASED_CUDA_CHECK=1` in `main.py` before runtime modules load, preventing destructive `cuInit` calls inside `torch._C._cuda_getDeviceCount()` and Windows Access Violation (0xC0000005) exceptions on systems with legacy or mismatched CUDA 13.0 drivers.
- **Defensive Vision Model Loading & Fallback Resilience:**
  - Wrapped SSDLite and OWL-ViT model instantiation in `vision_worker.py` and `vision_engine.py` with guarded `try...except` blocks, ensuring that AI model load errors do not block GCS operation or QR/lane tracking.
  - Implemented hardware diagnostics caching (`_cached_hw_status`) in `SmartOwlDetector`.

## [v1.3.35] - 2026-09-23

### 🛡️ GCS Startup Stabilization & Safe Graphics Initialization (OpenGL Safe Fallback)
- **Safe QOpenGLWidget Viewport in FPVWindow:**
  - Implemented safe `QOpenGLWidget` instantiation wrapped in a defensive `try...except` block, preventing native exits/crashes in graphics drivers on PCs lacking full OpenGL support.
  - Automatically falls back to the robust software rasterizer in case of any hardware acceleration failure.
- **Eliminated Early Video Race Condition (Smart Video Guard):**
  - Removed early `manage_stream()` invocation during `MainWindow` initialization, preventing stream startup attempts before application managers are completely wired.
- **Granular GUI Build Diagnostics (Step 4/6 Build Logging):**
  - Added detailed logging markers across all tab and editor initialization steps in `MainWindow` and `controller_step_initializer.py`, ensuring full transparency in `rcsim_gcs.log`.

## [v1.3.34] - 2026-09-22

### 🎨 GUI Polish & Visual Alignment for Tier 2 / Tier 3 (i18n & Combobox Fixes)
- **Resolved Vertical Clipping in QComboBox Dropdown Controls:**
  - Identified and permanently fixed text truncation and baseline shifting in Tier 3 (`ELRS Medium:`, `MAVLink Medium:`) and Tier 2 (`esp_transport_selector`) dropdown selectors.
  - Root cause resolved: stripped unwanted newlines (`\n`) and leading indentation from `<translation>` tags across `.ts` translation files that caused Qt to treat single-line strings as multiline text.
  - Hardened translation generators to prevent whitespace injection inside XML tags.
- **Label Grid Alignment & Nomenclature Consistency:**
  - Right-aligned connection configuration labels (`Qt.AlignmentFlag.AlignRight | Qt.AlignmentFlag.AlignVCenter`) for a uniform, premium layout grid.
  - Refined Tier 2 transport option from ambiguous `"Airlink / Wi-Fi (UDP)"` to clear `"Wi-Fi UDP (Lokalny AP / Router)"`.
- **Complete 100% i18n Coverage (8 Languages):**
  - Added 18 previously missing `ConnectionTab` strings (including `Transmitter Protocol:`, `Transmission Medium:`, `Firmware Version:`).
  - Compiled all 8 `.qm` language binaries (`pl`, `en`, `de`, `es`, `fr`, `it`, `cs`, `zh`) with 0 unfinished entries.

## [v1.3.33] - 2026-09-22

### 🌐 CRSF & MAVLink over TCP/UDP, ESP32 Tier 2 Pro & Steam Compliance
- **Native CRSF & MAVLink over TCP/UDP Transport Layer (Airlink / Wi-Fi Backpack / MicroLink VPN):**
  - Engineered modular `BaseByteTransport` I/O abstraction (`serial_transport.py`, `tcp_transport.py`, `udp_transport.py`).
  - Added low-latency wireless ExpressLRS support (Airlink / Backpack) with `TCP_NODELAY`, internal circular buffer, and auto-reconnect.
  - Implemented direct MAVLink Airport linking (`tcp:IP:PORT` and `udpout:IP:PORT`) in `pymavlink`.
  - Integrated transport medium selectors in GUI (`ConnectionTab`) with dynamic field visibility, Pydantic validation, and 100% 8-language i18n.
- **ESP32 Tier 2 Pro (CRSF Multi-Link Hub & MicroLink Tailscale VPN):**
  - Developed onboard vehicle firmware `ESP32V4_CRSF_MultiLink` with CRC8 DVB-S2 checksums, PCA9685 (I2C 400kHz), IMU, GPS, and ADC1.
  - Integrated MicroLink VPN tunnel (Tailscale / WireGuard) for internet/LTE remote driving without public IP or port forwarding.
  - Created PC USB transmitter dongle `ESP32_CRSF_Dongle_Transmitter` (USB CDC -> ESP-NOW).
- **NanoOWL / OWL-ViT Dual-Engine Architecture (Steam-Safe JIT Cache):**
  - Implemented the `SmartOwlDetector` facade detector in `vision_engine.py` featuring automated runtime hardware probing for NVIDIA CUDA and TensorRT availability.
  - Provided a 100% stable fallback to `OwlVitDetector` (PyTorch CUDA / CPU) for players equipped with AMD Radeon, Intel, or integrated GPUs, preventing binary crashes caused by foreign precompiled `.engine` files on Steam.
  - Implemented a local JIT cache architecture targeting `%LOCALAPPDATA%\RCSIM\models\nanoowl_vit_b16_{sm_arch}.engine` tailored to the end user's specific GPU microarchitecture.
  - Integrated `SmartOwlDetector` into `vision_worker.py` within the FPV live vision loop.
  - Enhanced `build_steam.py` with automated sanitization of non-portable TensorRT binaries, reducing Steam download size by ~183 MB.
- **Application Icon Remaster (Carbon Fiber Multi-Resolution Icon):**
  - Upgraded icon visual fidelity to contemporary racing simulation standards while strictly preserving 100% of brand identity (aerodynamic orange/cyan "R" monogram and brushed chrome "RCSIM" logotype).
  - Designed a chamfered real carbon-fiber outer bezel with 3D edge lighting and high-gloss ceramic lacquer reflections.
  - Replaced solid squircle corner background with an antialiased alpha channel (100% transparency), removing box artifacts on the Windows desktop and Steam library.
  - Generated a 7-layer multi-resolution `.ico` binary (16, 24, 32, 48, 64, 128, 256 px) with Lanczos downsampling and edge-sharpening for crisp taskbar legibility (16px/32px).
  - Updated all target asset files: `app_icon.ico`, `icon/app_icon.ico`, `iconrcsim.png` (Master RGBA), and `app_icon.png`.
- **Steam Legal Compliance & License Audit (Steam Compliance & AI Disclosure):**
  - Conducted a comprehensive technical and legal compliance audit for Steam distribution (`steam_compliance_report.md`).
  - Validated commercial compliance of permissive dependencies (MIT, BSD, Apache-2.0) and weak copyleft (LGPLv3 for PySide6, LGPLv2.1 for pygame-ce, Cairo, GStreamer) through Nuitka standalone directory distribution and re-linking terms in EULA.
  - Updated `THIRD_PARTY_LICENSES_PL.md` and `THIRD_PARTY_LICENSES_EN.md` with explicit SSDLite MobileNetV3 entry and mandatory Microsoft COCO dataset attribution (CC BY 4.0).
  - Formulated the official Steamworks AI Content Disclosure declaration for pre-generated UI instrumentation assets.
- **GUI Hardware Status, Tier 2 Overhaul & Full 100% i18n Localization (8 Languages):**
  - Added an *AI Hardware Acceleration* diagnostics group inside `ai_vision_widget.py` displaying live active backend (TensorRT, PyTorch CUDA, PyTorch CPU) and detected GPU name.
  - Fully redesigned the *Tier 2 (WiFi / Serial ESP32)* panel in *Connection Tab*: added firmware selectors (Tier 2 Pro V4 CRSF Multi-Link vs Classic V1-V3), transport modes (Wi-Fi UDP, MicroLink VPN Tailscale, ESP-NOW Link, Hardware Serial), live FPV MJPEG camera preview, and hardware specification readout (PCA9685 I2C 400kHz, IMU, GPS, battery ADC).
  - Resolved vertical text clipping in Windows `QComboBox` drop-down menus via updated stylesheet sizing (`min-height: 28px` in `dark.qss` and `light.qss`).
  - Throttled 50 Hz missing IMU telemetry warnings in `input_manager.py` (10s backoff) to keep the terminal console clean.
  - Wrapped all user-facing strings in `self.tr(...)`, supplied complete translations across all 8 official languages (PL, EN, DE, ES, FR, IT, CS, ZH), and compiled `.qm` binaries (0 unfinished).
  - Added unit test suites in `test_smart_owl_detector.py` and `test_connection_tab_gui.py` (all tests passed).

## [v1.3.32] - 2026-09-17

### 🚀 Advanced 6-Pillar Diagnostic Suite & Flight Recorder
- **Telemetry Flight Recorder (Circular Ring Buffer):**
  - Implemented zero-allocation 50-event circular ring buffer tracking critical station state changes (ARM/DISARM, Emergency Stop, flight modes, telemetry).
  - Automatically dumps chronological event history to log files right before traceback upon unhandled exceptions in main or background threads.
- **In-GUI Diagnostic Packager & Logs Exporter:**
  - Added *Open logs folder* and *Export report (.zip)* buttons directly inside the *Settings -> Developer* tab.
  - Automatically compiles a Desktop ZIP package containing hardware specs (CPU, RAM, GPU, drivers), package versions, log files (`rcsim_gcs.log`, `crash_handler.log`), and JSON configuration files.
- **Detailed USB/HID Input Device Inventory:**
  - Enhanced device scanning to log full controller capabilities: device name, backend (SDL/DirectInput/Logitech), axis count, button count, hats, unique GUID, and DirectInput FFB readiness.
- **Serial Port Diagnostics & CRSF RX Watchdog:**
  - Added connection parameter logging (baudrate, data bits, parity), a 2.5-second RX silence watchdog warning, and confirmation of the first valid CRSF packet received.
- **FPV Video Capture Device Scanner on Boot:**
  - Automatic inventory of video capture devices and grabbers (`QMediaDevices.videoInputs()`) during startup, logging names, IDs, and formats.
- **Graceful Shutdown Profiling & Thread Audit:**
  - Millisecond-level shutdown timing measurement and active thread auditing (`threading.enumerate()`) detecting non-daemon background threads.
- **Elimination of False-Positive Warnings & Errors:**
  - Missing optional Steamworks module in standalone mode is logged as clean `INFO` instead of `ERROR`.
  - Resolved `Gymnasium Box UserWarning` precision deprecations via explicit float32 casting in simulation environments.
- **Codebase Streamlining & 100% i18n:**
  - Refactored `general_tab.py` well under 500 lines and compiled complete `.qm` translation binaries across all 8 languages (PL, EN, DE, ES, FR, IT, CS, ZH) with 0 unfinished entries.

## [v1.3.31] - 2026-09-11

### 🛠️ Boot Stability & Environmental Hardening (Hotfix)
- **Eliminated Startup Freeze During AI Engine Initialization (CUDA Probe Bypass):**
  - Bypassed unconditional `torch.cuda.is_available()` probing during station boot in `SyncInferenceEngine`. Prevents native C++ Windows Access Violation crashes (`0xC0000005`) on PCs lacking dedicated NVIDIA RTX GPUs or running incompatible drivers. Safe CPU mode is default; GPU availability is checked lazily upon loading model weights.
- **Fixed Map Tile Cache Directory Permissions (Program Files UAC Guard):**
  - Relocated default map tile cache directory (`tile_cache`) for packaged installer builds from `C:\Program Files\RCSIM\tile_cache` to `%LOCALAPPDATA%\RCSIM\tile_cache`, completely eliminating `[WinError 5] Access is denied` errors under standard Windows user accounts.
- **Fault Isolation & Detailed Boot Logging in AppInitializer:**
  - Added comprehensive stage-by-stage logging (`[INIT]`) and wrapped network/autonomous subsystem instantiation (`RaceNetworkManager`, `RaceDirector`, `SlamController`) in defensive `try...except` safety boundaries.
- **Reliable C++ Crash Dump Logging (Faulthandler Crash Log):**
  - Fixed `faulthandler` configuration in windowed GUI mode — low-level C++ faults (SIGSEGV/SIGABRT) are now reliably captured and written to `crash_handler.log` inside the logs directory.
- **64-bit WSAIoctl Compatibility on MAVLink Bridge:**
  - Defined explicit 64-bit `argtypes` and `restype` for system `WSAIoctl` calls on UDP sockets, preventing 64-bit handle truncation and stack corruption.

## [v1.3.30] - 2026-09-11

### 🌍 Full Localization (100% i18n Across All 8 Languages)
- **Complete GUI Translation Coverage (PL, EN, DE, ES, FR, IT, CS, ZH):**
  - All labels, buttons, tooltips, and modal dialogs (IMU calibration, curve editor, AI trainer, telemetry) are translated and compiled to `.qm` binaries.
  - Implemented automated verification test `test_i18n.py` ensuring zero missing or unfinished translations.
  - Expanded comprehensive tooltips for FPV video settings, orientation filters, and autonomous AI modules.
- **SimHub Bridge & Unit Test Optimization:**
  - Refined CRSF angle conversion and telemetry packet dispatch to SimHub UDP socket.
  - Updated automated test suites for communication strategies and SimHub integration.

## [v1.3.29] - 2026-09-09

### ⚡ Zero-Lag Motion Cueing, SimHub & Adaptive IMU Interpolation
- **Dynamic Time Step (dt) in Orientation Filters (Eliminated 800ms Phase Lag):**
  - Resolved vehicle orientation phase lag observed at low radio telemetry rates (12 Hz / 20 Hz via ExpressLRS).
  - All orientation filters (`EKFFilter`, `ComplementaryFilter`, `MadgwickFilter`, `MahonyFilter`) now dynamically accept the live measured monotonic step $dt$, preventing gyro angular undershoot and eliminating slow accelerometer convergence.
- **Fixed Gravity Correction Fusion in Complementary Filter:**
  - Optimized gravity vector correction: replaced destructive delta-slerp blending with weighted error rotation $(1 - \alpha)$, preserving 100% of physical angular motion dynamics.
- **Adaptive IMU Interpolator & Zero-Lag Dead Reckoning:**
  - Implemented `IMUInterpolator` with buffered SLERP for OSD/HUD (smooth 60 FPS) and dead reckoning extrapolation with auto-decay and watchdog failsafe for SimHub motion platforms and Force Feedback.
  - Strict filtering of non-IMU auxiliary packets (AI detections, link statistics), preventing stance jitters and 0.0° zeroing.
- **CRSF Receiver Thread Optimization & Latency Benchmarking:**
  - Lowered serial read sleep from 10 ms to 1 ms, preventing Windows timer quantum latency.
  - Implemented hardware ingress performance timestamping (`_ingress_perf`) measuring end-to-end latency from USB arrival to SimHub UDP socket dispatch.

## [v1.3.28] - 2026-09-07

### 🌍 Localization & ExpressLRS Configurator (MSP over CRSF I18n)
- **Full ExpressLRS Configurator Dialog Localization (EN / PL / FR):**
  - Fully translated ExpressLRS module configurator interface: dialog titles, progress bars, discovery status messages, action buttons, and confirmation modals.
  - Added comprehensive English and French technical tooltips for all RF module settings (*Packet Rate, Telem Ratio, Switch Mode, Antenna Diversity, Gemini Link Mode, Model Match, TX Power, Dynamic Power, Fan Threshold, RF Band*).
  - Implemented an internal localization fallback `tr()` in `ELRSConfiguratorDialog` guaranteeing robust translation across all environments.

### 🧭 Performance & Navigation Engine (Monaco SLAM Default Optimization)
- **Monaco SLAM Engine Disabled by Default (Resource Optimization & Safe Default):**
  - Changed default `use_slam` flag to `False` across Pydantic configuration models and the mapping engine (`mapping_engine.py`).
  - GCS now starts with SLAM disabled by default, eliminating background CPU and memory usage until intentionally enabled by the operator.

## [v1.3.27] - 2026-09-07

### 🚨 Critical Safety & Fail-Safe (CRSF / Direct ELRS Stop Protection)
- **Immediate Motor Disarm & Neutral Throttle Snap (Emergency Stop):**
  - Resolved a critical safety condition in CRSF Direct mode (e.g. RadioMaster Nomad) where models failed to stop upon triggering DISARM.
  - The 100 Hz transmission thread (`crsf_transceiver.py`) immediately resets the `channels_norm` buffer to neutral `1500 µs` (0.0 for throttle and steering) when disarmed (`is_vehicle_armed = False`).
  - Enforced ExpressLRS safety standard: AUX1 (CH5) is forced to `1000 µs` (-1.0 / DISARMED), instantly triggering hardware motor cut-off in the receiver and ESC.
  - Added asynchronous *Emergency Flush* sending the STOP frame immediately to the serial port without waiting for the next 100 Hz cycle.

### 🎮 Hardware Input & Fanatec Wheelbase Compatibility
- **Eliminated Multi-Axis Assignment Lockout in Wizard Dialog:**
  - Introduced a 300 ms initial warmup delay when opening the axis assignment dialog, preventing sticky load-cell noise (e.g., Fanatec brake pedal) from triggering false detection.
  - Implemented an `ignored_inputs` filter in `InputBinder`, allowing smooth sequential assignment of throttle, brake, clutch, and steering on the same device.
  - Implemented robust identifier splitting (`rsplit(":", 2)`) to handle DirectInput controller GUIDs with embedded colons.
  - Moved listener teardown and signal disconnection to `QDialog.done(result)`, eliminating thread leaks and UI lockups after canceling an assignment.
- **Unlocked DirectInput Force Feedback for Fanatec Wheelbases (SDL Haptic):**
  - Updated `SDLHapticBackend` to dynamically enable DirectInput FFB effects for Fanatec wheelbases (constant force, spring, damper, and rumble vibrations).

### 🌍 Localization & Multi-Language Audio Synthesis (Cockpit & TTS)
- **Dynamic Connection Status Display in Cockpit:**
  - Implemented dedicated `update_connection_status_display()` in `CockpitTab`, maintaining translated labels (e.g., `Statut : Connecté`, `Status: Vehicle Connected`) across language switches and port state transitions.
  - Full retranslation for the connection mode selector dropdown in `retranslateUi()`.
- **Multi-Language Speech Synthesis (TTS Audio Prompts):**
  - Extended `NotificationService` with localized audio voice prompts (PL, EN, DE, FR, ES) for ARMED/DISARMED states and telemetry link connection/loss.
  - Updated and compiled full French translation database (`fr.ts`, `fr.qm`, `translations_fr.json`).

## [v1.3.26] - 2026-09-02

### 🏎️ Added & Optimized (Dynamic Soundscape, Rev Limiter & Transmission Physics)
- **Realistic Transmission RPM Coupling & Deep Audible Shift Drops:**
  - Decoupled target RPM from raw throttle so engine revs are mechanically linked to vehicle speed and gear ratios.
  - Implemented race-style up-shift RPM drops (35–45% drop) with a 260 ms flat-shift clutch pause and aggressive exhaust shift bang (DSG pop / shift bang).
  - Added sharp rev-match blips on down-shifts.
- **Hard Cut Rev Limiter & Ignition Cut Pops & Bangs:**
  - Introduced spark-cut backfire pulse in the exhaust (`limiter_pop.wav` & procedural shockwave synthesis) triggered on each 200 RPM bounce cycle of the rev limiter.
  - Increased rev limits for I4 Turbo (8800 RPM, 8500 redline) and Boxer (8200 RPM, 7900 redline).
- **Authentic Turbocharger Model (Continuous Phase Spool & Surge Flutter):**
  - Continuous-phase dual-harmonic turbine whistle (1.4–4.8 kHz) eliminating phase tearing between buffers.
  - Added throttle-responsive induction airflow rush and sequential blow-off valve flutter (HKS surge).
- **Decoupled Audio & RevLEDs from PWM to Physical Throttle Axis:**
  - Mapped audio engine synthesis and OSD Shift-Light LEDs directly to physical gas pedal axis (`0–100%`), removing lower-gear PWM speed governor constraints.

### 🖥️ OSD Cockpit & GUI Enhancements
- **Dynamic Speedometer Scaling Linked to Vehicle Profile V-max:**
  - Speedometer radial arc automatically calibrates 100% of its gauge to the vehicle profile theoretical V-max (e.g. 30.8 km/h = 100% gauge instead of static 200 km/h).
- **Dynamic Gear Shift Marker (Arc Notch):**
  - Renders a cyan notch on the radial arc indicating the speed limit of the current gear (e.g. 6.2 km/h on Gear 1) which illuminates in alert red when the up-shift speed threshold is reached.
- **OSD Throttle & Brake Indicator Calibration:**
  - Implemented automotive two-way ESC standard (1500 µs = 0% throttle / 0% brake) with direct priority reading from `control_states`, fixing the false 50% idle display.
  - Displays positive absolute speed (`abs(speed)`) to eliminate negative values during reverse/drift.

## [v1.3.25] - 2026-08-31

### 🚗 Added & Polished (RCSIM-MCS: Mobile Control Station & CRSF Direct)
- **ExpressLRS / Crossfire Direct Transmitter Driver on Raspberry Pi (Tier 3):**
  - Full implementation of `src/output/crsf.py` driver with cyclic 10 Hz Heartbeat (`0x0B`) for ELRS transmitter modules (e.g. RadioMaster Nomad) connected via direct USB UART (`115200 bps`, pins `Rx:3`, `Tx:1`).
  - Asynchronous reception and decoding of CRSF telemetry (Link Stats `0x14`, Battery voltage `0x08`, IMU `0x86`, GPS `0x02`).
- **Force Feedback & 3D IMU Rumble Vibration Engine (Linux evdev):**
  - Resolved `ENOSPC` (Errno 28) in Linux evdev event loop via proper effect slot persistence (`effect.id`).
  - Implemented 3D IMU Magnitude acceleration algorithm ($\Delta_{\text{dyn}} = |\sqrt{a_x^2 + a_y^2 + a_z^2} - 9.81|$), delivering physical haptic vibration and road bump pulses to gamepads and wheels connected to RPi.
- **Precision Throttle Centering & Hard Neutral Snap:**
  - Corrected deadband scaling in microseconds (`20 µs / 500 µs = 4.0%`), eliminating resting trigger noise on analog triggers (RT/LT) of Xbox controllers.
  - Implemented instant neutral state latching at `1500 µs` without step latency.
  - Seamless throttle recalculation (`_recalculate_throttle`) for smooth gear shifts under full throttle.
- **MCS WebUI Modernization:**
  - Enforced ascending output channel ordering (**CH 1 $\rightarrow$ CH 8**) in mixer tables, channel definitions, and deadzone editors.
  - Updated naming and assignments: CH 1 (Steering), CH 2 (Throttle / ESC Brake), CH 3 (Engine / Throttle), CH 4 (Rudder / Yaw), CH 5–8 (Aux 1–4).
  - Fixed G-Meter crosshair centering (fixed central reticle) and throttle readout in HUD mode.

### 🛠️ Fixed & Improved (Windows Installer & Clean Updates)
- **Automatic Legacy Installation Cleanup (Zero DLL Hell):**
  - Added automatic process termination for active `RCSIM.exe` instances and silent uninstall of prior versions (`unins000.exe /VERYSILENT`) before new deployment.
  - Implemented comprehensive removal of legacy binaries (`_internal`, `*.dll`, `*.pyd`, `*.exe`) in `[InstallDelete]`, eliminating update crashes caused by orphaned PyTorch / CUDA / PySide6 libraries.
- **User Configuration Protection:**
  - User configuration and controller profiles (`rc_config.json`) remain fully safe and untouched in `%LOCALAPPDATA%\RCSIM\config`.

## [v1.3.24] - 2026-08-29

### 🎚️ Added & Optimized (Tier 0 Audio PPM Tuning & EdgeTX MT12 Compatibility)
- **Audio PPM Pulse & Frame Timing Configurability:**
  - Added configurable PPM sync pulse width (`sync_pulse_us`) with an increased default of 500 µs (adjustable from 100 to 1000 µs in the Connection Tab) to prevent signal degradation caused by receiver RC low-pass filters / threshold detection on DSC trainer ports (RadioMaster MT12, EdgeTX, OpenTX).
  - Added configurable PPM frame length (`frame_length_ms`, 18.0–35.0 ms, default 22.5 ms) with dynamic sync gap calculation to prevent channel clipping.
- **AC Diagnostic Mode (Fixed 1500µs Neutral Signal):**
  - Integrated diagnostic toggle button in the Connection Tab to force continuous neutral pulses across all channels, allowing instant verification and isolation of AC-coupling DC bias drift.
- **EdgeTX / MT12 Optimization & Guidelines:**
  - Added in-app guidance for Windows Audio Enhancements deactivation and Positive PPM polarity configuration (`POS` in EdgeTX) for enhanced signal swing (+1.8V vs +0.5V) on AC-coupled sound outputs.
- **Full Internationalization (i18n):**
  - Synchronized and compiled translation catalogs for all 8 supported languages (English, German, Spanish, French, Italian, Czech, Chinese, Polish).

## [v1.3.23] - 2026-08-26

### 🎚️ Fixed & Optimized (Tier 0 Audio PPM Stabilization & Controller Debounce)
- **Audio PPM Frame-Boundary Double Buffering (Zero Phase Tearing):**
  - Eliminated phase tearing in the 100 Hz control loop. Buffer swapping occurs strictly at frame boundaries (`pos == 0`), preventing clipped sync pulses and lost frames on EdgeTX/OpenTX decoders (e.g. RadioMaster MT12).
  - Implemented continuous neutral carrier wave (No Carrier Loss) before arming, eliminating false 'Trainer signal lost' warnings and vibration alerts.
  - Added duplicated Tip + Ring stereo output for 100% electrical compatibility with mono and stereo 3.5mm cables.
- **ARM/DISARM Button Debounce:**
  - Added hardware debounce filter (0.4s) for physical steering wheel/gamepad buttons, preventing repetitive TTS voice alerts.

## [v1.3.22] - 2026-08-25

### 🎚️ Added & Modernized (Tier 0 Audio PPM & Connection Subtabs)
- **Tier 0: Audio PPM Direct (Zero Hardware Mode):**
  - Direct PPM signal generation from PC sound card / DAC output (3.5mm Jack) into RC transmitter DSC/Trainer port without microcontrollers (Arduino / RP2350).
  - Precision PCM waveform synthesizer (`PPMWaveformGenerator`) at 48/96/192 kHz with zero-glitch circular buffering and failsafe digital silence.
- **Hierarchical Connection Subtabs & Responsive Layout:**
  - Reorganized Connection Settings into 7 subtabs (`QTabWidget`) wrapped in frameless `QScrollArea`.
  - Eliminated vertical compression and control overlap across all DPI scalings.

## [v1.3.21] - 2026-08-24

### 🐛 Fixed & Polished
- **Startup Exception Fix (Paramiko Metadata):** Restored necessary `.dist-info` metadata files for `paramiko` and `cryptography` modules, resolving `PackageNotFoundError` during startup on clean Windows installations.

## [v1.3.20] - 2026-08-24

### 🐛 Fixed & Polished
- **About RCSIM Legal Documents Loading:** Fixed loading of legal markdown files (`EULA.md`, `PRIVACY_POLICY.md`, `THIRD_PARTY_LICENSES.md`) in frozen EXE mode with dynamic `_internal` and `docs` path resolution.
- **Startup Stability:** Secured configuration and OSD profile directories in read-only installation folders (`Program Files`).

## [v1.3.19] - 2026-08-23

### 🐛 Fixed & Polished
- **QR Generator Dialog Import:** Resolved missing `QRTriggerPolicy` import that prevented opening track designer and A4 print sheet generator from Multiplayer Lobby.

## [v1.3.18] - 2026-08-20

### 🏁 Added & Modernized (Indoor QR Vision Racing & Virtual Physics)
- **Dedicated Circuit / Indoor QR Vision Racing Mode:**
  - **QR Code Generator & Track Manager (`QRGeneratorDialog`):** Gate configuration tool with Version 1 (7% ECC) optimized high-contrast blocks resistant to FPV camera motion blur.
  - **A4 Print Sheets:** Ready-to-print gate sheets with cut lines and header info (1 large ~18cm code or 2 codes per page).
  - **Track Layout Persistence:** Save and load track configurations to JSON format in `config/tracks/` with full lap history.
- **Configurable QR Gate Trigger Policies:**
  - **Exit Photocell (`PASS_EXIT` - Default):** Lap recorded the instant code leaves the FOV.
  - **First Sighting (`FIRST_SIGHT`):** Immediate trigger upon detection.
  - **Peak Size (`PEAK_SIZE`):** Trigger at maximum bounding box area.
  - **Pit-Stop Station:** Instant confirmation when stopping inside service bay.
- **Virtual Racing Physics & Performance Scaling:**
  - **3 Tire Compounds:** Soft (100% power, 2.0x wear), Medium (-5% power, 1.0x wear), Hard (-10% power, 0.4x wear).
  - **Fuel Mass Impact:** Full tank (-5% power) vs reserve (+5% agility).
  - **Tire Wear Degradation Curve:** Progressive grip loss below 60% and critical drop below 20%.
  - **Dynamic Throttle Modulation:** Real-time vector multiplier `perf_mult` scaling throttle channel.
  - **Pit Lane Limiter:** Automatic power governor capping throttle to 30% inside pit lane.
- **Spectator Live Timing Tower (`SpectatorLeaderboardWindow`):**
  - Fullscreen broadcast window (F1/GT3 timing tower) for venue TVs and projectors with `F11` and MQTT sync.

## [v1.3.17] - 2026-08-19

### 🛸 Added & Modernized
- **Complete Glassmorphism OSD Engine Modernization (9 AAA Themes):**
  - **GT3 Racing (`racing.json`):** Vector speedometer dial, 10-LED Shift-Light bar, gear indicator, `THR / BRK` telemetry bars, live delta lap timer (`▲/▼`), timing tower, 2D friction circle.
  - **Off-Road 4x4 (`offroad.json`):** 3D Pitch/Roll clinometer with degree scale, wheel angle visualizer, altimeter, and OpenStreetMap mini-radar.
  - **FPV Aircraft (`aircraft.json`):** Vector pitch ladder with `━ ▲ ━` crosshair, heading tape ribbon, throttle gauge, home pointer `⌂ HOME`.
  - **Drift Master (`drift.json`):** Arcade drift points `PTS`, `x2.5 COMBO` multiplier, slip angle rail with glow effects.
  - **Cyberpunk HUD (`cyberpunk.json`):** RPi hardware diagnostics (CPU, Temp, RAM), network latency `PING ms`, neural net sparkline reward chart.
  - **AI Vision Navigation (`ai_vision.json`):** Hailo-8 NPU detection confidence, autopilot status, manual override indicator.
  - **SLAM Navigation (`slam.json`):** BreezySLAM robot pose estimation `POSE X/Y/Yaw` and real-time costmap radar.
  - **Night Vision (`nightvision.json`) & Cinematic (`cinematic.json`):** Tactical monochrome NVG HUD and rule-of-thirds 4K recording view.
- **Live OpenStreetMap (OSM) Tiles in OSD Minimap:**
  - Real-time fetching and rendering of OSM map tiles centered on GPS position with compass needle `N ▲`.
- **Consolidated 3D AR Navigation Engine (`ARNavigationWidget`):**
  - 3D spatial projection widget with orthogonal coordinate systems ("camera_relative" vs "waypoint_absolute"), portal gates, pylon gates, and off-screen target pointers.

## [v1.3.16] - 2026-08-14

### 🛸 Added
- **RadioMaster ER5C V2 ExpressLRS Custom Firmware & IMU Telemetry:**
  - **MPU9250 / MPU6050 / MPU6500 Support:** Native I2C inertial sensors on CH4 (SDA) / CH5 (SCL) pins in RadioMaster ER5C V2 receiver with custom ExpressLRS v4.1.0 firmware.
  - **Custom CRSF Frame `0x86` (`CRSF_FRAMETYPE_CUSTOM_IMU`):** High-speed 20 Hz transmission of raw accelerometer, gyro, and magnetometer telemetry to RCSIM GCS.
  - **Direct Force Feedback (FFB) & OSD:** Inertial telemetry driving physical steering wheel FFB effects and artificial horizon OSD for FC-less vehicles.
  - **Heartbeat & Fail-Safe Protection:** Automatic fallback frames ensuring fail-safe protection on sensor dropout.

## [v1.3.15] - 2026-08-12

### 🛸 Added
- **MAVLink UDP Telemetry Forwarding (Mission Planner / QGroundControl / INAV):**
  - **Telemetry Bridge `MAVLinkUDPBridge`:** UDP telemetry streaming on port 14550 for external ground stations.
  - **Automatic 1 Hz Heartbeat:** Identity heartbeat broadcast (Rover, Quadrotor, Boat) for immediate auto-connection.
  - **Supported Packets:** `ATTITUDE`, `SYS_STATUS`, `GPS_RAW_INT`, `GLOBAL_POSITION_INT`, `VFR_HUD`, `SERVO_OUTPUT_RAW`.

### 🛠️ Fixed & Improved
- **USB Camera Multi-Stage Detection Fallback:** Multi-tier probing: Qt `QMediaDevices` -> DirectShow FFMPEG -> OpenCV Index Probing (0..5).
- **Windows Winsock UDP Reset (`SIO_UDP_CONNRESET`):** Disabled `WSAECONNRESET` exceptions on unreachable ICMP ports under Windows.

## [v1.3.14] - 2026-08-12

### 🛠️ Fixed & Improved
- **RP2350 Tier 1 Serial COM Port Fix:** DTR/RTS line toggling, 300ms startup delay on USB CDC opening, and input buffer flushing.
- **QComboBox `findData` Fix:** Accurate COM port matching in connection tabs.
- **D-pad / Multi-Position Switch PWM Signal Fix:** Negative signal auto-inversion handling (`-1.0` to `2000 µs`).

## [v1.3.13] - 2026-08-07

### 🛠️ Fixed & Improved
- **Modal Collision & Startup UX Fix:** Prevented update dialog from colliding with modal EULA and startup splash windows. Delayed update checks until main window rendering is complete.

## [v1.3.12] - 2026-08-07

### 🛠️ Fixed & Improved
- **ArduPilot / Flight Controller Direct Mode Selector:** Direct MAVLink `set_mode_send` selector in Cockpit (MANUAL, STABILIZE, ACRO, FBWA, AUTO, RTL, LOITER, STEERING).

## [v1.3.11] - 2026-08-06

### 🛠️ Fixed & Improved
- **MAVLink RF & Nomad/XR4 mLRS (Tier 3):** Dynamic target routing (`target_system = 1`) and periodic 10 Hz telemetry data stream requests.
- **MAVLink Queue Collapsing & 20Hz Throttling:** Prevented serial buffer overflow on Nomad transmitters.

## [v1.3.10] - 2026-08-02

### 🛠️ Fixed & Improved
- **VisionWorker OpenCV Import Fix:** Fixed missing `cv2` import in `vision_worker.py`.
- **USB Video Multi-Tier Fallback:** Robust camera initialization in FPV view.

## [v1.3.9] - 2026-08-01

### 🛠️ Fixed & Improved
- **Vision Pipeline (BGR/RGB Alignment):** Harmonized color spaces across vision detectors (`ObjectDetector`, `OwlVitDetector`, `ConeDetector`, `LineDetector`).
- **RaceDirector & Multiplayer:** Fixed guest player mission synchronization.
- **Real AI Inference Integration:** Connected `FULL_AI` mode directly to PyTorch neural models via `SyncInferenceEngine`.

## [v1.3.8] - 2026-07-28

### 🏁 Major Milestone: Monaco SLAM v2 & Service Architecture
- **Monaco SLAM v2:** Probability Grid map representation, Submap Manager, and Numba JIT acceleration (40% CPU reduction).
- **Service Architecture:** Modular service structure (`SlamManager`, `TelemetryManager`, `AIManager`, `SignalRouter`, `CoordinateManager`).
- **Configuration:** Complete migration to **Pydantic (RCConfig)** models with GUI parameter validation.

## [v1.3.0] - 2026-05-12

### 🚀 Major Feature: Input & Hardware Abstraction
- **Data-Driven HID Parser:** YAML-based descriptor architecture for custom gamepads, steering wheels, and pedals.
- **Fail-Safe Parsing:** Error-resilient packet parser preventing crashes on corrupted USB packets.

## [v1.2.0] - 2026-02-11

### 🚀 Major Feature: Mission Control & OSD Expansion
- **Mission Control:** Waypoint speed limits, reverse mission routing, and visual map overlays.
- **New OSD Instruments:** G-Force Meter, Steering Visualizer, AI Confidence, Drift HUD, Battery Monitor.

## [v1.1.0] - 2026-02-11

### 🎯 Major Milestone: Autonomous Navigation & Stability
- **Local Planner (V1):** Real-time LiDAR and IMU obstacle avoidance planner.
- **Physical Gearbox Model:** Full vehicle physics synchronization with gearbox profiles.

## [v1.0.0] - 2026-01-14

### 🛠️ Fixed & Added (First Production Release)
- **Deployment:** Dockerized RPi application with system dependencies.
- **WebRTC:** Bidirectional audio/video communication with transceiver lifecycle management.
- **Tailscale:** Automated VPN networking for remote 4G/5G rover operation.

## [v0.6.7] - 2026-01-05
- **Local Planner Debug Widget:** 2D occupancy grid, IMU terrain roughness, and A* path debug visualizer in OSD.

## [v0.6.6] - 2025-12-20
- **Proactive Autonomy:** LiDAR and IMU sensor fusion obstacle avoidance.

## [v0.6.1] - 2025-12-01
- **Multimodal AI:** 17-zone sensory vector fusion with live camera stream. Support for `.tflite` (CPU) and `.hef` (Hailo-8L NPU).

## [v0.6.0] - 2025-11-20
- **Dockerized RPi Application & SimHub Integration:** Python 3.11 containerization and Forza Motorsport UDP bridge for motion rigs.

## [v0.5.1] - 2025-11-15
- **Distributed Multiplayer:** MQTT race coordinator and `RaceDirector`.

## [v0.4.4] - 2025-11-10
- **SimHub Bridge:** Motion platform and bass shaker transducer telemetry stream.

## [v0.4.3] - 2025-11-05
- **Active Safety (ACC) & Ghost Replay:** Adaptive cruise control and optimal lap ghost projection.

## [v0.4.0] - 2025-10-20
- **Extended Kalman Filter (EKF) & Motion Cueing 2.0:** 7-state EKF and Washout / Tilt-Coordination motion filter algorithms.

## [v0.3.0] - 2025-10-10
- **Supervisor Service & Native Drivers:** Lightweight `smbus2` I2C drivers on Linux.

## [v0.2.8] - 2025-09-25
- **OSD Polish:** Racing, Off-Road, and Aviation OSD visual redesign.

## [v0.2.5] - 2025-09-01
- **Global Internationalization (i18n):** Native multi-language support (PL, EN, DE, FR, IT, ES, CS, ZH).

## [v0.1.0] - 2025-05-01
- **The Phoenix MVP:** Initial release with PySide6 multi-threaded architecture and core RC link.
