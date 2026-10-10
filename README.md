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
| **Aktualna Wersja (Current Version)** | **v1.4.09** |
| **Data Wydania (Release Date)** | 2026-10-11 |
| **Rozmiar Pliku (File Size)** | ~2.01 GB (2058 MB) (2,158,439,110 B) |
| **Suma Kontrolna SHA-256** | `5d4ab96a29306e8163762a9ceb4d9cb91b166751bec7a1536305ed12f9c1a8f6` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.4.09 (Highlights)

- **🔓 Wydanie Samodzielne (Standalone / Non-Steam):**
  - Całkowicie uniezależniono instalator pobierany z `rcsim.org` od platformy Steam. Aplikacja uruchamia się bez konieczności instalowania Steam ani zakupu licencji na Steam.
  - Weryfikacja DRM jest uruchamiana wyłącznie w dedykowanych kompilacjach sklepowych (`RCSIM_STEAM_BUILD=1`).
  - Bezpieczny fallback przy starcie zapobiega jakimkolwiek crashom czy oknom błędów licencyjnych.
- **📡 Bezprzewodowy Mostek CRSF Wi-Fi TX Backpack (CRWF v1 Protocol):**
  - Bezpośrednie sterowanie RC przez sieć Wi-Fi i port UDP 8888 do modułów TX Backpack (RadioMaster Nomad / Pocket / MT12) w trybie EdgeTX Master/CRSF.
  - Zabezpieczenie losowym 32-bitowym tokenem sesji, odnawialny 300 ms lease oraz dwukierunkowy odbiór telemetrii CRSF downlink na gnieździe klienta.
- **🔋 Zaawansowane Zarządzanie Akumulatorami & Niezależna Telemetria Baterii:**
  - Konfiguracja pakietów 1S–12S, profile chemii (LiPo, Li-Ion, LiFePO4, NiMH), opcjonalna estymacja naładowania SoC.
  - Niezależne czasy świeżości napięcia i prądu w OSD eliminujące przekłamania przy częściowych pakietach telemetrii.
- **🏁 Race Director, Klasyfikacja i Stabilne Lobby:**
  - Sortowanie według liczby ukończonych okrążeń i łącznego czasu, tryb Hotlap, zatrzymanie zegara sesji na mecie.
  - Odporność na awarie wbudowanego brokera MQTT oraz filtrowanie błędnych pakietów UDP discovery.
- **✨ Odświeżony Ekran Startowy (Startup Window) & Pasek Profilu Kierowcy:**
  - Nowoczesny asynchroniczny splash screen z weryfikacją assetów graficznych i dynamicznym paskiem postępu.
  - Pasek szybkiego wyboru profilu kierowcy, pojazdu i aparatury w kokpicie bez wchodzenia do ustawień.
- **🛡️ Bezpieczeństwo Sprzętowe RP2350 & Poprawki Nawigacji:**
  - Odporne na awarie sterownika USB CDC rozłączanie portu szeregowego, twardy limit czasu zapisu 100 ms.
  - Korekta kąta powrotu RTH (bearing-to-home), obsługa punktów bazowych GPS na równiku/południku zerowym oraz poprawki kafelkowania mapy Mercatora.
- **💼 Model Licencyjny Steam i Sim-Center Commercial Pass:**
  - Przygotowanie na publikację Steam i Steam PC Café, manifesty dla Sim-Center Commercial Pass oraz odseparowane DLC Racing & Multiplayer.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.4.09:
```
5d4ab96a29306e8163762a9ceb4d9cb91b166751bec7a1536305ed12f9c1a8f6
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
