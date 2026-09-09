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
| **Aktualna Wersja (Current Version)** | **v1.3.29** |
| **Data Wydania (Release Date)** | 2026-09-09 |
| **Rozmiar Pliku (File Size)** | ~2.09 GB (2138 MB) (2,242,154,256 B) |
| **Suma Kontrolna SHA-256** | `409adf38358997078ea7b35675ca0795818e0cf930b09b0cbafe4db3e3425ac7` |
| **Oficjalna Strona WWW** | [https://rcsim.org/download](https://rcsim.org/download) |
| **Bezpośredni Link CDN (R2)** | [setup_RCSIM.exe](https://pub-82a77ffa62bd4ab7a9cbd0b9810b3b99.r2.dev/setup_RCSIM.exe) |

---

## 🌟 Najważniejsze Nowości w v1.3.29 (Highlights)

- **⚡ Zero-Lag Motion Cueing & Dynamiczny Krok Czasowy (dt):**
  - Eliminacja 800 ms opóźnienia fazowego w orientacji pojazdu przy niskich częstotliwościach telemetrii radiowej (12 Hz / 20 Hz) dzięki dynamicznemu przekazywaniu czasu kroku $dt$ do filtrów EKF, Complementary, Madgwick i Mahony.
- **🔄 Adaptacyjny Interpolator IMU & Dead Reckoning:**
  - Płynna ekstrapolacja orientacji i przeciążeń dla platform SimHub i Force Feedback (FFB) z automatycznym wygaszaniem (Decay) i twardym failsafe.
- **⏱️ Optymalizacja Wątku CRSF i Pomiar Latencji:**
  - Redukcja uśpienia pętli szeregowej do 1 ms (eliminacja opóźnień timera Windows) oraz wdrożenie precyzyjnego pomiaru latencji od wejścia pakietu do wysyłki do SimHub.

---

## 🔐 Weryfikacja Integralności (SHA-256 Checksum)

Aby zweryfikować poprawność pobranego pliku `setup_RCSIM.exe` w konsoli PowerShell:

```powershell
Get-FileHash .\setup_RCSIM.exe -Algorithm SHA256
```

Oczekiwany hash dla v1.3.29:
```
409adf38358997078ea7b35675ca0795818e0cf930b09b0cbafe4db3e3425ac7
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
