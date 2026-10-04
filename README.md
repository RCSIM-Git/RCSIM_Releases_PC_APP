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
| **Aktualna Wersja (Current Version)** | **v1.4.05** |
| **Data Wydania (Release Date)** | 2026-10-04 |
| **Rozmiar Pliku (File Size)** | ~2.01 GB (2054 MB) (2,154,086,599 B) |
| **Suma Kontrolna SHA-256** | `8fbdd8d2b7b07dd0eb05f084e8fe9f5f18cd9dbff1b4424d9ea8c4c1f5c45097` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.4.05 (Highlights)

- **🏎️ Integracja z Assetto Corsa (Shared Memory & UDP):**
  - Bezpośredni odczyt fizyki, obrotów RPM, prędkości, uślizgu kół i przeciążeń z symulatora Assetto Corsa przez pamięć współdzieloną Windows (`acpmf_physics`, `acpmf_graphics`, `acpmf_static`).
  - Rozbudowano dwukierunkowy mostek telemetryczny `simhub_bridge.py` z automatyczną translacją danych dla platform ruchowych (Motion Cueing) oraz haptyki FFB.
  - Dodano pełną konfigurację portów i parametrów w GUI oraz 100% lokalizacji w 8 oficjalnych językach (PL, EN, DE, ES, FR, IT, CS, ZH).

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.4.05:
```
8fbdd8d2b7b07dd0eb05f084e8fe9f5f18cd9dbff1b4424d9ea8c4c1f5c45097
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
