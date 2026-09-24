# Changelog

All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v1.3.37] - 2026-09-24

### 🏎️ Przestrzenna Tablica Startowa AR w Świecie (World-AR Start Board & HUD Fallback)
- **Kratownica Bramki i 5 Świateł Startowych F1 w Przestrzeni 3D (`RaceStartOverlay`):**
  - Zaimplementowano trójwymiarową bramownicę startową (Gantry Truss) zawieszoną w przestrzeni świata nad bramką Start/Meta ($H=2.8\,\text{m}$), łączącą słupki lewy i prawy.
  - Wdrożono tablicę startową F1 z 5 soczewkami LED ze skalowaniem perspektywicznym, realistyczną poświatą radialną dla aktywnych diod i rozbłyskiem zielonej linii "GO". W trybie klasycznym wyświetlane jest przestrzenne odliczanie 3–2–1 i "START!".
- **Inteligentny Filtr Wiarygodności Lokalizacji i Automatyczny Fallback na HUD 2D:**
  - Dodano walidator jakości namiaru (`_localization_is_reliable`): dla GPS wymagany jest `fix >= 3` oraz `h_accuracy <= 3.0 m`, dla SLAM `active=True` oraz `confidence >= 0.60`.
  - W przypadku słabego sygnału pozycjonowania, braku bramki lub gdy bramka znajduje się za plecami pojazdu ($Z \le 0.5\,\text{m}$), stacja natychmiastowo i bezszwowo przełącza się na klasyczną tablicę ekranową HUD 2D.
- **Bezpośrednia Integracja Strumienia Danych Toru z OSD FPV:**
  - Spięto sygnały aktualizacji toru wirtualnego z `FPVWindow` i `OSDGraphicsItem`, zapewniając natychmiastowe odzwierciedlenie zmian toru w podglądzie wideo bez restartu stacji.

### 🏁 Interaktywny Edytor Wirtualnego Toru na Mapie (GPS / SLAM Virtual Track Editor)
- **Bezpośrednie Tworzenie i Edycja Bramek na Mapie (`VirtualTrackItem` & `MapWidget`):**
  - Zaimplementowano intuicyjny tryb edycji toru wirtualnego na mapie: operator może klikać i przeciągać kursor w celu postawienia bramki wraz z wektorem dozwolonego kierunku przejazdu i zadaną szerokością.
  - Wdrożono dynamiczny podgląd widmowy (`ghost_gate`) w trakcie przeciągania myszy, prezentujący w czasie rzeczywistym pozycję słupków lewego (L) i prawego (R), szerokość w metrach oraz strzałkę kierunku.
  - Zapewniono automatyczny dobór typu bramki: pierwsza bramka staje się automatycznie bramką Start/Meta ze specjalnym złotym wykończeniem i wzorem szachownicy, a kolejne bramki stają się sektorami (cyjan) lub punktami kontrolnymi (pomarańcz).
- **Zarządzanie Kierunkiem, Kolejnością i Właściwościami Bramek (`VirtualTrackEditorWidget`):**
  - Dodano pasek narzędzi mapy z przyciskami przełączania trybu edycji na mapie, odwracania kierunku bramki `(⇄)` o 180° oraz zmiany kolejności bramek w sekwencji toru `(▲/▼)`.
  - Wdrożono menu kontekstowe pod prawym przyciskiem myszy na mapie umożliwiające błyskawiczną zmianę typu bramki (Start/Meta, Sektor, Checkpoint), odwrócenie kierunku przejazdu lub jej usunięcie.
- **Przejrzysty Podział w Kreatorze Misji (`MissionEditorWindow`):**
  - Zreorganizowano okno kreatora na dwie dedykowane zakładki: `Wirtualny Tor (Bramki / Sektory)` oraz `Misja Nawigacyjna (Waypointy)`, eliminując przeciążenie interfejsu.
- **Pełne Wsparcie Wielojęzyczności (100% i18n):**
  - Dodano tłumaczenia dla wszystkich nowych elementów interfejsu we wszystkich 8 językach oficjalnych (`pl`, `en`, `de`, `es`, `fr`, `it`, `cs`, `zh`) z wynikiem 0 unfinished.

### 🛡️ PyTorch & CUDA Thread Initialization Guard (Access Violation Fix)
- **Eliminacja Wyścigu Inicjalizacji PyTorch/CUDA w GCSVisionThread:**
  - Przeniesiono start wątku wizyjnego `GCSVisionThread` za etap pełnego skonstruowania interfejsu GUI w `controller_step_initializer.py`. Eliminuje to kolizję rejestracji operatorów PyTorch/Torchvision w tle z wątkiem głównym podczas tworzenia widżetów.
- **Wymuszenie Bezpiecznej Inspekcji CUDA (NVML Guard):**
  - Wdrożono zmienną środowiskową `PYTORCH_NVML_BASED_CUDA_CHECK=1` w `main.py`, chroniącą przed wywołaniem natywnego `cuInit` w `torch._C._cuda_getDeviceCount()` i błędem Access Violation (0xC0000005) na maszynach ze starszymi lub niekompatybilnymi sterownikami CUDA 13.0.
- **Odporność Modułów Wizyjnych na Błędy Bibliotek AI:**
  - Objęto instancjonowanie modeli SSDLite i OWL-ViT w `vision_worker.py` oraz `vision_engine.py` ochronnymi blokami `try...except`, gwarantując, że awaria ładowania wag nie przerywa działania stacji ani detekcji kodów QR/linii.
  - Zaimplementowano buforowanie statusu sprzętowego (`_cached_hw_status`) w `SmartOwlDetector`.

## [v1.3.35] - 2026-09-23

### 🛡️ Stabilizacja Startowa GCS & Bezpieczna Inicjalizacja Grafiki (OpenGL Safe Fallback)
- **Bezpieczny Viewport QOpenGLWidget w FPVWindow:**
  - Wdrożono bezpieczną instancjację `QOpenGLWidget` w bloku ochronnym `try...except`, zabezpieczając proces przed natywnym wyjściem/crashem sterownika graficznego na maszynach bez pełnego wsparcia OpenGL.
  - W przypadku błędu akceleracji sprzętowej następuje automatyczny i płynny fallback na stabilny rasterizer programowy.
- **Eliminacja Przedwczesnego Wyścigu Wideo (Smart Video Guard):**
  - Wyeliminowano wywoływanie `manage_stream()` podczas inicjalizacji `MainWindow`, zapobiegając próbom restartu strumienia przed pełnym powiązaniem menedżerów w aplikacji.
- **Granularna Diagnostyka Budowania Interfejsu (Step 4/6 Build Logging):**
  - Dodano precyzyjne znaczniki logowania pod-etapów tworzenia wszystkich zakładek i edytorów w `MainWindow` oraz `controller_step_initializer.py`, gwarantując pełną przejrzystość w pliku `rcsim_gcs.log`.

## [v1.3.34] - 2026-09-22

### 🎨 Doszlifowanie GUI i Pełna Spójność Wizualna Tier 2 / Tier 3 (i18n & Combobox Alignment)
- **Eliminacja Ucinania Tekstu w Polach Wyboru QComboBox:**
  - Zidentyfikowano i trwale wyeliminowano problem ucinania tekstu oraz przesuwania go do dolnej krawędzi w polach wyboru medium transmisyjnego Tier 3 (`ELRS Medium:`, `MAVLink Medium:`) oraz Tier 2 (`esp_transport_selector`).
  - Przyczyną były znaki nowej linii `\n` oraz wcięcia wstrzyknięte do tagów `<translation>` w plikach `.ts`, powodujące traktowanie ciągów znaków przez Qt jako dwuwierszowych.
  - Oczyszczono wszystkie pliki tłumaczeń i zabezpieczono generator przed dodawaniem zbędnych białych znaków.
- **Wyrównanie Etykiet i Ujednolicenie Nazewnictwa:**
  - Wyrównano wszystkie etykiety parametrów połączeń do prawej strony (`Qt.AlignmentFlag.AlignRight | Qt.AlignmentFlag.AlignVCenter`), nadając interfejsowi profesjonalny układ siatki.
  - Zmieniono etykietę w Tier 2 z mylącej `"Airlink / Wi-Fi (UDP)"` na `"Wi-Fi UDP (Lokalny AP / Router)"`.
- **Pełne Tłumaczenia 100% i18n (8 Języków):**
  - Uzupełniono 18 brakujących fraz w kontekście `ConnectionTab` (m.in. `Transmitter Protocol:`, `Transmission Medium:`, `Firmware Version:`).
  - Skompilowano binarne pliki `.qm` dla wszystkich 8 języków (`pl`, `en`, `de`, `es`, `fr`, `it`, `cs`, `zh`) z wynikiem 0 unfinished.

## [v1.3.33] - 2026-09-22

### 🌐 CRSF i MAVLink over TCP/UDP, ESP32 Tier 2 Pro & Zgodność ze Steam
- **Natywna Warstwa Transportowa CRSF & MAVLink over TCP/UDP (Airlink / Wi-Fi Backpack / MicroLink VPN):**
  - Wdrożono modułową abstrakcję I/O `BaseByteTransport` oraz implementacje `serial_transport.py`, `tcp_transport.py` i `udp_transport.py`.
  - Dodano bezstratną obsługę bezprzewodowych modułów ExpressLRS (Airlink / Backpack) z protokołem `TCP_NODELAY`, buforem kołowym i autoodzyskiwaniem.
  - Zaimplementowano bezpośrednie łączenie MAVLink Airport (`tcp:IP:PORT` oraz `udpout:IP:PORT`) w `pymavlink` dla mostków telemetrii.
  - Zintegrowano kontrolki wyboru medium w GUI (`ConnectionTab`) z pełną obsługą 8 języków i18n, dynamicznym ukrywaniem pól i walidacją Pydantic.
- **ESP32 Tier 2 Pro (CRSF Multi-Link Hub & MicroLink Tailscale VPN):**
  - Opracowano pokładowe oprogramowanie dla pojazdów RC `ESP32V4_CRSF_MultiLink` z sumą CRC8 DVB-S2, PCA9685 (I2C 400kHz), IMU, GPS i ADC1.
  - Wdrożono integrację z tunelem VPN MicroLink (Tailscale / WireGuard) do bezpiecznej jazdy przez Internet / LTE bez publicznego IP.
  - Opracowano dongle nadawczy dla PC `ESP32_CRSF_Dongle_Transmitter` (USB CDC -> ESP-NOW).
- **Architektura Hybrydowa NanoOWL / OWL-ViT Dual-Engine (Steam-Safe JIT Cache):**
  - Wdrożono inteligentny detektor fasadowy `SmartOwlDetector` w `vision_engine.py` z automatycznym wykrywaniem obecności GPU NVIDIA i biblioteki TensorRT.
  - Zaimplementowano 100% bezpieczny i stabilny fallback na silnik `OwlVitDetector` (PyTorch CUDA / CPU) dla graczy z kartami AMD Radeon, Intel lub bez dedykowanego GPU, całkowicie eliminując błędy binarne `invalid engine binary` na platformie Steam.
  - Wdrożono architekturę lokalnego bufora `%LOCALAPPDATA%\RCSIM\models\nanoowl_vit_b16_{sm_arch}.engine` dopasowanego do mikroarchitektury GPU użytkownika.
  - Zaktualizowano `vision_worker.py` do obsługi `SmartOwlDetector` w pętli wizji FPV.
  - Wzbogacono skrypt `build_steam.py` o automatyczną sanitację nieprzenośnych plików `.engine`, obniżając wagę paczki instalacyjnej Steam o ~183 MB.
- **Remastering i Unowocześnienie Ikony Aplikacji (Carbon Fiber Multi-Resolution Icon):**
  - Podniesiono jakość graficzną ikony do standardu współczesnych symulatorów wyścigowych przy zachowaniu 100% oryginalnego motywu (pomarańczowo-błękitne aerodynamiczne "R" oraz chromowany logotyp "RCSIM").
  - Wprowadzono frezowaną ramkę z prawdziwego włókna węglowego (Carbon Fiber) z krawędziowym oświetleniem 3D i ceramicznym połyskiem lakieru.
  - Usunięto sztywne tło narożników, zastępując je wygładzonym kanałem alfa (100% przezroczystości), co eliminuje artefakty kwadratu na pulpicie i w bibliotece Steam.
  - Wygenerowano 7-warstwowy plik `.ico` Multi-Resolution (16, 24, 32, 48, 64, 128, 256 px) ze specjalnym filtrem wyostrzającym dla miniatury paska zadań (16px/32px).
  - Zaktualizowano wszystkie docelowe pliki projektu: `app_icon.ico`, `icon/app_icon.ico`, `iconrcsim.png` (Master RGBA) oraz `app_icon.png`.
- **Audyt Licencyjny dla Steam & Zgodność Prawna (Steam Compliance & AI Disclosure):**
  - Przeprowadzono kompleksowy audyt techniczno-prawny dla dystrybucji na Steam (`steam_compliance_report.md`).
  - Potwierdzono pełną zgodność bibliotek permisywnych (MIT, BSD, Apache-2.0) oraz słabego copyleft (LGPLv3 dla PySide6, LGPLv2.1 dla pygame-ce, Cairo, GStreamer) poprzez dynamiczne linkowanie folderowe Nuitka i prawo do wymiany bibliotek w EULA.
  - Zaktualizowano `THIRD_PARTY_LICENSES_PL.md` oraz `THIRD_PARTY_LICENSES_EN.md` o model SSDLite MobileNetV3 oraz wymaganą atrybucję zbioru Microsoft COCO (CC BY 4.0).
  - Przygotowano oficjalną deklarację dla formularza Steam AI Content Disclosure w zakresie przedpremierowych grafik UI (Pre-generated AI assets).
- **GUI i Pełna Lokalizacja 100% i18n (8 Języków):**
  - W widżecie `ai_vision_widget.py` dodano grupę *Akceleracja sprzętowa AI* na żywo raportującą aktywny backend (TensorRT, PyTorch CUDA, PyTorch CPU) oraz model wykrytej karty graficznej.
  - Gruntownie przebudowano panel *Tier 2 (WiFi / Serial ESP32)* w zakładce *Połączenie*: dodano selektory firmware (Tier 2 Pro V4 CRSF Multi-Link vs Classic V1-V3), protokołów transportowych (Wi-Fi UDP, MicroLink VPN Tailscale, ESP-NOW Link, Hardware Serial), podgląd strumienia kamery FPV MJPEG oraz panel specyfikacji sprzętowej I2C/IMU/GPS/ADC.
  - Wyeliminowano ucinanie tekstu w rozwijanych listach `QComboBox` na Windowsie poprzez optymalizację stylów `dark.qss` i `light.qss` (`min-height: 28px`).
  - Wygaszono nadmiarowe logowanie 50 Hz braku telemetrii IMU dla Force Feedbacku w `input_manager.py` (throttling 10s).
  - Wszystkie nowe ciągi znaków opakowano w `self.tr(...)`, przetłumaczono dla wszystkich 8 oficjalnych języków (PL, EN, DE, ES, FR, IT, CS, ZH) i skompilowano pliki `.qm` z wynikiem 0 unfinished.
  - Utworzono testy jednostkowe w `test_smart_owl_detector.py` oraz `test_connection_tab_gui.py` (wszystkie testy zaliczone).

## [v1.3.32] - 2026-09-17

### 🚀 Zaawansowany Pakiet Diagnostyczny & Czarna Skrzynka (6-Pillar Diagnostic Suite)
- **Czarna Skrzynka Telemetrii (FlightRecorder Ring Buffer):**
  - Wdrożono bezalokacyjny bufor kołowy 50 ostatnich kluczowych zdarzeń (ARM/DISARM, Emergency Stop, przełączanie trybów, telemetria).
  - W przypadku wystąpienia nieobsłużonego błędu w wątku głównym lub wątkach pobocznych (`sys.excepthook`, `threading.excepthook`), historia operacji jest automatycznie zrzucana do pliku logu przed tracebackiem awarii.
- **Eksporter Raportów Diagnostycznych w GUI:**
  - W zakładce *Ustawienia -> Deweloper* dodano przyciski: *Otwórz folder logów* oraz *Eksportuj raport (.zip)*.
  - Eksport generuje na Pulpicie kompletny pakiet ZIP zawierający specyfikację PC (CPU, RAM, GPU, sterowniki), wersje bibliotek, pliki logów (`rcsim_gcs.log`, `crash_handler.log`) oraz aktualne konfiguracje JSON.
- **Szczegółowy Inwentarz Urządzeń USB/HID:**
  - Przy każdym skanowaniu lub podłączeniu urządzenia system szczegółowo loguje: nazwę kontrolera, backend (SDL/DirectInput/Logitech), liczbę osi, przycisków, hats, GUID oraz status gotowości Force Feedback.
- **Diagnostyka Portów Szeregowych i Watchdog RX CRSF:**
  - Wprowadzono logowanie parametrów połączenia UART (baudrate, bity, parzystość) oraz 2.5-sekundowy watchdog ostrzegający o braku danych RX, a także potwierdzenie odbioru pierwszej poprawnej ramki CRSF.
- **Skaner Kamer i Grabberów FPV przy Starcie:**
  - Automatyczna inwentaryzacja dostępnych urządzeń wideo (`QMediaDevices.videoInputs()`) podczas startu stacji naziemnej z logowaniem ich identyfikatorów i formatów.
- **Pomiary Czasu Zamykania & Audyt Wiszących Wątków:**
  - Precyzyjny pomiar czasu zamykania aplikacji w milisekundach oraz audyt aktywnych wątków `threading.enumerate()` wyłapujący wiszące wątki nie-demoniczne.
- **Wyeliminowanie Fałszywych Alarmów w Logach:**
  - Brak opcjonalnego pakietu Steamworks w trybie standalone jest teraz logowany czysto na poziomie `INFO` zamiast `ERROR`.
  - Usunięto ostrzeżenia `Gymnasium Box UserWarning` o precyzji float32/float64 w środowisku symulacji.
- **Optymalizacja Kodu i 100% i18n:**
  - Zoptymalizowano `general_tab.py` poniżej limitu 500 linii oraz skompilowano kompletne tłumaczenia `.qm` dla wszystkich 8 języków (PL, EN, DE, ES, FR, IT, CS, ZH) z wynikiem 0 unfinished.

## [v1.3.31] - 2026-09-11

### 🛠️ Stabilność Startu i Bezpieczeństwo Środowiskowe (Hotfix)
- **Eliminacja Zawieszenia przy Inicjalizacji Silników AI (CUDA Probe Bypass):**
  - Wyłączono bezwarunkowe odpytywanie `torch.cuda.is_available()` podczas startu stacji w `SyncInferenceEngine`. Zapobiega to natywnym błędom C++ Access Violation (`0xC0000005`) na komputerach bez dedykowanych kart NVIDIA RTX lub z niekompatybilnymi sterownikami. Domyślnym urządzeniem jest zawsze bezpieczne CPU, a weryfikacja GPU następuje wyłącznie przy jawnym wczytaniu wag modelu.
- **Naprawa Uprawnień Katalogu Bufora Kafelków (Program Files UAC Guard):**
  - Przeniesiono domyślny katalog pamięci podręcznej kafelków mapy (`tile_cache`) dla wersji instalacyjnej z `C:\Program Files\RCSIM\tile_cache` do `%LOCALAPPDATA%\RCSIM\tile_cache`, eliminując błędy braku dostępu `[WinError 5] Access is denied` na kontach standardowych użytkowników.
- **Odporność i Granice Błędów w AppInitializer:**
  - Wdrożono szczegółowe logowanie poszczególnych etapów tworzenia menedżerów (`[INIT]`) oraz zabezpieczono inicjalizację modułów sieciowych i autonomicznych (`RaceNetworkManager`, `RaceDirector`, `SlamController`) blokami `try...except`.
- **Rejestracja Zrzutów Awarii C++ (Faulthandler Crash Log):**
  - Naprawiono konfigurację `faulthandler` w trybie bezkonsolowym GUI — zrzuty krytycznych błędów C++ (SIGSEGV/SIGABRT) są teraz niezawodnie rejestrowane w pliku `crash_handler.log` w katalogu logów.
- **64-bitowa Kompatybilność Mostka MAVLink:**
  - Zdefiniowano ścisłe typy `argtypes` i `restype` dla wywołań systemowych `WSAIoctl` w gnieździe UDP, eliminując ryzyko uszkodzenia stosu na 64-bitowych systemach Windows.

## [v1.3.30] - 2026-09-11

### 🌍 Pełna Międzynarodowość (100% i18n we wszystkich 8 językach)
- **Kompletna baza tłumaczeń GUI (PL, EN, DE, ES, FR, IT, CS, ZH):**
  - Wszystkie etykiety, przyciski, komunikaty i okna dialogowe (m.in. kalibracja IMU, edytor krzywych, konfiguracja AI, telemetria) zostały przetłumaczone i skompilowane do plików `.qm`.
  - Wdrożono rygorystyczny test weryfikacyjny `test_i18n.py` gwarantujący brak niedokończonych lub brakujących tłumaczeń.
  - Rozszerzono bazę pomocniczych opisów narzędziowych (Tooltips) dla zaawansowanych parametrów FPV, filtrów i AI.
- **Optymalizacja Mostka SimHub i Testów Jednostkowych:**
  - Usprawniono buforowanie i konwersję kątów CRSF do mostka SimHub UDP.
  - Zaktualizowano i rozszerzono zestaw testów automatycznych stacji naziemnej.

## [v1.3.29] - 2026-09-09

### ⚡ Zero-Lag Motion Cueing, SimHub & Adaptive IMU Interpolation
- **Dynamiczny Krok Czasowy (dt) w Filtrach Orientacji (Eliminacja 800 ms Opóźnienia):**
  - Rozwiązano problem opóźnienia fazowego orientacji pojazdu przy niskich częstotliwościach telemetrii radiowej (np. 12 Hz / 20 Hz z ExpressLRS).
  - Wszystkie filtry orientacji (`EKFFilter`, `ComplementaryFilter`, `MadgwickFilter`, `MahonyFilter`) przyjmują teraz dynamiczny krok czasowy $dt$ mierzony na żywo z zegara monotonicznego, eliminując zaniżanie kąta obrotu żyroskopu i konieczność powolnego doganiania rzeczywistości przez akcelerometr.
- **Naprawa Fuzji Kwaternionów w Filtrze Komplementarnym:**
  - Zoptymalizowano korekcję grawitacyjną: zamiast niszczącego obrót żyroskopu slerpa delty kwaternionu, filtr aplikuje ważony obrót korygujący $(1 - \alpha)$, zachowując pełną dynamikę kątów wychylenia nadwozia.
- **Adaptacyjny Interpolator IMU i Dead Reckoning:**
  - Wdrożono moduł `IMUInterpolator` z buforowanym algorytmem SLERP dla OSD/HUD (stabilne 60 FPS) oraz ekstrapolacją Dead Reckoning z automatycznym wykładniczym wygaszaniem (Decay) i twardym failsafe dla pętli SimHub i Force Feedback.
  - Odfiltrowano pakiety nietelemetryczne (detekcje AI, statystyki łącza), zapobiegając chwilowym drganiom i zerowaniu kątów do 0.0°.
- **Optymalizacja Wątku Odbiorczego CRSF i Pomiar Latencji:**
  - Zredukowano uśpienie pętli odbiorczej portu szeregowego UART z 10 ms do 1 ms, eliminując kwantyzację opóźnień systemu Windows.
  - Wprowadzono precyzyjny stempel pomiaru czasu przelotu pakietu (`_ingress_perf`) od wejścia z portu szeregowego do wysyłki UDP do SimHub, z podglądem statystyk (min/avg/max) w logach.

## [v1.3.28] - 2026-09-07

### 🌍 Localization & ExpressLRS Configurator (MSP over CRSF I18n)
- **Pełne Tłumaczenie Dialogu Konfiguracji ExpressLRS (PL / EN / FR):**
  - Przetłumaczono wszystkie elementy interfejsu konfiguratora modułów ExpressLRS: nagłówki, paski postępu, statusy operacji, przyciski akcji oraz okna dialogowe potwierdzeń.
  - Wdrożono profesjonalne opisy parametrów technicznych (ToolTips) w języku angielskim i francuskim dla wszystkich funkcji modułów ELRS (*Packet Rate, Telem Ratio, Switch Mode, Antenna Diversity, Gemini Link Mode, Model Match, TX Power, Dynamic Power, Fan Threshold, RF Band*).
  - Dodano wewnętrzny fallback językowy `tr()` w klasie `ELRSConfiguratorDialog`, gwarantujący stabilne działanie tłumaczeń w każdym środowisku.

### 🧭 Performance & Navigation Engine (Monaco SLAM Default Optimization)
- **Domyślne Wyłączenie Silnika SLAM (Resource Optimization & Safe Default):**
  - Zmieniono domyślną wartość flagi `use_slam` na `False` w modelu Pydantic `slam.py` oraz silniku mapowania `mapping_engine.py`.
  - Aplikacja startuje z wyłączonym silnikiem SLAM, co eliminuje obciążenie CPU i pamięci w trakcie standardowej jazdy i pozwala użytkownikowi włączyć nawigację SLAM tylko wtedy, gdy jest faktycznie potrzebna.

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
