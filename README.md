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
| **Aktualna Wersja (Current Version)** | **v1.4.12** |
| **Data Wydania (Release Date)** | 2026-10-19 |
| **Rozmiar Pliku Pełnego (Full Size)** | ~2.02 GB (2066 MB) (2,166,017,099 B) |
| **Suma Kontrolna SHA-256** | `af17b8ef5e24e2a622b568ffd3fba4bd068764e6b24f1c0ba9cb3ecfc1201390` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (Full)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |
| **Bezpośredni Link CDN (Multi/Alpha)** | [setup_RCSIM_v1.4.12_alpha.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM_v1.4.12_alpha.exe) |
| **Szybka Łatka (Quick Patch)** | [setup_RCSIM_patch.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM_patch.exe) |

---

## 🌟 Najważniejsze Nowości w v1.4.12 (Highlights)

- **🎮 Poprawka Pakowania Zależności FFB (PySDL2 & SDL2 DLL):**
  - Dodano dołączanie modułów `sdl2` oraz `sdl2dll` do EXE oraz jawną zależność `PySDL2==0.9.17`.
  - Wprowadzono asercję przerywającą build przy braku bindingów SDL2, zapewniając bezbłędne działanie Force Feedbacku na kierownicach Direct Drive.
- **🧭 Profile Urządzeń IMU i Integracja MSP FC:**
  - Wdrożono profile urządzeń IMU, bezpośrednie odpytywanie kontrolerów lotu MSP oraz optymalizację minimapy FPV i wirtualnych bramek.
- **⚡ Szybka Łatka (Delta Patch):**
  - Lekki instalator aktualizacyjny dla użytkowników posiadających już RCSIM na PC — zawiera zaktualizowane binaria i biblioteki FFB bez pobierania PyTorcha.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.4.12:
```
af17b8ef5e24e2a622b568ffd3fba4bd068764e6b24f1c0ba9cb3ecfc1201390
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
