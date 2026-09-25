# 🏎️ RCSIM — Ground Control Station (GCS) Release Repository & Changelogs

Oficjalne repozytorium wydań, sum kontrolnych oraz pełnej historii zmian oprogramowania **RCSIM GCS (Ground Control Station)** dla komputerów PC (Windows 10 / 11).

Official release repository, integrity checksums, and complete change history for **RCSIM GCS (Ground Control Station)** for Windows 10 / 11.

---

## 📜 Pełna Historia Zmian (Complete Changelogs)

Pełny wykaz wszystkich aktualizacji, zmian technicznych, optymalizacji i naprawionych błędów znajduje się w dedykowanych plikach:

- 🇵🇱 **[CHANGELOG.md (Wersja Polska)](CHANGELOG.md)** — kompletna historia zmian od wersji V1.0.0 do najnowszej.
- 🇬🇧 **[CHANGELOG_EN.md (English Version)](CHANGELOG_EN.md)** — full changelog and release notes in English.

---

## 🚀 Dostępne Wydania (Available Releases)

### 1️⃣ Wersja Stabilna Beta (Zalecana / Monolityczna)
| Parametr | Wartość |
|---|---|
| **Wersja (Version)** | **v1.3.41 Beta** |
| **Data Wydania (Release Date)** | 2026-09-25 |
| **Plik (File)** | `setup_RCSIM.exe` |
| **Rozmiar (Size)** | ~2.01 GB (2063 MB) |
| **Suma Kontrolna SHA-256** | `92aad85165aef2f0850f18eba6d7bba4569648b53e442876c3dd3833c1c813de` |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

### 2️⃣ Wersja Testowa Alfa (Multi-Installer / Wybór Komponentów)
| Parametr | Wartość |
|---|---|
| **Wersja (Version)** | **v1.4.01 AlfaTest** |
| **Data Wydania (Release Date)** | 2026-09-25 |
| **Plik (File)** | `setup_RCSIM_v1.4.01_alpha.exe` |
| **Rozmiar (Size)** | ~2.01 GB (2062.66 MB) |
| **Suma Kontrolna SHA-256** | `833121fba116964f7cabd8a6deb12f57a861b876ab70c38c31caa72b68329be1` |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM_v1.4.01_alpha.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM_v1.4.01_alpha.exe) |

---


## 🌟 Najważniejsze Nowości w v1.4.01 (Highlights)

- **📦 Multi-Installer (Modułowa Instalacja Komponentów):**
  - Wprowadzono modularny instalator Windows Inno Setup z podziałem na profile (`Standard / Sim-Racing`, `Pełne Studio AI`, `Własna instalacja`).
  - Podstawowa instalacja Core jest odchudzona o ciężkie pakiety PyTorcha, umożliwiając błyskawiczny start i minimalne zużycie dysku.
  - Zabezpieczono silnik wizji (Graceful Degradation): aplikacja uruchamia się bezbłędnie bez modułu AI z aktywną klasyczną wizją OpenCV.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.4.01:
```
833121fba116964f7cabd8a6deb12f57a861b876ab70c38c31caa72b68329be1
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
