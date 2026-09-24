# 🏎️ RCSIM — Ground Control Station (GCS) Release Repository & Changelogs

Oficjalne repozytorium wydań, sum kontrolnych oraz pełnej historii zmian oprogramowania **RCSIM GCS (Ground Control Station)** dla komputerów PC (Windows 10 / 11).

Official release repository, integrity checksums, and complete change history for **RCSIM GCS (Ground Control Station)** for Windows 10 / 11.

---

## 📜 Pełna Historia Zmian (Complete Changelogs)

Pełny wykaz wszystkich aktualizacji, zmian technicznych, optymalizacji i naprawionych błędów znajduje się w dedykowanych plikach:

- 🇵🇱 **[CHANGELOG.md (Wersja Polska)](CHANGELOG.md)** — kompletna historia zmian od wersji V1.0.0 do najnowszej.
- 🇬🇧 **[CHANGELOG_EN.md (English Version)](CHANGELOG_EN.md)** — full changelog and release notes in English.

---

## 🚀 Pobieranie Najnowszej Wersji (Latest Download)

| Parametr | Wartość |
|---|---|
| **Aktualna Wersja (Current Version)** | **v1.3.36** |
| **Data Wydania (Release Date)** | 2026-09-24 |
| **Rozmiar Pliku (File Size)** | ~2.01 GB (2063 MB) (2,162,862,343 B) |
| **Suma Kontrolna SHA-256** | `443b2ce15b4e2763c188f354e5273528396531578c5017fe146edeb819cfaa77` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.3.36 (Highlights)

- **🏎️ Przestrzenna Tablica Startowa AR w Świecie (World-AR Start Board & HUD Fallback):**
  - Trójwymiarowa bramownica startowa (Gantry Truss) z 5 soczewkami LED F1 ze skalowaniem perspektywicznym i automatycznym fallbackiem na HUD 2D.
- **🏁 Interaktywny Edytor Wirtualnego Toru na Mapie (GPS / SLAM Virtual Track Editor):**
  - Wizualne tworzenie i edycja bramek wirtualnych SLAM/GPS, odwracanie kierunku przejazdu, podgląd widmowy (ghost) i pełne 100% i18n (8 języków).
- **🛡️ PyTorch/CUDA NVML Guard & Współdzielenie OpenGL:**
  - Ochrona przed Access Violation (0xC0000005) na sterownikach CUDA 13 oraz wymuszenie wspólnego API OpenGL dla QWebEngine i FPV.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.3.36:
```
443b2ce15b4e2763c188f354e5273528396531578c5017fe146edeb819cfaa77
```

---

## 💻 Wymagania Systemowe (System Requirements)

- **System Operacyjny:** Windows 10 / Windows 11 (64-bit).
- **Pamięć RAM:** Min. 4 GB RAM (zalecane 8 GB+).
- **Procesor:** Intel / AMD 64-bit Dual-Core 2.0 GHz+.
- **Łączność:** Port USB 2.0/3.0 (dla nadajnika ELRS/CRSF lub kabla audio PPM Tier 0) oraz karta Wi-Fi / VPN (Tailscale).
- **Kontrolery:** Bazy kierownic DirectInput (Fanatec, Moza, Logitech, Thrustmaster) lub gamepad Xbox.

---

© 2026 RCSIM Ecosystem. Open-Core SimRacing & Autonomous RC Control Station.
