# 🏎️ RCSIM — Ground Control Station (GCS) Release Repository & Changelogs

Oficjalne repozytorium wydań, sum kontrolnych oraz pełnej historii zmian oprogramowania **RCSIM GCS (Ground Control Station)** dla komputerów PC (Windows 10 / 11).

Official release repository, integrity checksums, and complete change history for **RCSIM GCS (Ground Control Station)** for Windows 10 / 11.

---

## 📜 Pełna Historia Zmian (Complete Changelogs)

Pełny wykaz wszystkich aktualizacji, zmian technicznych, optymalizacji i naprawionych błędów znajduje się w dedykowanych plikach:

- 🇵🇱 **[CHANGELOG.md (Wersja Polska)](CHANGELOG.md)** — kompletna historia zmian od wersji V1.0.0 do najnowszej.
- 🇬🇧 **[CHANGELOG_EN.md (English Version)](CHANGELOG_EN.md)** — full changelog and release notes in English.

---

## 🚀 Pobieranie Najnowszej Wersji (Latest Downloads — v1.4.03)

Dostępne są dwa warianty instalatora dostosowane do potrzeb użytkownika:

### 1. 🏎️ Wersja Modułowa (Slim Core + 3 Pakiety DLC — Alpha)
Lekki, zoptymalizowany instalator dedykowany do Sim-Racingu, FPV i robotyki. Zawiera 3 wybieralne moduły DLC (`Racing Pro & Multiplayer`, `SLAM Robotics & LiDAR`, `AI Training Studio`) bez gigantycznych bibliotek PyTorch.

| Parametr | Wartość |
|---|---|
| **Wersja (Version)** | **v1.4.03 (Alpha/Modular)** |
| **Data Wydania (Release Date)** | 2026-09-30 |
| **Rozmiar Pliku (File Size)** | **503.68 MB** (528,141,346 B) |
| **Suma Kontrolna SHA-256** | `0f7e918eb07090bff54f6b8142b69c4b47f8bbd4b8b4b3b85b5020dfec3550d6` |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM_v1.4.03_alpha.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM_v1.4.03_alpha.exe) |

---

### 2. 🧠 Wersja Pełna (Full AI & Research Suite)
Kompletne środowisko badawcze zawierające pełny rdzeń, wszystkie 3 moduły DLC oraz wbudowane biblioteki uczenia maszynowego PyTorch, Torchvision i akcelerację CUDA/CPU.

| Parametr | Wartość |
|---|---|
| **Wersja (Version)** | **v1.4.03 (Full Suite)** |
| **Data Wydania (Release Date)** | 2026-09-30 |
| **Rozmiar Pliku (File Size)** | **2.01 GB (2063 MB)** (2,163,218,339 B) |
| **Suma Kontrolna SHA-256** | `3cf238254f8b5d9f2d9debaab6d2e036618d484063db730400a79d8cc4575336` |
| **Główny Link Pobierania (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |
| **Mirror Wydania v1.4.03 (R2)** | [setup_RCSIM_v1.4.03.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM_v1.4.03.exe) |

---

## 🌟 Najważniejsze Nowości w v1.4.03 (Highlights)

- **📦 Ekosystem 3 Pakietów DLC (Modular Architecture):**
  - **DLC 1: Racing Pro & Multiplayer** — lobby sieciowe LAN/Online, synchronizacja pokoi wyścigowych, tablice wyników i precyzyjny pomiar czasu okrążeń.
  - **DLC 2: SLAM Robotics & LiDAR** — mapowanie otoczenia w czasie rzeczywistym 2D BreezySLAM, siatka zajętości terenu oraz nawigacja autonomiczna.
  - **DLC 3: AI Training Studio** — środowisko uczenia ze wzmocnieniem Gymnasium, klonowanie behawioralne modeli jazdy i integracja z DonkeyCar.
- **🎮 Uniwersalny Force Feedback (DirectInput/SDL 6DOF):**
  - Natywna fuzja telemetrii fizycznej IMU modelu RC dla wszystkich baz kierownic Direct Drive (Moza, Fanatec, Simagic, Logitech, Thrustmaster) bez zewnętrznych SDK.
- **📺 Zaawansowane OSD FPV & Telemetria:**
  - Zintegrowane wskaźniki wysokości, wariometru, poboru prądu, zużytych mAh, mocy TX i jakości łącza nakładane bezpośrednio na strumień wideo FPV.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksums)

W konsoli PowerShell:

```powershell
# Wersja Modułowa (Alpha)
Get-FileHash .\setup_RCSIM_v1.4.03_alpha.exe -Algorithm SHA256
# Oczekiwany: 0f7e918eb07090bff54f6b8142b69c4b47f8bbd4b8b4b3b85b5020dfec3550d6

# Wersja Pełna (Full)
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
# Oczekiwany: 3cf238254f8b5d9f2d9debaab6d2e036618d484063db730400a79d8cc4575336
```

---

## 💻 Wymagania Systemowe (System Requirements)

- **System Operacyjny:** Windows 10 / Windows 11 (64-bit).
- **Pamięć RAM:** Min. 4 GB RAM dla wersji Slim Core (zalecane 8-16 GB dla AI Studio).
- **Procesor:** Intel / AMD 64-bit Dual-Core 2.0 GHz+.
- **Łączność:** Port USB 2.0/3.0 (dla nadajnika ELRS/CRSF lub kabla audio PPM Tier 0) oraz karta Wi-Fi / VPN (Tailscale).
- **Kontrolery:** Bazy kierownic DirectInput (Fanatec, Moza, Simagic, Logitech, Thrustmaster) lub gamepad Xbox.

---

© 2026 RCSIM Ecosystem. Open-Core SimRacing & Autonomous RC Control Station.
