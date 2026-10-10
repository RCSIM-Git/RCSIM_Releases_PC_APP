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
| **Aktualna Wersja (Current Version)** | **v1.4.10** |
| **Data Wydania (Release Date)** | 2026-10-11 |
| **Rozmiar Pliku (File Size)** | ~2.01 GB (2059 MB) (2,158,651,792 B) |
| **Suma Kontrolna SHA-256** | `b49ea7c0745cecb183318602b5b80ee3a728280e2d4abd75de1988647c6c315a` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.4.10 (Highlights)

- **🗺️ Poprawka Śladu GPS i Czysty Obraz Minimapy FPV:**
  - Wyeliminowano wypełnianie śladu czarnym wielokątem na widoku FPV (`painter.setBrush(NoBrush)`).
  - Wyłączono nakładanie mleczno-szarej siatki SLAM na mapy OpenStreetMap w trybie GPS.
- **🏁 Pełne Renderowanie Bramek Toru na Minimapie FPV:**
  - Dodano wizualizację bramek wirtualnego toru (`virtual_track`): złote linie start/meta, neonowe sektory, pomarańczowe checkpointy, słupki i strzałki kierunkowe.
- **🔍 Optymalizacja Zbliżenia Minimapy dla Skali RC:**
  - Zwiększono poziom zoomu na postoju do Zoom 19 (dwukrotnie bliższy podgląd w skali mikro-toru).
- **🛠️ Autouzupełnianie w Kreatorze Toru:**
  - Automatyczne generowanie unikalnych ID bramek oraz wymuszenie typu start/meta dla pierwszej bramki toru.
- **🌍 Pełna Lokalizacja (8 języków):**
  - 100% przetłumaczonych fraz we wszystkich 8 oficjalnych językach (PL, EN, DE, ES, FR, IT, CS, ZH) wraz ze zaktualizowanymi plikami `.qm`.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.4.10:
```
b49ea7c0745cecb183318602b5b80ee3a728280e2d4abd75de1988647c6c315a
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
