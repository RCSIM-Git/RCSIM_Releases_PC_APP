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
| **Aktualna Wersja (Current Version)** | **v1.3.31** |
| **Data Wydania (Release Date)** | 2026-09-11 |
| **Rozmiar Pliku (File Size)** | ~2.09 GB (2139 MB) (2,242,444,027 B) |
| **Suma Kontrolna SHA-256** | `6e285aa06c181516f509951eb8430cd81e596698e2770beb6e77f22281ff90b0` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.3.31 (Highlights)

- **🌍 Pełna Lokalizacja (100% i18n we wszystkich 8 językach):**
  - Kompletna baza tłumaczeń GUI dla PL, EN, DE, ES, FR, IT, CS, ZH ze skompilowanymi plikami .qm i rygorystycznym testem integralności.
- **💡 Rozszerzona Baza Tooltipów i Pomocy Konfiguracyjnej:**
  - Precyzyjne opisy techniczne parametrów telemetrycznych, filtrów orientacji, modułów AI oraz kart wideo FPV.
- **🏎️ Optymalizacja Mostka SimHub i Pętli Kontrolera:**
  - Płynniejsza konwersja kątów CRSF do SimHub oraz stabilizacja zestawu testów automatycznych stacji naziemnej.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.3.31:
```
6e285aa06c181516f509951eb8430cd81e596698e2770beb6e77f22281ff90b0
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
