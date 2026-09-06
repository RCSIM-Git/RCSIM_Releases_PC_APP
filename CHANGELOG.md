# Changelog

All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v1.3.27] - 2026-09-07

### 🚨 Critical Safety & Fail-Safe (CRSF / Direct ELRS Stop Protection)
- **Natychmiastowe Odcięcie Silnika przy DISARM (Emergency Stop & Neutral Snap):**
  - Rozwiązano krytyczny problem braku zatrzymania pojazdu po wyzwoleniu stanu DISARM w protokole CRSF Direct (np. RadioMaster Nomad).
  - Pętla nadawcza 100 Hz (`crsf_transceiver.py`) w stanie wyłączenia stacji (`is_vehicle_armed = False`) natychmiast zeruje bufor `channels_norm` do wartości neutralnych `1500 µs` (wartość 0.0 dla gazu i skrętu).
  - Wymuszenie standardu ExpressLRS dla wyłącznika bezpieczeństwa: kanał AUX1 (CH5) jest automatycznie ustawiany na `1000 µs` (-1.0 / DISARMED), co natychmiast odcina zasilanie silnika w odbiorniku RC i regulatorze ESC.
  - Dodano asynchroniczny *Emergency Flush* – pakiet STOP jest formowany i wysyłany bezpośrednio do portu szeregowego UART bez czekania na kolejny interwał pętli.

### 🎮 Hardware Input & Fanatec Wheelbase Compatibility
- **Eliminacja Blokady Bindowania Kolejnych Osi w Kreatorze (Multi-Axis Binding Fix):**
  - Wprowadzono 300 ms okres karencji (warmup) przy otwieraniu okna dialogowego przypisywania, co eliminuje fałszywe zatrzaskiwanie osi ze stałym szumem lub dryftem (np. load cell pedału hamulca Fanatec).
  - Wdrożono dynamiczny filtr `ignored_inputs` w `InputBinder`, dzięki czemu wcześniej przypisana oś (np. skręt lub gaz) jest ignorowana podczas przypisywania kolejnej funkcji tego samego urządzenia.
  - Zastosowano hermetyczne dzielenie identyfikatorów wejść (`rsplit(":", 2)`), gwarantujące bezbłędne przetwarzanie GUID-ów kontrolerów zawierających znaki dwukropka.
  - Przeniesiono procedurę zwalniania zasobów dialogu do metody `done(result)`, co zapobiega wyciekom wątków i zawieszaniu nasłuchu po anulowaniu operacji klawiszem Esc lub przyciskiem.
- **Odblokowanie Force Feedback dla Baz Fanatec (DirectInput SDL):**
  - W `SDLHapticBackend` dodano dynamiczne odblokowywanie DirectInput FFB dla kierownic marki Fanatec (obsługa stałego oporu, siły sprężystości, tłumienia i wibracji wybojów).

### 🌍 Localization & Multi-Language Audio Synthesis (Cockpit & TTS)
- **Dynamiczne Tłumaczenie Statusu Połączenia w Kokpicie:**
  - Wdrożono dedykowaną metodę `update_connection_status_display()` w `CockpitTab`, która zachowuje pełne tłumaczenie etykiet (np. `Statut : Connecté`, `Status : Połączono z pojazdem`) po zmianie języka oraz przy dynamicznym przełączaniu stanu portu.
  - Zapewniono pełną retranslacja selektora trybów połączenia (`mode_selector`) w `retranslateUi()`.
- **Wielojęzyczne Komunikaty Głosowe (TTS Multi-Language Support):**
  - Zaktualizowano `NotificationService` o wielojęzyczne frazy audio (PL, EN, DE, FR, ES) dla komunikatów uzbrojenia/rozbrojenia (ARMED/DISARMED) oraz nawiązania/utraty połączenia radiowego.
  - Skompilowano i zsynchronizowano bazę tłumaczeń francuskich (`fr.ts`, `fr.qm`, `translations_fr.json`).

## [v1.3.26] - 2026-09-02

### 🏎️ Added & Optimized (Dynamic Soundscape, Rev Limiter & Transmission Physics)
- **Mechaniczne sprzężenie obrotów silnika ze skrzynią biegów (Realistic Gearbox RPM Coupling):**
  - Wyeliminowano natychmiastowe wracanie do obrotów maksymalnych – obroty na biegach 1..6 są teraz mechanicznie powiązane z prędkością modelu i przełożeniem bieżącego biegu.
  - Wdrożono wyścigowy spadek obrotów przy zmianie w górę (o 35–45% zależnie od przełożeń) oraz 260 ms pauzę sprzęgłową (flat-shift cut pause) z soczystym strzałem wydechu (DSG pop / shift bang).
  - Przy redukcji biegu w dół (Down-shift) dodano agresywny międzygaz (rev-match blip).
- **Hard Cut Rev Limiter & Docięcie Zapłonu (Ignition Cut Pops & Bangs):**
  - Wprowadzono procedurę odcięcia zapłonu (spark cut) przy maksymalnych obrotach (`limiter_pop.wav` oraz proceduralna synteza fali uderzeniowej spalin) wyzwalaną na każdym odbiciu 200 RPM limitera.
  - Podniesiono limity obrotów dla profilu I4 Turbo do 8800 RPM (redline 8500 RPM) oraz Boxera do 8200 RPM (redline 7900 RPM).
- **Autentyczny model turbosprężarki (Continuous Phase Turbo Spool & Surge Flutter):**
  - Zaimplementowano dwuharmoniczny świst wirnika (1.4–4.8 kHz) o ciągłej fazie bez trzasków i zaników między buforami audio.
  - Dodano szum zasysania powietrza doładowania (induction rush) rosnący z otwarciem przepustnicy oraz sekwencyjny blow-off z flutterem (HKS surge).
- **Rozdzielenie dźwięku i diod od PWM na fizyczną oś gazu:**
  - Przeniesiono źródło przepustnicy dla syntezy audio i diod Shift-Light / RevLED bezpośrednio na fizyczną oś pedału gazu (`0–100%`), eliminując dławienie dźwięku przez limiter PWM skrzyni na niższych przełożeniach.

### 🖥️ OSD Cockpit & GUI Enhancements
- **Dynamiczne skalowanie prędkościomierza pod V-max modelu:**
  - Tarcza prędkościomierza (radialny łuk) automatycznie dostosowuje swoją pełną skalę do wyliczonego V-max z Profilu Pojazdu (np. 30.8 km/h = 100% tarczy zamiast sztywnego 200 km/h).
- **Dynamiczny znacznik limitu aktualnego biegu na łuku (Gear Shift Marker / Notch):**
  - Na łuku prędkościomierza wyświetlany jest neonowy znacznik cyjanowy wskazujący limit prędkości aktualnego biegu (np. 6.2 km/h na 1. biegu), który po osiągnięciu prędkości zmienia kolor na czerwony ostrzegając o konieczności zmiany biegu (Up-shift).
- **Kalibracja OSD Throttle & Brake Indicator:**
  - Wdrożono samochodowy standard dwukierunkowy ESC (1500 µs = 0% gaz / 0% hamulec) oraz priorytetowy odczyt fizycznych pedałów ze słownika `control_states`, eliminując fałszywe 50% w spoczynku.
  - Prędkościomierz wyświetla bezwzględną wartość prędkości (`abs(speed)`), eliminując wartości ujemne przy cofaniu.

## [v1.3.25] - 2026-08-31

### 🚗 Added & Polished (RCSIM-MCS: Mobile Control Station & CRSF Direct)
- **Moduł Nadawczy ExpressLRS / Crossfire Direct na Raspberry Pi (Tier 3):**
  - Pełna implementacja sterownika `src/output/crsf.py` z cyklicznym pakietem Heartbeat 10 Hz (`0x0B`) dla modułów nadawczych ELRS (np. RadioMaster Nomad) podłączanych bezpośrednio przez USB UART (`115200 bps`, piny `Rx:3`, `Tx:1`).
  - Asynchroniczny odbiór i dekodowanie telemetrii CRSF (Link Stats `0x14`, Napięcie baterii `0x08`, IMU `0x86`, GPS `0x02`).
- **Silnik Fizyki Force Feedback & Wibracji Rumble (Linux evdev):**
  - Rozwiązano błąd `ENOSPC` (Errno 28) w pętli Linux evdev poprzez poprawne zarządzanie slotami efektów (`effect.id`).
  - Zaimplementowano algorytm wypadkowej przeciążeń 3D IMU Magnitude ($\Delta_{\text{dyn}} = |\sqrt{a_x^2 + a_y^2 + a_z^2} - 9.81|$), generujący fizyczne impulsy wibracji i wybojów w padach i kierownicach podłączonych do RPi.
- **Precyzyjne Centrowanie Gazu i Eliminacja Zacinania (Hard Neutral Snap):**
  - Poprawiono przelicznik martwej strefy w µs (`20 µs / 500 µs = 4.0%`), całkowicie eliminując problem szumu spoczynkowego analogowych triggerów (RT/LT) na padach Xbox.
  - Wdrożono natychmiastowe zatrzaskiwanie stanu `1500 µs` w neutrum bez opóźnień krokowych.
  - Płynna zmiana biegów pod pełnym gazem (`_recalculate_throttle`) bez konieczności odpuszczania przepustnicy.
- **Modernizacja Interfejsu WebUI MCS:**
  - Wymuszono rosnący porządek kanałów wyjściowych (**CH 1 $\rightarrow$ CH 8**) w tabeli mikserów, edytorze definicji i martwych strefach.
  - Zaktualizowano nazewnictwo i przypisania: CH 1 (Skręt / Kierownica), CH 2 (Gaz / Hamulec ESC), CH 3 (Przepustnica / Silnik), CH 4 (Kierunek / Yaw), CH 5–8 (Aux 1–4).
  - Poprawiono celownik wskaźnika przeciążeń G-Meter (nieruchomy krzyż nitkowy w centrum) oraz odczyt przepustnicy w trybie HUD.

### 🛠️ Fixed & Improved (Instalator Windows & Czyste Aktualizacje)
- **Automatyczne czyszczenie starej instalacji (Zero DLL Hell):**
  - Dodano automatyczne zamykanie aktywnych procesów `RCSIM.exe` oraz cichą deinstalację poprzedniej wersji (`unins000.exe /VERYSILENT`) przed nałożeniem nowej wersji.
  - Zaimplementowano bezwzględne usuwanie starych plików binarnych (`_internal`, `*.dll`, `*.pyd`, `*.exe`) w sekcji `[InstallDelete]`, co definitywnie eliminuje błędy po aktualizacji spowodowane osieroconymi plikami PyTorch / CUDA / PySide6.
- **Bezpieczeństwo konfiguracji użytkownika:**
  - Konfiguracja i profile `rc_config.json` pozostają w pełni nienaruszone w `%LOCALAPPDATA%\RCSIM\config`.

## [v1.3.24] - 2026-08-29

### 🎚️ Added & Optimized (Tier 0 Audio PPM Tuning & EdgeTX MT12 Compatibility)
- **Audio PPM Pulse & Frame Timing Configurability:**
  - Added configurable PPM sync pulse width (`sync_pulse_us`) with an increased default of 500 µs (adjustable from 100 to 1000 µs in the Connection Tab) to prevent signal degradation caused by receiver RC low-pass filters / threshold detection on DSC trainer ports (RadioMaster MT12, EdgeTX, OpenTX).
  - Added configurable PPM frame length (`frame_length_ms`, 18.0–35.0 ms, default 22.5 ms) with dynamic sync gap calculation to prevent channel clipping.
- **AC Diagnostic Mode (Fixed 1500µs Neutral Signal):**
  - Integrated diagnostic toggle button in the Connection Tab to force continuous neutral pulses across all channels, allowing instant verification and isolation of AC-coupling DC bias drift.
- **EdgeTX / MT12 Optimization & Guidelines:**
  - Added in-app guidance for Windows Audio Enhancements deactivation and Positive PPM polarity configuration (`POS` in EdgeTX) for enhanced signal swing (+1.8V vs +0.5V) on AC-coupled sound outputs.
- **Full Internationalization (i18n):**
  - Synchronized and compiled translation catalogs for all 8 supported languages (English, German, Spanish, French, Italian, Czech, Chinese, Polish).

## [v1.3.23] - 2026-08-26

### 🎚️ Fixed & Optimized (Stabilizacja Tier 0 Audio PPM & Debounce Kontrolera)
- **Audio PPM Frame-Boundary Double Buffering (Zero Phase Tearing):**
  - Wyeliminowano zjawisko rwania fazy fali PPM przy 100 Hz pętli sterowania. Podmiana bufora następuje wyłącznie na granicy ramki (`pos == 0`), zapobiegając ucinaniu szpilek synchronizacyjnych i gubieniu ramek w dekoderach EdgeTX / OpenTX (np. RadioMaster MT12).
  - Wdrożono ciągłą bezpieczną falę nośną (No Carrier Loss) przed uzbrojeniem, zapobiegając fałszywym alarmom utraty sygnału trenera i podwójnym wibracjom aparatury.
  - Dodano podwójne wyjście Tip + Ring (Stereo) dla 100% kompatybilności elektrycznej z kablami mono i stereo.
- **Debounce Przycisku Uzbrojenia (ARM/DISARM):**
  - Dodano sprzętowy filtr drgań styków (debounce 0.4s) dla fizycznych przycisków na kierownicy i gamepadzie, zapobiegając spamowaniu komunikatami głosowymi TTS.

## [v1.3.22] - 2026-08-25

### 🎚️ Added & Modernized (Nowe Funkcje - Tier 0 Audio PPM & Podzakładki Połączeń)
- **Tier 0: Audio PPM Direct (Zero Hardware Mode):**
  - Bezpośrednia generacja sygnału PPM z wyjścia karty dźwiękowej / przetwornika DAC PC (Jack 3.5mm) do portu trenera (DSC) aparatury RC bez konieczności używania mikrokontrolera (Arduino / RP2350).
  - Precyzyjny syntezator fali PCM (`PPMWaveformGenerator`) o próbkowaniu 48 kHz / 96 kHz / 192 kHz z asynchronicznym buforem kołowym (0% glitching) i zabezpieczeniem Failsafe z cyfrową ciszą.
- **Hierarchiczne Podzakładki i Responsywność Zakładki Połączeń (`ConnectionTab`):**
  - Przebudowano układ na 7 podzakładek `QTabWidget` (Tier 0..4, PWM/MAVLink, SimHub/Symulator) opakowanych w bezramkowe `QScrollArea`.
  - Wyeliminowano pionowe ściskanie i nakładanie się kontrolek przy dowolnym skalowaniu DPI oraz dodano automatyczne przełączanie podzakładki przy zmianie trybu w głównym selektorze.

## [v1.3.21] - 2026-08-24

### 🐛 Fixed & Polished (Poprawki Błędów i Stabilności)
- **Naprawa błędu uruchamiania (Paramiko Metadata / Stop na 0%):** Przywrócono niezbędne pliki metadanych `.dist-info` dla modułów `paramiko` oraz `cryptography`, zapobiegając błędowi `PackageNotFoundError: No package metadata was found for paramiko` przy ładowaniu stacji naziemnej na czystych systemach.

## [v1.3.20] - 2026-08-24

### 🐛 Fixed & Polished (Poprawki Błędów i Stabilności)
- **Okno "O programie" (About RCSIM):** Naprawiono ładowanie plików prawnych (`EULA.md`, `PRIVACY_POLICY.md`, `THIRD_PARTY_LICENSES.md`) w skompilowanej aplikacji EXE – dynamiczne wykrywanie ścieżek z katalogów `_internal` oraz `docs`.
- **Stabilność Uruchamiania:** Zabezpieczono dostęp do katalogów konfiguracyjnych i profili OSD w środowiskach z uprawnieniami tylko do odczytu (`Program Files`).

## [v1.3.19] - 2026-08-23

### 🐛 Fixed & Polished (Poprawki Błędów i Stabilności)
- **Naprawa generatora kodów QR (`QRGeneratorDialog`):** Poprawiono brakujący import `QRTriggerPolicy`, który uniemożliwiał otwarcie kreatora toru i generatora arkuszy A4 z poziomu Lobby.

## [v1.3.18] - 2026-08-20

### 🏁 Added & Modernized (Nowe Funkcje - Indoor QR Race & Virtual Physics)
- **Dedykowany Tryb Wyścigowy dla Torów Zamkniętych (Circuit / Indoor QR Vision Racing):**
  - **Generator i Menedżer Kodów QR (`QRGeneratorDialog`):** Narzędzie do konfiguracji bramek toru z generowaniem zoptymalizowanych kodów Version 1 (7% ECC) o dużych blokach odpornych na motion blur kamery FPV.
  - **Arkusze Wydruku A4:** Eksport gotowych do druku arkuszy z nagłówkami i liniami cięcia (wybór: 1 duży kod ~18cm lub 2 kody na stronę).
  - **Zapis i Wczytywanie Układów Tras:** Zapis konfiguracji toru do formatu JSON w `config/tracks/` z pełną historią rekordów.
- **Konfigurowalna Polityka Wyzwalania Bramek Czasowych (QR Trigger Policies):**
  - **Fotokomórka na wyjściu (`PASS_EXIT` - Domyślna):** Zaliczenie czasu następuje w klatce, w której kod znika z kadru (przejechanie linii bramki jak na fotokomórce).
  - **Pierwsza klatka (`FIRST_SIGHT`):** Natychmiastowe zaliczenie w momencie pojawienia się w kadrze.
  - **Punkt najbliższy (`PEAK_SIZE`):** Zaliczenie w punkcie maksymalnego rozmiaru kodu w kadrze.
  - **Stanowisko Pit-Stop:** Natychmiastowe zatwierdzenie postoju po zatrzymaniu się auta przed kodem serwisowym.
- **Wirtualna Fizyka Wyścigowa i Dynamika Modelu (Virtual Physics & Performance Scaling):**
  - **3 Mieszanki Opon:** Soft (100% mocy, 2.0x zużycie), Medium (-5% mocy, 1.0x zużycie), Hard (-10% mocy, 0.4x zużycie).
  - **Wpływ Masy Paliwa:** Pełny bak (-5% mocy) vs pusty bak / rezerwa (+5% zrywności).
  - **Krzywa Degradacji i "Kapcie":** Progresywny spadek mocy i trakcji poniżej 60% oraz krytyczna redukcja do 40-50% poniżej 20% stanu opon.
  - **Dynamiczna Modulacja Przepustnicy:** Wektorowy mnożnik `perf_mult` moduluje sygnał gazu (PWM) w czasie rzeczywistym.
  - **Pit Lane Limiter:** Automatyczny ogranicznik do 30% mocy w alei serwisowej.
- **Dedykowany Widok dla Widowni i Telebimów (`SpectatorLeaderboardWindow`):**
  - Pełnoekranowe okno transmisyjne (Live Timing Tower F1/GT3) dla projektorów i telewizorów na torze z obsługą `F11` i synchronizacją MQTT.
- **Nowy Widget OSD FPV (`TireFuelGaugeWidget`):**
  - Zarys pojazdu z 4 kołami zmieniającymi barwę (zielony/żółty/czerwony), pigułka mieszanki, procentowe zużycie opon, poziom paliwa `⛽ PALIWO` oraz wskaźnik aktualnej mocy silnika `⚡ MOC SILNIKA: XX%`.

## [v1.3.17] - 2026-08-19

### 🛸 Added & Modernized (Nowe Funkcje i Udoskonalenia)
- **Kompletna Modernizacja Silnika OSD (9/9 Motywów Glassmorphism AAA):**
  - **Wyścigowy GT3 (`racing.json`):** Wektorowy zegar prędkościomierza z radialnym obciążeniem, 10-diodowy pasek Shift-Light LED, wskaźnik biegów, telemetria pedałów gazu i hamulca (`THR / BRK`), stoper okrążeń z dynamiczną deltą czasową (`▲/▼`), wieża liderów oraz 2D friction circle przeciążeń.
  - **Terenowy 4x4 (`offroad.json`):** Klinometr 3D przechyłów bocznych i wzdłużnych ze skalą kątów, wskaźnik kąta skrętu kół, wysokościomierz oraz radar OpenStreetMap.
  - **Samolot FPV (`aircraft.json`):** Wektorowa drabinka pochylenia (Pitch Ladder) z celownikiem myśliwca `━ ▲ ━`, wstęga kursu (Heading Ribbon Tape), dedykowana pojedyncza manetka ciągu `THR %` oraz wskaźnik bazy `⌂ HOME`.
  - **Drift Master (`drift.json`):** Licznik punktów poślizgu arcade `PTS`, mnożnik `x2.5 COMBO`, dwukierunkowa szyna kąta poślizgu (Slip Angle) z poświatą aktywnego driftu.
  - **Cyberpunk HUD (`cyberpunk.json`):** Panel diagnostyki RPi (CPU, Temp, RAM), opóźnienie sieciowe `PING ms`, telemetria sieci neuronowej RL (Sparkline chart nagród i wektor akcji) w neonach Cyan/Magenta.
  - **Nawigacja AI Vision (`ai_vision.json`):** Wskaźnik pewności modeli detekcji NPU Hailo-8, status autopilota i przejęcia kontroli `RC Manual Override`, celownik taktyczny.
  - **Nawigacja SLAM (`slam.json`):** Estymacja pozycji i kąta robota `POSE X/Y/Yaw` (BreezySLAM) oraz szklany radar lokalnej mapy kosztów i trajektorii.
  - **Noktowizja NVG (`nightvision.json`) & Filmowy (`cinematic.json`):** Monochromatyczny taktyczny zielony HUD nocny oraz minimalistyczny kadr z siatką trójpodziału do nagrań 4K.
- **Dynamiczne Kafelki OpenStreetMap (OSM) w Minimapie OSD:**
  - Wdrożono pobieranie i renderowanie na żywo kafelków mapy OSM pod pozycją GPS pojazdu z dynamiczną igłą Północy `N ▲`, siatką celownika i filtracją zakłóceń.
- **Konsolidacja Silnika Projekcji 3D AR Navigation (`ARNavigationWidget`):**
  - Uniwersalny widget nawigacji 3D z ortogonalnymi osiami odniesienia ("camera_relative" vs "waypoint_absolute"), bramkami portali, bramkami słupkowymi i wskaźnikami celów poza ekranem.

## [v1.3.16] - 2026-08-14

### 🛸 Added (Nowe Funkcje)
- **RadioMaster ER5C V2 ExpressLRS Custom Firmware & Telemetria IMU:**
  - **Obsługa IMU MPU9250 / MPU6050 / MPU6500:** Dodano natywną obsługę czujników inercyjnych przez magistralę I2C na wyjściach CH4 (SDA) / CH5 (SCL) w odbiorniku RadioMaster ER5C V2 z autorskim oprogramowaniem układowym ExpressLRS v4.1.0.
  - **Nowy Typ Ramki CRSF `0x86` (`CRSF_FRAMETYPE_CUSTOM_IMU`):** Transmisja surowych danych akcelerometru, żyroskopu i magnetometru 20Hz bezpośrednio do stacji RCSIM GCS.
  - **Bezpośredni Force Feedback (FFB) & OSD:** Wykorzystanie odczytów inercyjnych odbiornika do napędzania efektów fizycznych na kierownicy oraz horyzontu OSD w pojazdach bez kontrolera lotu.
  - **Heartbeat & Fail-Safe Protection:** Automatyczne nadawanie ramek sprawdzających $1.0g$ przy zaniku odczytu czujnika.

## [v1.3.15] - 2026-08-12

### 🛸 Added (Nowe Funkcje)
- **MAVLink UDP Telemetry Forwarding (Mission Planner / QGroundControl / INAV):**
  - **Mostek Telemetryczny `MAVLinkUDPBridge`:** Zaimplementowano strumieniowanie ramek telemetrii MAVLink v1/v2 przez UDP (port 14550) dla zewnętrznych stacji naziemnych (Mission Planner, QGroundControl, INAV Configurator).
  - **Automatyczny Heartbeat 1Hz:** Generowanie i wysyłanie ramki `HEARTBEAT` z identyfikacją typu pojazdu (Rover, Quadrotor, Boat) w celu natychmiastowego auto-połączenia.
  - **Strumieniowanie Telemetrii:** Ramki `ATTITUDE`, `SYS_STATUS` (Vbat, Current, Battery %), `GPS_RAW_INT`, `GLOBAL_POSITION_INT`, `VFR_HUD` oraz `SERVO_OUTPUT_RAW` (PWM CH1-CH8).

### 🛠️ Fixed & Improved (Poprawki i Udoskonalenia)
- **USB Camera Detection Multi-Stage Fallback:**
  - Dodano kaskadowe wykrywanie kamer USB: Qt `QMediaDevices` -> FFMPEG DirectShow -> OpenCV Index Probing (0..5).
  - Rozwiązano problem braku wykrywania kamer na komputerach bez zainstalowanego binarnego narzędzia `ffmpeg`.
- **Windows Winsock UDP Reset (`SIO_UDP_CONNRESET`):**
  - Wyłączono rzucanie błędu `WSAECONNRESET` (10054 / 0x80004005) po odebraniu ICMP Port Unreachable w systemie Windows.
- **Normalizacja Polskich Kluczy Telemetrii:**
  - Naprawiono wyciąganie wartości `napięcie_baterii`, `pobór_prądu` i `naładowanie` ze słowników telemetrii RPi.

## [v1.3.14] - 2026-08-12

### 🛠️ Fixed & Improved (Poprawki i Udoskonalenia)
- **RP2350 Tier 1 Serial COM Port Fix:**
  - **Serial Strategy DTR/RTS & Buffer Flushing:** Dodano przełączanie sygnałów DTR/RTS, 300ms zwłoki stabilizującej po otwarciu portu USB CDC oraz czyszczenie bufora wejściowego przed zapisem ramek na RP2350.
  - **QComboBox `findData` Fix:** Naprawiono dopasowanie wybranych portów COM w zakładek konfiguracji połączeń GCS poprzez użycie `findData` zamiast `findText`.
  - **Firmware ARM E-Stop Logic:** Zaktualizowano oprogramowanie RP2350 (`main.cpp`), aby komenda `ARM` bezwarunkowo resetowała flaga awaryjnego zatrzymania `estop_active`.
- **D-pad / HAT & Multi-Position Switch PWM Signal Fix:**
  - **Negative Signal Auto-Invert:** Zaimplementowano automatyczne wykrywanie ujemnego wychylenia surowego (`-1.0` z D-pada DOWN/LEFT) podczas przypisywania wejść w GUI i włączanie opcji *Odwróć (Invert)*.
  - **MultiDirectProcessor Negative Signal Support:** Zaktualizowano procesor przełącznika wielopozycyjnego o obsługę wartości ujemnych (`-1.0`), gwarantując poprawne przełączanie kanałów na **2000us**.
- **Raspberry Pi Tier 4 Title & Translations:**
  - **Poprawka Oznaczenia Poziomu:** Zmieniono opis sekcji na **`Raspberry Pi (General) - Tier 4`** we wszystkich interfejsach oraz skompilowano zaktualizowane pliki tłumaczeń `.qm` dla 9 języków.

## [v1.3.13] - 2026-08-07

### 🛠️ Fixed & Improved (Poprawki i Udoskonalenia)
- **Modal Collision & Startup UX Fix:**
  - **Ochrona Okien Modalnych (EULA vs Aktualizacja):** Naprawiono problem nakładania się okna powiadomień o aktualizacji z oknem EULA / Splash podczas pierwszego uruchomienia.
  - **Opóźnione Sprawdzanie Wersji (`showEvent`):** Przeniesiono inicjalizację testu aktualizacji na zdarzenie wyrenderowania głównego okna oraz wdrożono strażnika `QApplication.activeModalWidget()`, który odkłada wyświetlenie okna aktualizacji do czasu zamknięcia wszystkich dialogów startowych.

## [v1.3.12] - 2026-08-07

### 🛠️ Fixed & Improved (Poprawki i Udoskonalenia)
- **ArduPilot / FC Direct Flight Mode Control:**
  - **Przełącznik Trybów FC w Kokpicie:** Zaimplementowano selektor trybów pracy kontrolera lotu (MANUAL, STABILIZE, ACRO, FBWA, AUTO, RTL, LOITER, STEERING) w zakładce Kokpitu, przesyłający bezpośrednie ramki MAVLink `set_mode_send` oraz `MAV_CMD_DO_SET_MODE`.
  - **Domyślne Wymuszenie Trybu MANUAL:** Zaktualizowano domyślną wartość kanału `aux_ch5_neutral` na 1100µs w `connection.json`, zapewniając automatyczne przełączanie FC na czysty trym ręczny bez ingerencji żyroskopów (IMU).
  - **Dynamiczna Widoczność:** Dedykowany selektor trybów FC wyświetla się w interfejsie GCS wyłącznie podczas korzystania z protokołu MAVLink.

## [v1.3.11] - 2026-08-06

### 🛠️ Fixed & Improved (Poprawki i Udoskonalenia)
- **MAVLink RF & Nomad/XR4 mLRS (Tier 3):**
  - **Dynamic Target Routing:** Naprawiono automatyczne adresowanie pakietów sterowania `RC_CHANNELS_OVERRIDE` oraz komend `ARM` do `target_system = 1` (ArduPilot FC) po wykryciu odbiornika mLRS (`SysID 51`).
  - **Auto Telemetry Request (`REQUEST_DATA_STREAM`):** Zaimplementowano okresowe wysyłanie ramki `request_data_stream_send` (10Hz) w `MAVLinkRFStrategy`, aktywując przesyłanie pełnego zestawu 26 pakietów telemetrii z ArduPilot na portach pomocniczych.
  - **Queue Collapsing & 20Hz Throttling:** Wdrożono zwijanie kolejki wyjściowej sterowania oraz dławienie transmisji MAVLink do 20Hz (max co 50ms), eliminując przepełnienia bufora szeregowego i miganie diody na nadajniku Nomad.
  - **Safety & PreArm Documentation:** Dodano kompletną instrukcję bypassu Failsafe (`THR_FAILSAFE = 0`) oraz `ARMING_CHECK` dla stacji GCS bez fizycznego odbiornika RC.

## [v1.3.10] - 2026-08-02

### 🛠️ Fixed & Improved (Poprawki i Udoskonalenia)
- **VisionWorker OpenCV Import Fix:** Dodano brakujący import `cv2` w `vision_worker.py`, usuwając błąd pętli detekcji wizyjnej `VisionWorker Inference Error: name 'cv2' is not defined`.
- **USB Video Multi-Tier Fallback:** Wdrożono 3-stopniowy mechanizm awaryjny inicjalizacji kamer USB w widoku FPV (`video_thread.py`), zapobiegający awarii podglądu w przypadku braku obsługi specyficznego kodka/klatkażu przez sterownik DirectShow.

## [v1.3.9] - 2026-08-01

### 🛠️ Fixed & Improved (Poprawki i Udoskonalenia)
- **Vision Pipeline (BGR/RGB Alignment):** Ujednolicono obsługę kanałów kolorów dla detektorów wizyjnych (`ObjectDetector`, `OwlVitDetector`, `ConeDetector`, `LineDetector`) eliminując błędy odczytu palety HSV i klasyfikacji obiektów na surowym strumieniu wideo BGR.
- **RaceDirector & Multiplayer:** Naprawiono brakujące odwołanie do `MissionManager` w `RaceDirector`, umożliwiając poprawne odbieranie i rozpoczynanie misji tras wyścigowych u graczy dołączających jako goście (Guest).
- **Communication & Safety Defaults:** Zaktualizowano domyślne poziomy neutralne dla kanałów pomocniczych AUX (3-8) oraz rozszerzono testowanie menedżera geofence.
- **Gearbox Debounce Configuration:** Przeniesiono parametr opóźnienia zmiany biegów (`gear_change_debounce`) do modelu `GearboxConfig` oraz dodano płynną kontrolę i regulację w GUI (`VehicleGearboxWidget`), chroniąc przed przypadkową podwójną zmianą biegów przy kliknięciu łopatek.
- **SanityChecker Speed Limit:** Przeniesiono hardkodowany dotąd limit "niemożliwej" prędkości (15 m/s) do konfigurowalnego parametru `max_speed` w `RacingConfig`, eliminując fałszywe alarmy `IMPOSSIBLE SPEED` przy szybszych modelach RC.
- **PilotService Graceful Shutdown:** Poprawiono kolejność zamykania silników AI w `pilot_service.py` (`stop_event.set()` -> `join(timeout=2.0)` -> `terminate()` w ostateczności), umożliwiając czyszczenie zasobów i bezpieczny zapis checkpointów przed ubiciem procesu.
- **Non-blocking AI Frame Queue:** Zastąpiono blokujące `queue_frames.put()` bezblokującym `queue_frames.put_nowait()` z cichym odrzucaniem nadmiarowych klatek w pętlach silników AI (`pilot_engines.py`, `training_engines.py`), chroniąc pętlę sterowania pojazdu przed zamrożeniem z powodu zatorów w GUI.
- **Real AI Assist & Full AI Inference Integration:** Zaimplementowano dedykowane gałęzie dla trybów `FULL_AI` oraz `AI_ASSIST` w `SlamController.compute()`, wspierane przez nowy moduł `model_loader.py` oraz synchroniczny silnik `SyncInferenceEngine`. Podłączono przycisk w zakładce `7. Full AI` oraz selektor w Cockpit (`AI_FULL`) bezpośrednio do trybu `FULL_AI`, aktywując rzeczywistą inferencję sieci PyTorch zamiast algorytmu RACING.
- **UI Progress Styling:** Zmodernizowano stylizację paska pobierania aktualizacji (`QProgressBar`) w oknie `UpdateDialog` na gładki, ciągły pasek z gradientem niebieskim.

## [v1.3.8] - 2026-07-28

### 🏁 Major Milestone: Monaco SLAM v2 & Service Architecture
To wydanie konsoliduje system RCSIM jako stabilną platformę "Race-Ready", wprowadzając drugą generację silnika SLAM oraz pełną modularność GCS.

### ✨ Added (Nowości)
- **Monaco SLAM v2:**
    - **Probability Grid:** Nowy model reprezentacji mapy zapewniający płynne przejścia i lepszą obsługę niepewności pomiarowej.
    - **Submap Manager:** Architektura oparta na submapach, eliminująca dryft globalny i umożliwiająca optymalizację Loop Closure.
    - **Numba JIT:** Akceleracja obliczeń gridu, redukująca zużycie CPU o 40%.
- **Architecture & Logic:**
    - **Service-Based GCS:** Rozbicie `AppController` na niezależne serwisy (`SlamManager`, `TelemetryManager`, `AIManager`).
    - **SignalRouter:** Centralny hub sygnałów Qt, eliminujący twarde zależności między logiką a GUI.
    - **CoordinateManager:** Zunifikowany system współrzędnych synchronizujący GPS, SLAM i Symulator.
- **Documentation & Management:**
    - **Hierarchical TODOs:** Wdrożenie lokalnych plików zadań dla każdego podsystemu.
    - **DONE.md Archive:** System archiwizacji ukończonych zadań w celu utrzymania czystości głównej mapy drogowej.
    - **Agent Knowledge Base:** Rozszerzenie bazy wiedzy o 50+ nowych punktów "Gotchas" i standardów architektonicznych.

### 🛠️ Fixed (Poprawki)
- **Stability:** Wdrożenie rygorystycznej sekwencji `Safe Shutdown`, eliminującej procesy zombie WSL2/GPU.
- **Security:** Usunięcie podatności na wstrzykiwanie haseł sudo w modułach Hailo i Keras.
- **Performance:** Throttling aktualizacji mapy w GUI (3Hz), przywracający responsywność interfejsu przy gęstych skanach LiDAR.

### ⚠️ Changed (Zmiany)
- **Standardy:** Pełna migracja konfiguracji do modeli **Pydantic (RCConfig)** z automatyczną walidacją i dokumentacją parametrów w GUI.

## [v1.3.0] - 2026-05-12

### 🚀 Major Feature: Input & Hardware Abstraction
- **Data-Driven HID Parser:** Całkowita przebudowa obsługi niestandardowych kontrolerów HID. Zastąpiono sztywne kodowanie systemem opartym na zewnętrznych deskryptorach YAML.
- **YAML Descriptors:** Wprowadzono obsługę plików konfiguracyjnych `.yaml` definiujących strukturę raportów HID (offsety, maski bitowe, typy danych).
- **Fail-Safe Parsing:** Parser odporny na uszkodzone pakiety; poprawne logowanie zdarzeń bez awarii aplikacji.

## [v1.2.0] - 2026-02-11

### 🚀 Major Feature: Mission Control & OSD Expansion
Rozszerzenie możliwości planowania misji oraz nowe instrumenty pokładowe (OSD).

### ✨ Added (Nowości)
- **Mission Control:**
    - **Speed Limits:** Możliwość definiowania limitów prędkości dla każdego waypointa.
    - **Reverse Mission:** Funkcja natychmiastowego odwracania kolejności misji.
    - **Visuals:** Wyświetlanie limitów prędkości bezpośrednio na mapie.
- **OSD Instruments:**
    - **G-Force Meter:** Wizualizacja przeciążeń (akcelerometr) w czasie rzeczywistym.
    - **Steering Visualizer:** Pasek pokazujący aktualne wychylenie steru.
    - **AI Confidence:** Widget prezentujący pewność sieci neuronowej.
    - **Drift HUD:** Arcade-style widget z punktami i mnożnikiem driftu.
    - **Battery Monitor:** Graficzna ikona baterii z kolorem zależnym od napięcia.
- **AI Integration:** Przygotowanie pod integrację backendu AI (Confidence Score).

### 🛠️ Fixed (Poprawki)
- **AI Workflow:** Naprawiono `AITrainer` (obsługa `telemetry.csv` zamiast tylko JSON).
- **Verification:** Dodano skrypt `verify_osd_widgets.py` do testowania instrumentów w izolacji.

## [v1.1.0] - 2026-02-11

### 🎯 Major Milestone: Autonomous Navigation & Stability
Wydanie v1.1.0 to kulminacja prac nad autonomiczną nawigacją i stabilnością systemu. Wprowadzono **Local Planner** (unikanie przeszkód), poprawiono logikę skrzyni biegów oraz zoptymalizowano logger danych AI.

### ✨ Added (Nowości)
- **Local Planner (V1):** Lokalny planer ścieżki wykorzystujący LiDAR i IMU do omijania przeszkód w czasie rzeczywistym.
- **Connection Prompt:** Nowe okno dialogowe po połączeniu z RPi (wybór między kalibracją ESC a uzbrojeniem systemu).
- **AI Data Quality:** Ulepszony logger danych AI (naprawiono brakujące pliki, dodano telemetrię nawigacyjną).
- **Physical Gearbox Model:** Pełna synchronizacja fizyki pojazdu z profilem (`gearbox_config`) - V-max, przełożenia, logika wstecznego biegu.

### 🛠️ Fixed (Poprawki)
- **Navigation:** Naprawiono nawigację przycisku "Calibrate ESC" w oknie powitalnym (teraz poprawnie kieruje do zakładki Tests).
- **Simulation:** Poprawiono logikę wstecznego biegu (użycie `-1` zamiast `"R"`).
- **Stability:** Rozwiązano problem z zapisem plików przez `AIDataLogger` (wątki).
- **UI:** Poprawiono indeksowanie zakładek w `AppController`.

## [v1.0.0] - 2026-01-14

### 🛠️ Fixed (Poprawki)
- **Deployment:** Rozwiązano błąd kompilacji `python-prctl` poprzez dodanie `libcap-dev` do Dockerfile.
- **Deployment:** Naprawiono błąd instalacji PySide6 na RPi poprzez instalację wersji `pip` z kompletem zależności systemowych `libxcb`.
- **WebRTC:** Wyeliminowano błąd `ValueError: None is not in list` poprzez wymuszenie kierunków transceiverów (sendonly/recvonly).
- **WebRTC:** Naprawiono błąd `Signal source has been deleted` na PC poprzez poprawne zarządzanie hierarchią Parent/Child w CommManager.
- **GUI:** Naprawiono konflikt metaklas (metaclass conflict) w bazowych strategiach komunikacji.
- **GUI:** Naprawiono wyświetlanie logów w narzędziu wdrożeniowym (oczyszczanie z kodów ANSI).
- **Hardware:** Poprawiono logikę wykrywania kamer na RPi 5 w pliku `config.txt`.

### ✨ Added (Nowości)
- **Network:** Pełna integracja Tailscale ze skryptem wdrożeniowym (automatyczna instalacja i logowanie).
- **Diagnostics:** Ulepszono wskaźnik statusu połączenia (LED) oraz odblokowano zaawansowane akcje (Logs/Restart) po teście SSH.
- **AR:** Dodano wymuszenie kierunku strumienia wideo, stabilizując rzutowanie bramek AR na PC.

## [v0.6.7] - 2026-01-05

### ✨ Added (Nowości)
- **Local Planner Debug Widget (OSD):** Nowy widżet w edytorze OSD prezentujący stan planera (siatka zajętości 2D, mapa szorstkości terenu IMU, ścieżka A*, wektory celu GPS).
- **GUI Config Tab (Planner Settings):** Nowe kontrole w interfejsie do strojenia wag kosztów przeszkód.

## [v0.6.6] - 2025-12-20

### ✨ Added (Nowości)
- **Proactive Autonomy:** Moduł prewencyjnego omijania przeszkód i planowania tras na bazie fuzji danych z LiDAR oraz IMU.
- **Costmap Generation:** Generowanie lokalnych map kosztów uwzględniających przeszkody twarde i nierówności terenu.

## [v0.6.1] - 2025-12-01

### ✨ Added (Nowości)
- **Multimodal AI (Sensor Fusion):** Wektor 17 stref sensorycznych (LiDAR, prędkość, pochylenie IMU, odchylenie nawigacyjne) podawany równolegle ze strumieniem wideo.
- **Hybrid AI Manager:** Wsparcie dla modeli `.tflite` (CPU) oraz `.hef` (NPU Hailo-8L).

## [v0.6.0] - 2025-11-20

### ✨ Added (Nowości)
- **Dockerized RPi Application:** Konteneryzacja aplikacji na RPi w środowisku Python 3.11-slim.
- **SimHub Integration:** Emulacja protokołu Forza Motorsport UDP do integracji z SimHub i platformami ruchu.
- **Hardware-in-the-Loop (HITL):** Możliwość zasilania telemetrii z zewnętrznych symulatorów wyścigowych.

## [v0.5.1] - 2025-11-15

### ✨ Added (Nowości)
- **Distributed Multiplayer (MQTT & Lobby):** Przejście na architekturę MQTT z wbudowanym brokerem oraz sędzią wyścigu (`RaceDirector`).

## [v0.4.4] - 2025-11-10

### ✨ Added (Nowości)
- **SimHub Bridge:** Sterowanie platformami ruchu i pasami basowymi przez mostek UDP Forza.

## [v0.4.3] - 2025-11-05

### ✨ Added (Nowości)
- **Active Safety (ACC):** Adaptacyjny tempomat na RPi płynnie redukujący przepustnicę przed przeszkodami.
- **Ghost Replay:** System nagrywania i wyświetlania wirtualnego ducha optymalnego okrążenia.

## [v0.4.2] - 2025-10-28

### ✨ Added (Nowości)
- **Adaptive Video Streaming:** Algorytm AIMD regulujący strumieniowanie WebRTC w zależności od RTT/Ping.

## [v0.4.0] - 2025-10-20

### ✨ Added (Nowości)
- **Extended Kalman Filter (EKF):** 7-stanowy EKF dla precyzyjnej orientacji przestrzennej.
- **Motion Cueing 2.0:** Algorytm z filtrami Washout i Tilt-Coordination do symulowania przeciążeń G-Force.

## [v0.3.0] - 2025-10-10

### ✨ Added (Nowości)
- **Industrial Edition Base:** Usługa `Supervisor Service` zarządzająca cyklem życia aplikacji na RPi.
- **Native Drivers:** Lekkie sterowniki `smbus2` eliminujące biblioteki Adafruit.

## [v0.2.8] - 2025-09-25

### ✨ Added (Nowości)
- **OSD Polish:** Przeprojektowanie motywów wizualnych OSD (wyścigowy, off-road, lotniczy).

## [v0.2.7] - 2025-09-18

### ✨ Added (Nowości)
- **OSD Editor 2.0 & RPi Health Monitor:** Edytor drag-and-drop OSD oraz monitorowanie temperatury CPU/RAM na RPi.

## [v0.2.6] - 2025-09-10

### ✨ Added (Nowości)
- **LiDAR Radar Widget:** Wizualizacja 72-strefowa LiDAR w stacji GCS.

## [v0.2.5] - 2025-09-01

### ✨ Added (Nowości)
- **Global Internationalization (i18n):** Przebudowa GUI z natywnym wsparciem dla 9 języków (PL, EN, DE, FR, IT, ES, CS, SK, ZH).

## [v0.2.0] - 2025-07-01

### ✨ Added (Nowości)
- **Clean Architecture & QPainter:** Migracja rysowania OSD na natywny `QPainter` oraz wdrożenie wzorców Domain/Services.

## [v0.1.0] - 2025-05-01

### ✨ Added (Nowości)
- **The Phoenix MVP:** Pierwsza stabilna wersja po przepisaniu aplikacji na PySide6 i architekturę wielowątkową.
