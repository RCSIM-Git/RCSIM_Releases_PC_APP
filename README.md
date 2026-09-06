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
| **Aktualna Wersja (Current Version)** | **v1.3.27** |
| **Data Wydania (Release Date)** | 2026-09-07 |
| **Rozmiar Pliku (File Size)** | ~616.35 MB (646,288,844 B) |
| **Suma Kontrolna SHA-256** | `9587639e2a3e679acd615ae66a31f7960ea9bb8feea12b8197d6ff2597bd15f1` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.3.27 (Highlights)

- **🚨 Zabezpieczenie Awaryjne CRSF DISARM & Stop Silnika:**
  - Natychmiastowe zerowanie kanałów do neutralnych 1500 µs (0.0) oraz wymuszenie AUX1 = 1000 µs (DISARM) przy wyłączeniu stacji lub klawiszem SPACJA, odcinające natychmiast napęd pojazdu (Emergency Flush).
- **🎮 Obsługa Baz Fanatec & SC Link (Multi-Axis Mapping):**
  - Wyeliminowano blokadę bindowania kolejnych osi w kreatorze dzięki 300 ms okresowi karencji (warmup) oraz filtrowi `ignored_inputs`.
  - Odblokowano i zoptymalizowano DirectInput Force Feedback (FFB) dla kierownic Fanatec w `SDLHapticBackend`.
- **🌍 Lokalizacja i Pełne Tłumaczenia w Kokpicie (I18n & TTS):**
  - Zapewniono dynamiczne tłumaczenie statusu połączenia (`update_connection_status_display()`) i selektora trybów.
  - Wielojęzyczne komunikaty głosowe (TTS) w 5 językach dla zdarzeń ARMED/DISARMED oraz połączenia.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.3.27:
```
9587639e2a3e679acd615ae66a31f7960ea9bb8feea12b8197d6ff2597bd15f1
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
