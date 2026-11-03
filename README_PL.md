# 🏎️ RCSIM — Notatki o Wydaniach i Historia Zmian PC

> 🌐 **Language / Język:** [English](README.md) | **Polski**

To repozytorium przechowuje oficjalną historię zmian, notatki o wydaniach oraz sumy kontrolne oprogramowania stacji naziemnej **RCSIM GCS (Ground Control Station)** dla komputerów PC z systemem Windows 10 / 11. Oficjalny zakup, instalacja i aktualizacje aplikacji PC odbywają się za pośrednictwem platformy Steam.

---

## 📜 Pełna Historia Zmian

Pełny wykaz wszystkich aktualizacji, zmian technicznych, optymalizacji i naprawionych błędów znajduje się w dedykowanych plikach:

- 🇵🇱 **[CHANGELOG.md (Wersja Polska)](CHANGELOG.md)** — kompletna historia zmian od wersji V1.0.0 do najnowszej.
- 🇬🇧 **[CHANGELOG_EN.md (Wersja Angielska)](CHANGELOG_EN.md)** — pełna historia zmian i release notes w języku angielskim.

---

## 🚀 Aplikacja PC na Steam

| Parametr | Wartość |
|---|---|
| **Aktualna Wersja** | **v1.4.12** |
| **Data Wydania** | 2026-10-19 |
| **Steam App ID** | `5383000` |
| **Strona Sklepu Steam** | [RCSIM na Steam](https://store.steampowered.com/app/5383000/) |
| **Oficjalna Strona WWW** | [https://rcsim.org](https://rcsim.org) |

Dodaj aplikację do listy życzeń lub zakup na platformie Steam. Steam odpowiada za instalację, weryfikację uprawnień i automatyczne aktualizacje.

---

## 🌟 Najważniejsze Nowości w v1.4.12

- **🎮 Pakowanie Zależności FFB (PySDL2 & SDL2 DLL):**
  - Dołączono biblioteki wykonawcze `sdl2` oraz `sdl2dll` bezpośrednio do dystrybucji wraz z jawną zależnością `PySDL2==0.9.17`.
  - Wprowadzono asercję gwarantującą niezawodne działanie Force Feedbacku na kierownicach z napędem bezpośrednim (Direct Drive).
- **🧭 Profile Urządzeń IMU i Integracja z Kontrolerami Lotu MSP:**
  - Wdrożono konfigurowalne profile czujników IMU, bezpośrednie odpytywanie kontrolerów lotu MSP (Betaflight/INAV) oraz zoptymalizowano renderowanie minimapy FPV i wirtualnych bramek.
- **⚡ Wydajność i Bezpieczeństwo:**
  - Niskolatencyjna pętla sterowania (100–250 Hz) i zaawansowane procedury failsafe dla ochrony fizycznego modelu RC.

---

## 💻 Wymagania Systemowe

- **System Operacyjny:** Windows 10 / Windows 11 (64-bit).
- **Pamięć RAM:** Minimum 4 GB RAM (zalecane 8 GB lub więcej).
- **Procesor:** Intel / AMD 64-bit Dual-Core 2.0 GHz+.
- **Łączność:** Port USB 2.0/3.0 (dla nadajnika ELRS/CRSF lub kabla audio mini-jack 3.5 mm DSC/PPM Tier 0), karta Wi-Fi lub połączenie VPN (Tailscale).
- **Obsługiwane Kontrolery:** Bazy kierownic DirectInput (Fanatec, Moza, Simagic, Logitech, Thrustmaster) lub standardowe pady Xbox/XInput.

---

© 2026 RCSIM Ecosystem. Open-Core SimRacing & Autonomous RC Control Station.
