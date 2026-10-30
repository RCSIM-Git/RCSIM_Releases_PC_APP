# 🏎️ RCSIM — PC Release Notes & Changelogs

To repozytorium przechowuje historię zmian i notatki wydań aplikacji **RCSIM GCS**. Oficjalny zakup, instalacja i aktualizacje aplikacji PC odbywają się przez Steam.

This repository contains release notes and changelogs for **RCSIM GCS**. Official purchase, installation and updates for the PC app are handled by Steam. This repository does not distribute PC installers.

---

## 📜 Pełna Historia Zmian (Complete Changelogs)

Pełny wykaz wszystkich aktualizacji, zmian technicznych, optymalizacji i naprawionych błędów znajduje się w dedykowanych plikach:

- 🇵🇱 **[CHANGELOG.md (Wersja Polska)](CHANGELOG.md)** — kompletna historia zmian od wersji V1.0.0 do najnowszej.
- 🇬🇧 **[CHANGELOG_EN.md (English Version)](CHANGELOG_EN.md)** — full changelog and release notes in English.

---

## 🚀 Aplikacja PC na Steam (PC App on Steam)

| Parametr | Wartość |
|---|---|
| **Aktualna Wersja (Current Version)** | **v1.4.12** |
| **Data Wydania (Release Date)** | 2026-10-19 |
| **Steam App ID** | `5383000` |
| **Strona aplikacji (Store page)** | [RCSIM na Steam](https://store.steampowered.com/app/5383000/) |

---

Wybierz „Obserwuj” lub kup aplikację na Steam. Steam odpowiada za instalację, weryfikację uprawnień i aktualizacje. To repozytorium nie jest źródłem instalatorów.

Use the Steam store page to wishlist or purchase the app. Steam handles installation, entitlement checks and updates. This repository is not an installer source.

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

## 💻 Wymagania Systemowe (System Requirements)

- **System Operacyjny:** Windows 10 / Windows 11 (64-bit).
- **Pamięć RAM:** Min. 4 GB RAM (zalecane 8 GB+).
- **Procesor:** Intel / AMD 64-bit Dual-Core 2.0 GHz+.
- **Łączność:** Port USB 2.0/3.0 (dla nadajnika ELRS/CRSF lub kabla audio PPM Tier 0) oraz karta Wi-Fi / VPN (Tailscale).
- **Kontrolery:** Bazy kierownic DirectInput (Fanatec, Moza, Logitech, Thrustmaster) lub gamepad Xbox.

---

© 2026 RCSIM Ecosystem. Open-Core SimRacing & Autonomous RC Control Station.
