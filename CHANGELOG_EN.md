# Changelog

All notable changes to the RCSIM project are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
