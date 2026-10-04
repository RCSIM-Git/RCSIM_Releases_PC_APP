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
| **Aktualna Wersja (Current Version)** | **v1.4.06** |
| **Data Wydania (Release Date)** | 2026-10-04 |
| **Rozmiar Pliku (File Size)** | ~2.01 GB (2055 MB) (2,154,920,411 B) |
| **Suma Kontrolna SHA-256** | `8f84c333d5bcb44d52203b281cb03af82d1c73e1039b150a2a00e6abcb2b2485` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.4.06 (Highlights)

- **🏎️ Optymalizacja Wideo USB (MJPG 60 FPS & Zero-Copy BGR888):**
  - Minimalna latencja sterownika DirectShow (`CAP_PROP_BUFFERSIZE = 1`) oraz zerokopiowy renderer FPV (`QImage.Format_BGR888`), oszczędzający ~1.8 ms na klatkę i 372 MB/s pamięci RAM.
  - Wymuszone sprzętowe MJPG w konstruktorze DirectShow eliminujące dławienie do 9.4 FPS na USB 2.0.
  - Standaryzacja formatu kamery w GUI i konfiguracji wyłącznie na MJPG.
- **🔊 Eliminacja Przycinania Dźwięku Silnika (Audio Dropout Fix):**
  - Zwiększenie bufora blokowego audio z 512 do 1024 próbek (~46.4 ms marginesu), likwidujące dropouts pod obciążeniem telemetrią.
  - Zastąpienie kosztownej walidacji Pydantic bezpośrednią propagacją danych w pętli 50 Hz.
- **🏎️ Odblokowanie FFB Simucube & Assetto Corsa:**
  - Naprawiono przepływ telemetrii i odblokowano generowanie efektów FFB na kierownicach DirectInput/Simucube.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.4.06:
```
8f84c333d5bcb44d52203b281cb03af82d1c73e1039b150a2a00e6abcb2b2485
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
