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
| **Aktualna Wersja (Current Version)** | **v1.3.28** |
| **Data Wydania (Release Date)** | 2026-09-07 |
| **Rozmiar Pliku (File Size)** | ~2.09 GB (2138 MB) (2,242,105,267 B) |
| **Suma Kontrolna SHA-256** | `b6469da41fece62d5420683a61eede2a49f4d3bb452daf64164a6c1a62880063` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.3.28 (Highlights)

- **🌍 Pełna Lokalizacja Konfiguratora ExpressLRS (MSP over CRSF I18n):**
  - Wielojęzyczny interfejs konfiguracji modułów ELRS (nagłówki, statusy, paski postępu, przyciski).
  - Precyzyjne opisy techniczne (ToolTips) w języku angielskim i francuskim dla wszystkich parametrów radiowych (Packet Rate, Gemini Link, Dynamic Power, Fan Threshold).
- **🧭 Domyślna Optymalizacja Silnika Monaco SLAM:**
  - Silnik SLAM jest teraz domyślnie wyłączony przy starcie, co eliminuje zbędne zużycie zasobów CPU i pamięci.
- **🚨 Zabezpieczenie Awaryjne CRSF DISARM & Stop Silnika:**
  - Natychmiastowe zerowanie kanałów do neutralnych 1500 µs (0.0) oraz wymuszenie AUX1 = 1000 µs (DISARM) przy wyłączeniu stacji lub klawiszem SPACJA, odcinające natychmiast napęd pojazdu (Emergency Flush).
- **🎮 Obsługa Baz Fanatec & SC Link (Multi-Axis Mapping & FFB):**
  - 300 ms warmup eliminujący zakłócenia pedałów load cell, filtr `ignored_inputs` oraz obsługa Force Feedback.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.3.28:
```
b6469da41fece62d5420683a61eede2a49f4d3bb452daf64164a6c1a62880063
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
