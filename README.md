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
| **Aktualna Wersja (Current Version)** | **v1.3.26** |
| **Data Wydania (Release Date)** | 2026-09-02 |
| **Rozmiar Pliku (File Size)** | ~2.13 GB (2,237,054,864 B) |
| **Suma Kontrolna SHA-256** | `5e94d68a28c97e1b48254e51ae702e2cd0a632a79a6ef9bddf90f47673ac9302` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.3.26 (Highlights)

- **Mechaniczne sprzężenie obrotów silnika ze skrzynią biegów (Realistic Gearbox RPM Coupling):**
  - Obroty silnika na biegach 1..6 są mechanicznie zsynchronizowane z prędkością modelu i przełożeniami.
  - Wyścigowy spadek obrotów przy zmianie w górę (o 35–45%) z 260 ms pauzą flat-shift oraz strzałem z wydechu (DSG shift pop / bang).
  - Agresywny międzygaz (rev-match blip) przy redukcji biegów.
- **Hard Cut Rev Limiter & Docięcie Zapłonu (Ignition Cut Pops & Bangs):**
  - Proceduralna synteza fali uderzeniowej spalin i odcięcia iskry na limiterze (odbicie 200 RPM).
  - Podniesione limity: I4 Turbo (8800 RPM), Boxer (8200 RPM).
- **Model Turbosprężarki (Continuous Phase Turbo Spool & Surge Flutter):**
  - Ciągłofazowy dwuharmoniczny gwizd turbiny (1.4–4.8 kHz) oraz efekt upustu zaworu blow-off (HKS surge).
- **Dynamiczne Skalowanie Prędkościomierza OSD:**
  - Radialny łuk prędkościomierza automatycznie skaluje się do teoretycznej prędkości maksymalnej profilu pojazdu (np. 30.8 km/h = 100% tarczy).
  - Wskaźnik optymalnego punktu zmiany biegu (Gear Shift Marker / Notch).
- **Skalibrowany Wskaźnik Gazu i Hamulca (OSD ESC Throttle / Brake):**
  - Pełna zgodność ze standardem 1500 µs (0% spoczynku).

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.3.26:
```
5e94d68a28c97e1b48254e51ae702e2cd0a632a79a6ef9bddf90f47673ac9302
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
