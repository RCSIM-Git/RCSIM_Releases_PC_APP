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
| **Aktualna Wersja (Current Version)** | **v1.4.11** |
| **Data Wydania (Release Date)** | 2026-10-12 |
| **Rozmiar Pliku (File Size)** | ~2.01 GB (2060 MB) (2,159,719,391 B) |
| **Suma Kontrolna SHA-256** | `80103a4138093ae4990167924de36390b933cda5f19ecc13be3d821aa1626f17` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.4.11 (Highlights)

- **📈 Zero-Lag Telemetria w Czasie Rzeczywistym (Decoupled Plotter):**
  - Wyeliminowano ~2-sekundowe opóźnienie wykresów w zakładce Real time Telemetry.
  - O(1) buforowanie próbek oraz odseparowany timer renderowania pyqtgraph (30 FPS, QTimer 33ms) z czasem monotonicznym.
- **🧭 Zaawansowane Narzędzie Orientacji Montażu FC 3D (Alignment Tool):**
  - Interaktywny sześcian 3D `FCCubeWidget`, rotacje Roll/Pitch/Yaw, presety, przypisywanie magnetometru i zapis do EEPROM.
- **🔌 Matryca Portów Szeregowych FC (UART Ports Matrix):**
  - Wizualna konfiguracja UART1–UART8 kontrolera lotu (MSP, Serial RX, Telemetria, GPS, VTX) bezpośrednio w GCS.
- **⚡ Natywna Łączność Betaflight / INAV MSP:**
  - Bezpośrednia obsługa protokołu MSP przez USB CDC w Kokpicie i Konfiguracji z pętlą 100 Hz.
- **🌍 Pełna Lokalizacja (8 języków):**
  - 100% przetłumaczonych fraz we wszystkich 8 oficjalnych językach (PL, EN, DE, ES, FR, IT, CS, ZH) wraz ze skompilowanymi plikami `.qm`.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.4.11:
```
80103a4138093ae4990167924de36390b933cda5f19ecc13be3d821aa1626f17
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
