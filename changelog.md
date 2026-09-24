# Changelog

All notable changes to Modulo firmware will be documented in this file, structured by software component.

---

## Modulo Base
### [0.1.101] - 2026-09-23
### Added
- Animazioni LED anello Base per notifiche in arrivo (Blink e Swipe rotante) con colore ed effetto configurabili.
- Gestione comandi SET_SCREEN_POWER e SET_SCREEN_WAKE_ON_NOTIF con sincronizzazione broadcast WebSocket.
- Instradamento automatico notifiche allo Smart Screen collegato.

### [0.1.100] - 2026-09-18
### Changed
- Memorizzazione e ripristino della specifica vista orologio scelta dall'utente (Digitale o Analogico) dopo la fine della riproduzione musicale.

### [0.1.99] - 2026-09-18
### Changed
- Implementata persistenza vista display: quando l'utente seleziona manualmente una vista dall'app, il display rimane stabilmente su quella vista senza forzare il ritorno all'orologio dopo inattivitÃ  audio.

### [0.1.98] - 2026-09-17
### Fixed
- Recupero automatico moduli slave orfani (Smart Screen) tramite scansione periodica CMD_GET_INFO con PROTO_BROADCAST_ID.
- Aumentato timeout di offline nel registry a 24h per evitare drop dei moduli dopo reboot della Base.
- Sincronizzazione timing I2C master/slave con retry read per accomodare la latenza di rendering LVGL.
- Scansione diagnostica hardware periodica (i2c_master_probe).

### [0.1.97] - 2026-09-17
### Fixed
- Recupero automatico moduli slave orfani (Smart Screen) gia presenti sul bus I2C tramite scansione periodica CMD_GET_INFO.
- Aumentato timeout di offline nel registry a 24h per evitare drop dei moduli dopo reboot della Base.
- Sincronizzazione timing I2C master/slave per evitare svuotamento anticipato del buffer di trasmissione slave.

### [0.1.96] - 2026-09-17
### Added
- Commutazione automatica a Vista Musica all'avvio della riproduzione audio con switch configurabile via App.
- Ritorno automatico a Vista Orologio dopo 5 minuti di inattivita musicale.
- Salvataggio impostazione auto_switch_music su NVS.

### [0.1.91] - 2026-09-10
### Fixed
- Risolto blocco dello schermo sulla schermata FOTA: ripristino automatico della vista display prima del riavvio Base e auto-recovery su polling.
- Corretta gestione invio frame CMD_SCREEN_SET_VIEW eliminando l'attesa di risposta non prevista dallo slave.

### [0.1.90] - 2026-09-10
### Fixed
- Corretto bug del ciclo di sincronizzazione dell'orologio e meteo su Vista 02 del display.
- Aggiunto comando SET_DISPLAY_DATETIME per sincronizzazione istantanea da smartphone.
- Cache persistente meteo su Base per aggiornamento display affidabile e continuo.

### [0.1.89] - 2026-09-10
### Added
- SNTP Client con timezone Roma per sincronizzazione orologio schermo in tempo reale.
- Gestione comandi WebSocket SET_DISPLAY_WEATHER e SEND_POPUP_NOTIFICATION.
- Coordinamento automatico della Vista 08 (FOTA) su display durante gli aggiornamenti firmware.

### [0.1.88] - 2026-09-10
### Added
- Allineamento esatto motore copertine iTunes con Smart Filter dell'App.


### [0.1.93] - Fix I2C Discovery Legacy CRC Fallback & Unknown Module Display Mapping
- Risolto il blocco della discovery I2C per slave con firmware legacy: la Base ora valida anche frame con CRC calcolato su header (0x69).
- Modulo slave con ID 0x00 ora mappato correttamente su Display (Smart Screen) per consentire FOTA.
### [0.1.87] - Multi-View Display Routing & State Synchronization
- Sincronizzazione ed esportazione delle viste display (01 Now Playing, 02 Orologio/Meteo, 06 Notifiche, 08 OTA, 00 Brand) tramite Data Broker e WebSocket.
- Parsing robusto di target_id nel comando SET_DISPLAY_VIEW.

### [0.1.86] - Motore Copertine Album Robusto e Sincronizzazione Schermo Idle
- Implementata sanitizzazione avanzata query iTunes (rimozione suffissi rumorosi come feat, Remix, Remaster, Live).
- Query media=music con limit=3 e fallback automatico a ricerca per solo titolo.
- Sostituzione URL dinamica basata sull'ultimo slash a 180x180bb.jpg (garantisce sempre formato JPEG nativo compatibile con ROM TJPGD).
- Supporto stream HTTP chunked e buffer immagini esteso fino a 32 KB con validazione SOI header.
- Gestione I2C protetta con abort sicuro (SUB_COVER_RESET) e retry automatico in caso di chunk drop.
- Sincronizzazione dello stato idle (nessun brano) verso lo Smart Screen eliminando i placeholder demo.

### [0.1.85] - Fix LED State CONNECTED_WIFI
- Corretto bug race condition: i LED ora mostrano correttamente il verde (CONNECTED_WIFI) dopo la connessione Wi-Fi.

### [0.1.84] - Fix LED State CONNECTED_WIFI
- Corretto bug race condition: i LED ora mostrano correttamente il verde (CONNECTED_WIFI) dopo la connessione Wi-Fi, invece di restare su READY (bianco tenue).

### [0.1.83] - Supporto Copertine Alta Risoluzione 180x180
- Aggiornato il download da iTunes a 180x180bb.jpg per copertine ad altissima nitidezza.

### [0.1.82] - Pacing I2C 22ms e Telemetria Chunks
- Pacing I2C aumentato a 22ms per zero-drop chunk streaming verso Smart Screen.
- Integrata la metrica cover_chunks nel JSON di stato WebSocket.

### [0.1.81] - Thread-safe I2C Mutex & Protezione Bus durante Streaming Copertine
- Aggiunto mutex di protezione su bus I2C master per serializzare tutte le transazioni I2C.
- Bloccate letture VBUS concorrenti durante lo streaming copertina con fallback a valore cached.
- Aggiunti retry su chunk I2C della copertina e telemetria estesa stato display.

### [0.1.80] - Streaming I2C Ottimizzato Copertine Dinamiche e Demo Sync
- Migliorato il parsing della risposta iTunes API con lettura completa dello stream HTTP.
- Ottimizzato il pacing di trasmissione I2C dei chunk JPEG verso lo Smart Screen per eliminare drop di pacchetti.
- Aggiunta richiesta automatica della copertina album per la traccia di default all'avvio.

### [0.1.79] - Supporto SET_DISPLAY_MEDIA e Copertine Dinamiche
- Aggiunto supporto al comando SET_DISPLAY_MEDIA via WebSocket per il controllo diretto o da App dei metadati della vista Now Playing dello Smart Screen.
- Ottimizzata la prioritÃƒÆ’Ã‚Â  automatica tra streaming AVRCP dello Speaker Bluetooth e metadati inviati da applicazione.

### [0.1.78] - Copertine Album Dinamiche e Sincronizzazione Smart Screen
- Introdotto cover_manager per il fetch asincrono delle copertine da iTunes Search API e streaming a pacchetti I2C verso lo Smart Screen.
- Implementata la sincronizzazione live dei metadati brano (titolo, artista, stato, avanzamento) dello Speaker Bluetooth verso la vista Now Playing.
- Aggiunti comandi I2C per la gestione delle viste dello Smart Screen (CMD_SCREEN_SET_VIEW, CMD_SCREEN_LOAD_COVER).

### [0.1.77] - Negoziazione Diretta 12V PD & Refresh Periodico LED 1Hz
- Stabilizzata la tensione di bus: richiesta diretta a 12V (CFG1=0, CFG2=0, CFG3=1) ed eliminati i cicli di caduta a 5V per prevenire sfarfallii e instabilitÃƒÆ’Ã‚Â .
- Aggiunto auto-refresh lento a 1Hz per le strisce LED WS2812 dei Pogo Pin, ripristinando all'istante eventuali spegnimenti accidentali da inserimento modulo.

### [0.1.76] - Tolleranza Pings I2C Aumentata a 6 per StabilitÃƒÆ’Ã‚Â  Moduli
- Aumentata la soglia di tolleranza disconnessione MODULE_OFFLINE_THRESHOLD a 6 pings falliti consecutivi per prevenire cali di tensione 5V/12V a fronte di piccoli ritardi I2C.

### [0.1.75] - Fix Spegnimento LED dopo Animazione WIPE Connessione Modulo
- Aggiunta la variabile s_force_refresh in led_manager.c per forzare il ripristino del colore dei LED di stato al termine delle animazioni dinamiche (WIPE / BLINK), impedendo ai LED di rimanere spenti/neri.

### [0.1.74] - Fix GitHub FOTA Redirects e Reporting Tensione 5V Idle
- Aggiunta la gestione delle ridirezioni HTTP (max_redirection_count = 5) nel client FOTA per consentire il download completo dei binari da GitHub Releases.
- Corretto il fallback di lettura tensione a 5000mV in assenza di moduli attivi (eliminata la segnalazione erronea di 12V in idle).

### [0.1.73] - Ripristino Negoziazione Dinamica 20V e Fix Surriscaldamento LED Idle
- Ripristinata partenza a 5V e negoziazione assistita a cascata (20V/15V/12V/9V/5V) al collegamento dei moduli.
- Corretto il reporting di tensione all'App via I2C dallo Slave per ovviare alla mancanza di ADC su PIN 34 (ESP32-S3).
- Ottimizzata la gestione dei LED WS2812C (4 RMT + 2 SPI): rimosso il refresh continuo a 20Hz per colori statici, eliminando l'overheating dell'ESP32-S3 in idle.

### [0.1.73] - Ripristino Negoziazione Dinamica 20V e Fix Surriscaldamento LED Idle
- Ripristinata partenza a 5V e negoziazione assistita a cascata (20V/15V/12V/9V/5V) al collegamento dei moduli.
- Corretto il reporting di tensione all'App via I2C dallo Slave per ovviare alla mancanza di ADC su PIN 34 (ESP32-S3).
- Ottimizzata la gestione dei LED WS2812C (4 RMT + 2 SPI): rimosso il refresh continuo a 20Hz per colori statici, eliminando l'overheating dell'ESP32-S3 in idle.

### [0.1.72] - Fix Oscillazione VBUS nell'App (Cache Lettura I2C)
- Root cause identificata: la base ESP32-S3 non ha ADC sul GPIO 34 (pin inesistente), quindi legge VBUS via I2C dallo slave. Quando la query I2C falliva (bus occupato), il fallback restituiva 5000 mV fisso causando l'oscillazione 5V ÃƒÂ¢Ã¢â‚¬Â Ã¢â‚¬Â 12V nell'app.
- Aggiunta cache `s_last_valid_vbus_mv`: se l'I2C fallisce, restituisce l'ultimo valore valido invece di tornare a 5V di default.
- Anche le letture ADC locali ora cachano il valore per resilienza agli errori transitori.

### [0.1.71] - Fix Assoluto Oscillazione VBUS CH224K (Lock 20V/Max in bootup permanente)
- Impostati i pin CFG1=0, CFG2=1, CFG3=0 (20V/Max PD) direttamente all'inizializzazione dell'hardware di alimentazione.
- Eliminata qualunque chiamata a `set_cfg_pins(1, 0, 0)` di fallback che provocava il drop a 5V e l'oscillazione durante la lettura I2C dei moduli slave.
- Garanzia totale di tensione fissa e continua erogata al valore massimo dell'alimentatore (es. 12V/20V) dal primo istante di boot.
### [0.1.70] - Fix Definitivo Oscillazione Tensione PD CH224K (Lock Senza Re-setting CFG)
- Eliminati tutti i cambi intermedi dei pin CFG durante e dopo la negoziazione 20V/Max.
- In modalitÃƒÆ’Ã‚Â  20V (`CFG1=0, CFG2=1, CFG3=0`), il CH224K negozia e mantiene la tensione massima erogabile dal caricatore (es. 12V/15V/20V) senza eseguire re-setting sui pin CC, eliminando completamente qualsiasi oscillazione tra 5V e 12V.
### [0.1.69] - Fix Oscillazione VBUS PD e Locking CFG
- Risolto il bug hardware-loop introdotto nella 0.1.68 che causava la disconnessione e oscillazione continua della tensione VBUS tra 5V e 12V/15V.
- Ora i pin CFG vengono bloccati correttamente sulla tensione massima effettivamente erogata dal caricatore, prevenendo il timeout del CH224K.
### [0.1.68] - Stabilizzazione Negoziazione PD CH224K (Stop Fallback 5V)
- Rimosso il cambio superfluo dei pin CFG a 12V dopo la risposta dell'alimentatore alla richiesta 20V. La commutazione faceva resettare il chip CH224K a 5V.
- Mantenuto il lock permanente sui 12V/15V agganciati per garantire stabilitÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â  elettrica continua.

### [0.1.67] - Fix Negoziazione PD Assistita (12V/15V/20V) e Telemetria VBUS
- Risolto il bug che bloccava prematuramente la negoziazione a 5V prima del rilevamento dei moduli I2C
- Abilitata la negoziazione PD assistita dai moduli per agganciare automaticamente il massimo profilo erogabile dall'alimentatore (es. 12V/15V/20V)
- Serializzata la misura di tensione `vbus_mv` (bus 20V) nei pacchetti di stato WebSocket per il badge PD dell'App

### [0.1.65] - Accettazione 12V/15V e Stop Loop Re-negoziazione
- Accetta e stabilizza la tensione massima fornita dal caricatore PD (es. 12V/15V/9V) senza resettare a 5V
- Elimina i loop infiniti di ri-negoziazione PD su I2C
- **Pilotaggio Open-Drain (OD) dei pin CFG e Reset Pulse 5V**: Configurato il driver GPIO dei pin `CFG1` (GPIO 15), `CFG2` (GPIO 16), `CFG3` (GPIO 17) in modalitÃƒÆ’Ã†â€™Ãƒâ€ Ã¢â‚¬â„¢ÃƒÆ’Ã¢â‚¬Å¡Ãƒâ€šÃ‚Â  Open-Drain con pull-up interno abilitato per consentire il perfetto rilascio (floating) delle linee del CH224K. Aggiunta l'emissione di un impulso di reset transitorio a 5V (`CFG1=1` per 150ms) prima della richiesta 20V per forzare l'hardware del CH224K ad avviare un nuovo ciclo di negoziazione USB-PD sui pin CC.

### [0.1.62] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-08-08
- **Isolamento DP/DM in Alta Impedenza (High-Z)**: Configurato GPIO 18 (`USB_DN`) e GPIO 19 (`USB_DP`) dell'ESP32-S3 come ingressi fluttuanti a tri-state (High-Z) durante l'inizializzazione del gestore alimentazione. Questo rimuove l'interferenza dell'interfaccia USB dell'ESP32-S3 sui pin DP/DM del chip CH224K, impedendogli di bloccarsi in modalitÃƒÆ’Ã†â€™Ãƒâ€ Ã¢â‚¬â„¢ÃƒÆ’Ã¢â‚¬Å¡Ãƒâ€šÃ‚Â  legacy BC1.2/QC 5V e sbloccando la negoziazione USB-PD 20V sui pin CC.

### [0.1.61] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-08-08
- **Estesa Finestra Temporale di Negoziazione PD (2.0s)**: Aggiunta un ciclo di polling a campionamento continuo (10 letture ogni 200ms) durante ogni step di tensione (20V/15V/12V/9V) per concedere agli alimentatori USB-PD 67W / GaN il tempo necessario (1.0-1.5s) per eseguire la negoziazione sui cavi CC ed erogare i 20V.

### [0.1.60] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-08-08
- **Lettura VBUS via Modulo Assistito**: Quando l'ADC locale della Base ÃƒÆ’Ã†â€™Ãƒâ€ Ã¢â‚¬â„¢ÃƒÆ’Ã¢â‚¬Å¡Ãƒâ€šÃ‚Â¨ disabilitato (GPIO 34 non ÃƒÆ’Ã†â€™Ãƒâ€ Ã¢â‚¬â„¢ÃƒÆ’Ã¢â‚¬Å¡Ãƒâ€šÃ‚Â¨ un canale ADC su ESP32-S3), la Base interroga via I2C lo Speaker/modulo collegato (`CMD_GET_VBUS_VOLTAGE`) per mostrare il VBUS reale nei log e sul cruscotto dell'App.
- **Reset Automatico della Negoziazione PD**: Modificato lo stato di negoziazione USB-PD (`s_pd_negotiation_done = false`) in caso di fallimento o ricollegamento del modulo, permettendo alla Base di ritentare l'aggancio ai 20V non appena lo Speaker aggiornato viene rilevato.
- **Supporto Redirect HTTP 302 FOTA**: Aggiunto `.max_redirection_count = 5` al client HTTPS di `ota_manager.c` per supportare i redirect 302 di GitHub Releases ed S3.

### [0.1.59] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-20
- **Updated Default FOTA Color**: Bumped default FOTA update LED indicators to bright orange (`R=255, G=100, B=0`), making it stand out clearly in the UI.

### [0.1.58] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-18
- **FOTA Updating Color Customization**: Added support for customizing the LED blink color during FOTA firmware updates (both Base and modules) via the newly extended `SET_LED_CONFIG` JSON payload.

### [0.1.57] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-18
- **Base LED Customization & NVS Persistence**: Added support for setting global solid colors with brightness/intensity controls and advanced state-specific colors (Provisioning, WiFi Connected, Ready, and Error states) via WebSocket `SET_LED_CONFIG` command. Configurations are persisted in NVS and restored automatically upon boot.

### [0.1.56] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-16
- **Real-time module telemetry diagnostics logging**: Added automatic console (UART/TCP) diagnostic logs when any connected module reports hardware warnings or errors (such as AHT21 errors, ENS160 connection failure, or E-Paper timeout).
- **Log module validity and warm-up state changes**: Added real-time tracking and logging of ENS160 gas sensor state changes (Warm-up, Start-up, Invalid output, or normal operations) to simplify remote diagnostics when the sensor is burning-in.

### [0.1.55] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-16
- Added TCP remote log server on port 1234: all ESP_LOG output is forwarded in real-time to any connected TCP client (e.g. `telnet <base-ip> 1234` or `read_log.ps1`).
- UART output is preserved in parallel ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â no functionality change if no client is connected.
- Server starts automatically after Wi-Fi connection is established; handles one client at a time with automatic reconnection support.

### [0.1.54] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-16
- Disabled Wi-Fi power saving mode (WIFI_PS_NONE) on startup to ensure instant connection responses and eliminate WebSocket timeouts during discovery/scan.

### [0.1.53] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-10
- Enabled complete erasure of stored Wi-Fi credentials when the Base is decoupled/unpaired from the app.
- Triggered automated ESP32 reboot 1 second after decoupling to return the device to its virgin/provisioning state.

### [0.1.52] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-09
- Added user account email lock pairing persisted in Base NVS flash memory.
- Handled REGISTER_USER and UNREGISTER_USER WebSocket commands for secure pairing/unpairing.
- Serialized active registered email into Base WebSocket status JSON.

### [0.1.51] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-09
- Added support for routing SET_BASS and SET_TREBLE commands to Bluetooth Speaker over I2C.
- Included bass and treble values in the speaker status cJSON WebSocket packet.

### [0.1.50] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-07
- Added support for reading and forwarding I2C and EPD error telemetry in cJSON WebSocket packet.

### [0.1.49] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-07
- Added AQI (Air Quality Index) field serialization to environmental sensor WebSocket JSON.

### [0.1.48] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-07
- Added I2C polling support for Environmental Monitor module type (0x05).
- Implemented serialization of environmental telemetry (temperature, humidity, TVOC, eCO2, and pressure) to the WebSocket status packet payload.

### [0.1.47] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-04
- Added I2C polling support for LED Tower module type (0x08).
- Implemented serialization of LED Tower brightness state to the WebSocket status packet payload.

### [0.1.46] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-02
- Resolved clangd warnings in data_broker.c by fixing float-precision casts and removing unused crt header.

### [0.1.45] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-01
- Reduced I2C polling interval to 1.0 second and optimized discovery routine to run only once every 5 seconds, resulting in immediate metadata and song updates.
- Refined remaining track time interpolation to compute it based on total duration, eliminating fluctuations.

### [0.1.44] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-01
- Redesigned progression interpolation to filter out stale/lagging AVRCP play position packets and prevent the progress bar from jumping back and forth.

### [0.1.43] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-30
- Reverted I2C polling interval to 2.0s and WebSocket broker interval to 1.0s to prevent I2C bus timeouts and collisions.
- Implemented client-side progress interpolation in the web player for smooth 1-second track updates.
- Embedded changelog in the web client as a fallback to resolve fetch errors when loaded via HTTP.

### [0.1.42] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Reduced I2C polling and WebSocket data broker intervals to 500ms for near real-time dashboard updates.

### [0.1.41] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Reduced I2C discovery and status polling interval to 1.0 second for smoother dashboard updates.

### [0.1.40] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Fixed FOTA status polling task lockup: added checking for `OTA_STATE_IDLE` to break from the `module_ota_task` loop. This prevents the Master from getting stuck polling the Slave's OTA status, which paused regular I2C manager polling and blocked all manual player controls and metadata updates.
- Improved web player UI layout to display Title, Artist, and Album separately.

### [0.1.39] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-24
- Implemented polling and WebSocket forwarding for Bluetooth A2DP/AVRCP track metadata (title, artist, album) from the speaker slave module.

### [0.1.38] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-22
- Rearchitected module firmware update process: replaced I2C chunk streaming with direct Wi-Fi FOTA updates, transmitting Wi-Fi credentials and firmware URLs to target slaves.
- Implemented status polling loop and WebSocket broadcast to update the frontend UI.

### [0.1.37] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Implemented robust transmit-then-poll streaming protocol for module OTA updates over I2C, resolving communication NACK errors during slave flash write operations.

### [0.1.36] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Paused I2C manager early in the module OTA task to avoid concurrent polling during the HTTP connection/handshake phase.

### [0.1.35] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Implemented dynamic I2C OTA phase-shift recovery: detects if the Slave sends a stale/misaligned response chunk index (due to a previous timeout) and performs a clean read transaction without transmitting, clearing the stale queue and restoring proper frame alignment.
- Allowed `i2c_manager_send_frame` to perform read-only transactions (when `tx` is NULL) on both standard and no-offset addresses.

### [0.1.34] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Refined I2C read length behavior: allowed reading extra bytes (`rx_len + 48`) during non-data commands (like `SUB_OTA_BEGIN` and `SUB_OTA_END`) to clear any stale frames from the Slave's TX FIFO.

### [0.1.33] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Added compatibility delay (220ms wait) for older Slave modules (versions below 20/0.1.10) to accommodate slower flash writes and LED refreshes during I2C streaming updates.

### [0.1.32] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Implemented I2C polling optimizations and WebSocket message handling for Bluetooth Speaker controls.

### [0.1.31] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Minor internal updates.

### [0.1.20] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-12
- Increased default I2C command response wait time to 50ms for improved communication stability.

### [0.1.19] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-12
- Fixed I2C OTA delay logic for legacy slave firmware.

### [0.1.18] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-12
- Implemented dynamic delay compatibility for slave modules running v0.1.6.

### [0.1.17] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-12
- Optimized Base OTA_BEGIN wait time to 5000ms.

### [0.1.16] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-12
- Fixed I2C OTA Chunk 0 timeout: removed slave 200ms delay and increased master wait to 30ms.

### [0.1.15] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-12
- Increased HTTP client buffer size to 4096 bytes to support long redirect URLs from GitHub releases to AWS S3.
- Implemented HTTP client redirection handling (up to 5 redirects).

### [0.1.14] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-12
- Preparatory updates for HTTP client redirection.

### [0.1.13] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-12
- Initial release containing I2C polling pause during Slave OTA updates and orange blinking LED support.

### [0.1.12] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented real I2C OTA firmware streaming protocol over I2C (BEGIN/DATA/END sub-commands).
- Configured dynamic I2C response wait times depending on the specific sub-command.
- Increased `module_ota_task` stack size to support full HTTPS download and streaming.

### [0.1.11] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Bumped project version to coordinate with module update.

### [0.1.10] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Implemented CMD_GET_INFO (0x02) request in polling loop and post-OTA sequence to refresh module type and versions.

### [0.1.9] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Increased CMD_FIRMWARE_UPDATE I2C response delay to 50ms to ensure slave NVS write completion.

### [0.1.8] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Added support for asynchronous I2C module OTA progress and changelog rendering.
- Resolved I2C response length validation issues for module firmware update command.
- Display specific module type and version in web application.
- Changed default base IP to 192.168.1.178.

### [0.1.7] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Added `CMD_GET_VBUS_VOLTAGE` I2C command to query VBUS voltage readings from connected modules.
- Implemented module-assisted USB-PD power negotiation when the Base's internal ADC is unavailable.

### [0.1.6] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Handled invalid VBUS ADC pin on Base gracefully to prevent initialization errors.
- Set default Base VBUS voltage reporting to 5V (5000 mV) when the internal ADC is not available.

### [0.1.5] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Fixed HTTP client buffer overflow error during FOTA updates by increasing buffer size.
- Fixed front-end JavaScript console exception when WebSocket connection is closed.

### [0.1.4] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Incremented version to 0.1.4 for FOTA update flow verification.

### [0.1.3] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Added real-time FOTA update progress tracking and detailed error reporting to the app dashboard.
- Configured asynchronous background task for OTA updates to avoid blocking WebSockets.
- Added default SSL CA certificate bundle (`esp_crt_bundle.h`) to verify HTTPS connections.

### [0.1.2] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Added VBUS voltage monitoring on serial output every 10 seconds.

### [0.1.1] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-03
- Bumped software version of Modulo Base (`base_app`) to `0.1.1`.
- Updated version display and formatting in mobile app dashboard and update manager.

### [0.1.0] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-03
- First functional firmware release for the Modulo Base.
- Wi-Fi provisioning via BLE (NimBLE).
- OTA firmware update support with dual OTA partitions and rollback.
- WebSocket-based communication with the mobile app.
- Module hot-swap detection and lifecycle management.
- Power distribution and communication bus management.

---

## Modulo Bluetooth Speaker

### [0.1.31] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-08-08
- **Calibrazione ADC VBUS e GPIO 36**: Riconfigurato il pin di lettura ADC VBUS sul GPIO 36 (`SENSOR_VP`, Pin 4) con divisore resistivo ricalibrato (`(18k + 2.7k) / 2.7k = 7.6667f`).
- **Supporto Redirect HTTP 302 FOTA**: Aggiunto `.max_redirection_count = 5` alla configurazione dell'HTTPS Client per permettere lo scaricamento da GitHub Releases / AWS S3.
- **Fix Asset URL Manifest**: Corretto il nome del file binario in `manifest.json` in `speaker.bin`.

### [0.1.30] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-09
- Implemented software equalization (Direct Form I Biquad filters) for low-shelf (Bass) and high-shelf (Treble) filters.
- Handled CMD_SPK_SET_BASS and CMD_SPK_SET_TREBLE I2C commands.
- Updated CMD_SPK_GET_STATUS I2C payload to report current Bass and Treble settings.

### [0.1.29] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-04
- Spared and bumped version to 0.1.29 to keep aligned with LED Tower release.

### [0.1.28] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-03
- Migrated default pinout mappings for ESP32 target (SDA=27, SCL=14, ADC=36, LED=19, I2S standard pins) to align with the new hardware revision schematic.

### [0.1.27] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-01
- Filtered out 0xFFFFFFFF play status query payloads from the phone to prevent tracking errors.

### [0.1.26] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-30
- Reverted periodic Bluetooth AVRCP status query interval to 1.0 second to ensure system stability and avoid bus conflicts.

### [0.1.25] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Reduced AVRCP play status query interval to 500ms for near real-time track progress updates.

### [0.1.24] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Reduced periodic AVRCP play status query interval to 1.0 second for smoother real-time track progress.

### [0.1.23] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Implemented classic Bluetooth AVRCP play status polling (position and duration queries) and exposed progress and remaining track time values over I2C status packet.

### [0.1.22] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Freed Bluetooth and I2S resources during OTA to prevent Out-Of-Memory (OOM) heap segment allocation freeze at 4% progress.

### [0.1.21] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Migrated legacy I2C Slave driver to the new ESP-IDF v5.x driver (`driver/i2c_slave.h`).

### [0.1.20] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-29
- Cleaned up Bluetooth compiler warnings.

### [0.1.15] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-24
- Reset Slave OTA state to IDLE upon receiving SSID.

### [0.1.14] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-24
- Implemented full Bluetooth Classic A2DP Sink and AVRCP Controller stack.
- Configured software-based volume control using Q8 fixed-point quadratic curve.
- Enabled track metadata transmission via new `CMD_SPK_GET_METADATA` command.
- Integrated aggressive compile-size and IRAM optimizations to prevent memory segment overflow.

### [0.1.13] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-22
- Test release for FOTA update verification with 3-byte SemVer protocol.

### [0.1.12] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-22
- Implemented direct Wi-Fi FOTA update capability: connects to Wi-Fi STA and executes HTTPS OTA in a background task, keeping the I2C bus responsive.

### [0.1.11] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Implemented automatic I2C driver recovery to clear hardware bus lockups/hangs: distinguished driver/communication errors (negative return values) from standard idle timeouts. When an error is detected, the Slave automatically de-initializes and re-initializes its I2C driver to reset the hardware peripheral and release the SDA/SCL lines.

### [0.1.10] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Implemented Bluetooth volume control (0-100), song titles, progress tracking, and listening states.
- Optimized I2C OTA updates by throttling LED status strip refresh rate (once every 500ms) to eliminate high latency and avoid blocking I2C transactions.

### [0.1.9] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Skeletons and commands for bluetooth controls (play, pause, next, prev, volume up/down).

### [0.1.8] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Skeletons for bluetooth controls.

### [0.1.7] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Skeletons and build configurations.

### [0.1.5] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.
- Switched partition table layout to two-OTA partitions.

### [0.1.4] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Removed `s_module_type` loading from NVS to prevent stale/incorrect module type reports.

### [0.1.3] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented CMD_GET_INFO (0x02) command to return unique chip ID, module type, and hardware/software versions.

### [0.1.2] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Deferred NVS flash writes in CMD_FIRMWARE_UPDATE response to prevent blocking the I2C transaction.

### [0.1.1] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-03
- Bumped software version of Modulo Slave (`slave_app`) to `0.1.1`.
- Updated version display and formatting.

### [0.0.1] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-03
- Initial setup.

---

## Modulo Environmental Monitor
### [0.1.94] - 2026-09-21
### Fixed
- Rimossa la rotazione automatica ogni 60s: il display rimane sulla schermata selezionata.
- Aggiornamento periodico impostato a 5 minuti (300s) oppure istantaneo al cambio di soglia colore AQI.
- Risolta lettura dati sensore ENS160 (supporto aria pulita TVOC = 0 ppb, baseline eCO2 400 ppm, recupero STATAS).
- Stabilizzata compensazione T/H ogni 30s.

### [0.1.90] - 2026-09-21
### Added
- Firmware diagnostico: toggling RST (GPIO 16) ogni 2s per verifica multimetro.
- Scansione I2C master automatica su GPIO 17 (SDA) e GPIO 5 (SCL).
- Lettura telemetrica in tempo reale dello stato logico di tutti i pin su connettore FPC 15 pin.

### [0.1.89] - 2026-09-18
### Fixed
- Ripristinata mappatura fisica connettore FPC 15 pin (PINOUT_EINK_SENSORI.md): BUSY=18, RST=16, DC=4, CS=15, CLK=23, DIN=22, SDA=17, SCL=5.
- Risolto spegnimento prematuro (100ms) del display prima del completamento dell'aggiornamento chimico: garantiti 12 secondi continui di booster ad alta tensione.
- Rimossa configurazione pull-down conflittuale con la resistenza hardware R8 (4.7k) su scheda madre.

### [0.1.88] - 2026-09-18
### Fixed
- Risolto conflitto e mappatura pin GPIO per display e-paper e sensori ambientali: ripristinati i pin hardware corretti (BUSY=4, RST=16, DC=17, CS=5, CLK=18, DIN=23, SDA=21, SCL=22).
- Corretto stato pull-down sul pin BUSY (GPIO 4) eliminando i timeout di refresh.
- 6 Schermate ufficiali Arduino Test_2in9_G.ino attive e cicliche ogni 60s.

### [0.1.86] - 2026-09-18
### Fixed
- Corretta la polarita' del pin BUSY per il pannello Waveshare 2.9" (G) (LOW=Occupato, HIGH=Pronto).
- Aggiunta la sequenza completa di registri di configurazione hardware e risoluzione 128x296 (comando 0x61).
- Implementato driver SPI bit-banging con massima forza di pilotaggio (GPIO_DRIVE_CAP_3) attraverso connettore FPC 15 pin.

### [0.1.85] - 2026-09-18
### Added
- Driver hardware e-Paper Waveshare 2.9" (G) 4-colori (Nero, Bianco, Giallo, Rosso e retinatura ottica per il verde).
- 5 Viste E-Paper integrate: Home, Air Quality, Comfort, Detailed e Screensaver (pianta vettoriale).
- Acquisizione sensori I2C Master ENS160 (TVOC, eCO2, AQI) e AHT21 (Temperatura, Umidita') su connettore FPC 15-pin.
- Aggiornamento periodico display e refresh immediato su comando I2C dalla Base / App.

### [0.1.84] - 2026-09-16
### Added
- Integrazione completa del nuovo driver nativo Waveshare 2.9" (G) a 4 colori (Nero, Bianco, Giallo, Rosso + Verde con retinatura ottica 75% Giallo / 25% Nero).
- Supporto connettore FPC Pin 4..15 con segnali contigui: BUSY (GPIO 18), SCL (GPIO 5), SDA (GPIO 17), RST (GPIO 16), DIN (GPIO 22), CLK (GPIO 23), CS (GPIO 15), DC (GPIO 4), 3V3 (Pin 12-13) e GND (Pin 15).
- Unificazione dinamica della schermata Air Quality (Viste 1 e 3) parametrizzata sui livelli 1 (Excellent - Verde), 2 (Fair/OK - Giallo) e 3 (Poor/Bad - Rosso).
- Visualizzazione rigorosamente legata ai dati reali dei sensori ENS160 + AHT21 (nessun dato fittizio / mock). Se il sensore non risponde compare "--.-" / "NO SENSOR".
- Selezione dinamica delle schermate da remoto via bus I2C (CMD_SCREEN_SET_VIEW e CMD_SCREEN_GET_VIEW) con vista predefinita di avvio impostata su Screen 1 (Home).



### [0.1.74] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-23
- **Restored HTTPS Certificate Bundle for FOTA**: Restored `.crt_bundle_attach = esp_crt_bundle_attach` in `slave_ota_task`'s HTTP configuration. This was accidentally omitted in `0.1.71` during buffer configurations, which caused HTTPS handshakes with the GitHub release server to fail and abort all FOTA updates.

### [0.1.73] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Fixed ENS160 Zero Readings and Polling**: Consolidated three separate I2C register reads (AQI, TVOC, eCO2) into a single 6-byte burst-read transaction starting from register `0x21` (matching Adafruit's standard library approach). Removed the unreliable `NEWDAT` bit checks in the status register which frequently remained low and blocked all telemetry updates.

### [0.1.72] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Fixed E-Paper Blank Screen and restored DC Line State**: Restored the Data/Command (DC) line state to HIGH (DATA) immediately after command transmission completes, matching GxEPD2's exact hardware driver signaling. Leaving the line LOW placed the SSD1680 controller in a perpetual command listening state, rendering the display unresponsive. Increased the SPI clock frequency to 4 MHz to align with GxEPD2 defaults.

### [0.1.71] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Fixed FOTA Loop and Retry Failures**: Resolved recurrent FOTA retry failures where subsequent FOTA attempts failed immediately. Fixed the issue by preserving the default event loop (removed `esp_event_loop_delete_default()` from FOTA task cleanup, which broke global event dispatching) and properly unregistering event instances instead.

### [0.1.70] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Aligned E-Paper Driver with GxEPD2 Logic**: Removed pull-down resistor from BUSY pin config (`pull_down_en = 0`), keeping it as a floating/high-impedance INPUT. Replaced BUSY pin polling during reset/SWRESET with static 10ms delays, mirroring GxEPD2's startup sequence. Trimmed partial update LUT (`_WF_PARTIAL_2IN9`) to 153 bytes and limited `epd_write_lut` to only write those 153 bytes, avoiding corrupting analog voltage registers (VBorder, Gate, Source, VCOM) that were overloading the display charge pump and sagging the 3.3V rail.

### [0.1.69] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Removed SPI DMA and Implemented Chunking**: Disabled SPI DMA (`SPI_DMA_DISABLED`) to eliminate bus-matrix memory conflicts and voltage/noise sags on the 3.3V rail. Rewrote `epd_write_data_buffer` to automatically split transmissions into max 64-byte chunks (matching the ESP32 hardware FIFO limit) while keeping the CS line LOW to present a single seamless transaction to the display. This resolves both the E-Paper busy timeouts and the ENS160 I2C master failures.
- **Combined Error Flags**: Merged the sensor manager's error flags (`sensor_mgr_get_error_flags()`) with the E-Paper's flags (`epd_get_error_flags()`) so that the Base can correctly diagnose and display the state of both subsystems.

### [0.1.68] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Wi-Fi FOTA Sequence and Timing Fixes**: Fixed recurrent FOTA connection failures by initializing network interfaces (`esp_netif_init` and `esp_event_loop_create_default`) once at boot in `app_main` to prevent duplicate initialization crashes. Reordered `slave_ota_task` execution to terminate the sensor task and free memory stack before refreshing the display, and added a 5-second stabilization delay before starting the Wi-Fi interface. This guarantees the e-Paper's high-voltage refresh cycle completes and its current draw drops to zero before the Wi-Fi radio powers on, preventing power sags and RF connection failures.

### [0.1.67] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Restored Real Sensor Readings**: Bypassed the Hello World debug loop in `env_sensor_task` and re-enabled full initialization and reads of the physical AHT21 and ENS160 sensors. Updates the local e-Paper dashboard and transmits real telemetries to the Base.
- **E-Paper Alignment with GxEPD2**: Activated SPI DMA (`SPI_DMA_CH_AUTO`) and aligned the framebuffer and LUT buffer structures in memory (4-byte alignment). Rewrote the `epd_update_display()` sequence to configure the RAM windows (`0x44` and `0x45`) and reset RAM counters (`0x4E` and `0x4F`) before every data transmission. Writes to `0x26` before `0x24` to match GxEPD2's exact SSD1680 controller update logic and fix the blank screen on production PCBs.

### [0.1.66] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Wi-Fi FOTA Connection Reliability Improvements**: Added an automatic Wi-Fi reconnect retry mechanism (up to 5 attempts) in the Slave's `wifi_event_handler` on disconnection events. Configured PMF (Protected Management Frames) settings (`pmf_cfg.capable = true`, `pmf_cfg.required = false`) for seamless connection to WPA3/WPA2 Mixed mode APs, removed strict authorization thresholds, and increased the connection wait timeout from 15 to 30 seconds. This resolves transient connection timeouts and failures during the initialization phase of FOTA.

### [0.1.65] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-22
- **Restored Production Pinout**: Reverted EPD pins back to the production PCB pinout (`CONFIG_SLAVE_DEVICE_BREADBOARD_PINOUT=n`, which uses `DC=25`, `RST=26`, and `BUSY=32`), as confirmed correct by the user. Kept the software timing fixes (1ms busy wait delay, 2ms reset duration, and 0xCC refresh sequence) to solve the black screen issue on the production hardware.

### [0.1.64] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-20
- **Breadboard Pinout Selection**: Enabled the breadboard/Arduino pinout (`CONFIG_SLAVE_DEVICE_BREADBOARD_PINOUT=y`) by default to use `DC=17`, `RST=16`, and `BUSY=4`, matching Mauro's prototype wiring.
- **Busy wait margin and display update fix**: Added 1ms delay in `epd_wait_busy()` to allow the SSD1680 controller to pull the BUSY pin HIGH before reading it. Corrected the partial update display command parameter to `0xCC` (matches GxEPD2).

### [0.1.63] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-20
- **Waveshare Reset timing Fix**: Restored the 2ms short reset pulse duration (`esp_rom_delay_us(2000)`) in `epd_reset()`. The Waveshare "clever" power-transistor reset circuit cuts off VCC power to the display if RST is held low for too long (e.g. 20ms), which caused the display to brown out and fail to initialize. This matches the exact timing in Mauro's working GxEPD2 Arduino sketch.

### [0.1.62] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-20
- **SPI DMA Disabled**: Disabled SPI DMA (using `SPI_DMA_DISABLED`) to eliminate strict 4-byte buffer alignment requirements and stack-allocation constraints for the transaction buffers. This resolves the black/blank screen issue caused by SPI transfer failures on ESP32 when sending stack-allocated LUTs and unaligned framebuffers.

### [0.1.61] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-20
- **Minimal Test Hello World Screen**: Simplified the env_sensor_task to bypass all sensor initialization/reads and loop with a minimal "Hello World!" drawing sequence, showing an incrementing refresh counter on screen. Used full refreshes to verify basic screen functionality and isolate hardware/bus issues.

### [0.1.60] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-20
- **Stable Reset Timing and SPI DMA**: Adjusted the hardware reset sequence to use 20ms, 20ms, and 200ms delays to guarantee the SSD1680 display chip completes its internal power startup. Enabled SPI DMA (auto-allocated channel) and updated bulk data writes to use a single high-speed SPI transaction, ensuring gapless clock transitions. Stack-buffered the LUT data transfer to bypass ESP32 DMA flash reading limits.

### [0.1.59] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-20
- **SPI Chip Select CS Transmission Fix**: Optimized the SPI transmission flow to maintain `CS` low during the entire write transaction of the 4736-byte framebuffer (0x24 and 0x26) and 153-byte LUT. This matches the exact SPI communication timing of the GxEPD2 library, resolving a critical issue where pulling CS high/low on every byte would cause the SSD1680 controller to reset its internal registers and remain blank.
- **PCB Pinout Restored as Default**: Configured the standard PCB layout (DC=25, RST=26, BUSY=32, I2C SDA=21, SCL=22) as the default compilation configuration.

### [0.1.58] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-20
- **Configurable Pinouts & Kconfig Compatibility**: Added Kconfig configuration options for all local Environmental Monitor peripherals, including E-Paper pins (CLK, DIN, CS, DC, RST, BUSY) and local I2C Master pins (SDA, SCL).

### [0.1.57] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-18
- **Fixed E-Paper initialization freeze**: Adjusted the hardware reset pulse duration to exactly **2ms** (using precise `esp_rom_delay_us`), matching the exact configuration used by the working GxEPD2 Arduino library (`display.init(115200, true, 2, false)`). Preceded the pulse by a 10ms VCC power stabilization delay (RST HIGH) and followed it by a 15ms stabilization delay. This prevents the Waveshare "clever" power-transistor reset circuit from cutting off VCC power to the display panel, which was causing the display chip to enter brownout or fail to initialize when using long (200ms) reset pulses.

### [0.1.56] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-17
- **Fixed E-Paper blank screen issue**: Resolved incompatibility with newer Waveshare 2.9" rev2.1 displays mounting the SSD1680Z chip. Switched the full display update control option from `0xC7` to `0xF7` which triggers automatic internal LUT loading and temperature compensation directly from the OTP memory. Added writing the frame data to both memory banks (0x24 BW RAM and 0x26 Previous RAM) as required by the SSD1680Z differential refresh controller. Removed manual/static LUT loading on full updates.
- **Fixed ENS160 zero-readings during warm-up**: Refactored the reading routine to check the `NEWDAT` flag (bit 1 of STATUS register 0x20) and `Validity` bits (bits 2-3) before reading data registers. Stored last valid measurements in static variables to return them during the 3-minute warm-up phase (Validity = 1 or 2) and when no new samples are available, preventing the values from dropping to zero in the UI.

### [0.1.55] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-17
- **Glitch Filter for E-Paper and Release Bump**: Bumps version to 0.1.55 to trigger FOTA cleanly, integrating the EPD busy pin glitch filter that prevents false Healthy status on unpowered/disconnected displays.

### [0.1.54] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-17
- **Fixed ENS160 Reset and Initialization Delays**: Increased the software reset wait from 20ms to 100ms, and the STANDARD mode transition wait from 10ms to 100ms, adhering strictly to ScioSense communication guidelines. This guarantees the sensor's bootloader completes execution and stabilizing before active sensing writes are sent.

### [0.1.53] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-17
- **FOTA Version Bump**: Bumps the Environmental Monitor firmware to 0.1.53 to trigger a clean FOTA update cycle for remote register diagnostics.

### [0.1.52] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-17
- **Added Remote Sensor Diagnostics packing**: Packed the raw ENS160 `OPMODE` register (bits 6-7) and `STATUS` register (bits 8-15) directly into the unused upper bits of the 16-bit `error_flags` field. This enables remote diagnostics of the sensor state over WebSocket/TCP without requiring physical UART serial access to the slave board.

### [0.1.51] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-17
- **Implemented Passive E-Paper Presence Detection**: Replaced the timing-sensitive active startup reset checks with a reliable passive detection strategy. The EPD driver now starts with the assumption that the screen is connected, allowing SPI commands to execute without block. It tracks if the BUSY pin goes HIGH at any point during operation (such as during the mandatory screen refresh at boot). If after boot the BUSY pin is never observed HIGH, the `EPD_ERR_BUSY_TIMEOUT` is raised, correctly flagging a disconnected screen without false timeout alerts on working screens.

### [0.1.50] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-17
- **Fixed E-Paper false busy-timeout alert**: Fixed an issue where a correctly connected and powered E-Paper screen was reported with a Busy Timeout. The hardware reset timing was too slow for a simple check, causing the presence check to miss the brief high state of the BUSY pin. The initialization routine now checks display presence by sending a digital Software Reset (0x12) command and polling the BUSY pin at 100-microsecond intervals for up to 10 milliseconds, catching the busy pulse reliably.

### [0.1.49] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-17
- **Fixed ENS160 data initialization lock**: Fixed an issue where the ENS160 digital gas sensor would remain in a command-executing state after reading its firmware version (which registers as Validity 0 but outputs all zeros for TVOC/eCO2). The command register (0x12) is now correctly reset to NOP (0x00) following the app version retrieval, allowing the sensor to execute normal sensing algorithms in STANDARD mode.
- **Fixed E-Paper disconnection masking**: Fixed a bug where EPD busy wait routine (`epd_wait_busy`) would clear the `EPD_ERR_BUSY_TIMEOUT` flag if it didn't hit a timeout, which masked disconnected/unpowered displays. The driver now tracks physical display presence (`s_epd_present`) established during initialization loopback test and maintains the error flag if absent.

### [0.1.48] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-16
- **Fixed ENS160 permanent warm-up**: The ENS160 gas sensor requires external temperature and humidity compensation data (registers `TEMP_IN` 0x13 and `RH_IN` 0x15) to complete its initialization and exit the warm-up phase. Without these writes, the sensor stays at validity=0 (warm-up) indefinitely and returns all-zero readings for TVOC, eCO2, and AQI. Now, every sensor polling cycle writes the real AHT21 temperature and humidity values (or reasonable defaults of 25ÃƒÆ’Ã†â€™ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡ÃƒÆ’Ã¢â‚¬Å¡Ãƒâ€šÃ‚Â°C/50% if AHT21 is unavailable) to the ENS160 compensation registers using the ScioSense format: `TEMP_IN = (TÃƒÆ’Ã†â€™ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡ÃƒÆ’Ã¢â‚¬Å¡Ãƒâ€šÃ‚Â°C + 273.15) ÃƒÆ’Ã†â€™Ãƒâ€ Ã¢â‚¬â„¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬ÃƒÂ¢Ã¢â€šÂ¬Ã‚Â 64`, `RH_IN = RH% ÃƒÆ’Ã†â€™Ãƒâ€ Ã¢â‚¬â„¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬ÃƒÂ¢Ã¢â€šÂ¬Ã‚Â 512`, LSB-first.
- **Fixed E-Paper "Healthy" status when display is disconnected**: The existing RSTÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬ÃƒÂ¢Ã¢â‚¬Å¾Ã‚Â¢BUSY diagnostic test (3 cycles of toggling RST and reading BUSY) was running but its results were never used to set error flags. Since GPIO 32 (BUSY) has an internal pull-down, an absent display reads as always-LOW, never triggering the `epd_wait_busy()` timeout. Now, if the BUSY pin never goes HIGH during any of the 3 diagnostic cycles, the `EPD_ERR_BUSY_TIMEOUT` flag is immediately set, correctly reporting the display as absent.

### [0.1.47] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-16
- **Version bump**: Bumps software version to 0.1.47 to trigger a clean over-the-air (FOTA) update cycle on the Environmental Monitor and clear cached registry versions on the Base.

### [0.1.46] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-16
- **Improved ENS160 boot recovery and reliability**: Added software reset (`0xF0`) to OPMODE register (`0x10`) and boot delay during address probing (`0x53` and `0x52`) to match official ScioSense initialization sequence, ensuring clean state recovery even after hot-plugging or soft resets.
- **Made sensor initialization independent**: Refactored the driver to initialize and poll the AHT21 (temperature/humidity) and the ENS160 (air quality) sensors independently. A failure or absence of one sensor no longer locks the other sensor or causes the entire board initialization to fail.

### [0.1.42] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- **Added sensor power stabilization delay**: Added a 150ms delay at the very beginning of `sensor_mgr_init` before performing any I2C communication. This allows the 3.3V power rail and the ENS160 internal boot logic to stabilize on cold power-on, preventing the sensor from getting locked in its bootloader state by premature I2C transactions.

### [0.1.41] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- **Fixed ENS160 mode transition lock**: Added a NOP (0x00) command write to the `COMMAND` register (0x12) after version retrieval and before setting STANDARD mode. The ENS160 internal command handler requires the register to return to NOP to allow the OPMODE transition from IDLE to STANDARD.

### [0.1.40] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- **Fixed ENS160 boot ready detection**: Changed bootloader ready wait logic to issue the official `GET_APPVER` command (0x0E) and verify the version returns a non-zero value, ensuring we wait until the sensor has fully loaded its internal firmware before starting standard mode.

### [0.1.39] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- **Added hardware diagnostics and raw data logging**: Added an EPD RST-to-BUSY pin loopback test at startup to verify physical connections and power. Added pre-init register dump for the ENS160 gas sensor, along with raw reading logs for both AHT21 (temp/humidity) and ENS160 (TVOC/eCO2 bytes) to isolate and diagnose sensor and display activity.

### [0.1.38] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- **Fixed OTA abort/reset crash**: Added a 300ms delay after deleting `env_sensor_task` at the start of FOTA. This gives the FreeRTOS idle task time to actually deallocate the 8KB task stack before starting the heavy Wi-Fi driver, avoiding Out-Of-Memory (OOM) allocations.

### [0.1.37] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- **Fixed ENS160 boot ready lock**: Implemented the official ScioSense startup handshake loop (NOP 0x00 + CLRGPR 0xCC, reading GPR_READ_4..6 (0x4C) until they are all 0) to ensure the internal MCU bootloader is ready before transitioning the chip into continuous measurement mode. Also guaranteed the mandatory transition from IDLE to STANDARD mode during automatic register checks/restores.
- **Fixed e-Paper blank screen**: Added the official Waveshare V2 Look-Up Table (LUT) loading sequence for both full (WS_20_30) and partial (_WF_PARTIAL_2IN9) refresh modes, enabling the SSD1680 controller to generate proper driving voltages.

### [0.1.36] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- **Fixed ENS160 measurement engine not starting**: Added mandatory COMMAND register writes (NOP 0x00 + CLRGPR 0xCC to register 0x12) during sensor initialization. Without these commands, the sensor accepts OPMODE=STANDARD but never activates its internal heater/measurement engine (STATAS bit stays 0), resulting in perpetual TVOC=0, eCO2=0, AQI=0 readings.
- **Fixed e-Paper display not refreshing**: Corrected Display Update Control 2 values to match official Waveshare V2 driver (0xC7 for full refresh, 0x0F for partial). Previous values (0xF7/0xFF) caused the SSD1680 controller to use invalid waveform sequences. Also added 10ms delay after SW reset per datasheet requirements, and RAM address counter initialization (0x4E/0x4F) at end of init sequence.

### [0.1.35] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- Added verbose diagnostics for ENS160 (logs PART_ID, OPMODE, and status registers on every measurement cycle).
- Added return code verification and error logs for e-Paper initialization (`epd_init`) in the main sensor task.

### [0.1.34] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-08
- Fixed E-Paper false-positive "restored" logs: corrected EPD error telemetry status so the busy timeout flag persists until a refresh command completes successfully.
- Added detailed telemetry debugging to trace startup/refresh GPIO levels of the busy pin.
- Optimized hardware reset timing for WaveShare SSD1680 displays (toggled RST pin with 200ms intervals).

### [0.1.33] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-07
- Fixed FOTA crash/abort on standard ESP32: automatically pauses/deletes the heavy environmental sensor and e-Paper drawing tasks before starting Wi-Fi STA and HTTPS OTA, reclaiming 8KB of task stack and preventing Out-Of-Memory (OOM) failures.
- Displays a dedicated "AGGIORNAMENTO FIRMWARE..." message on the e-Paper display during the update process.

### [0.1.32] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-07
- Added I2C and EPD error flags (AHT21 connection, ENS160 connection, ENS160 data validity, EPD busy timeout) to the protocol payload.

### [0.1.31] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-07
- Added a 50ms boot delay after ENS160 I2C reset command to avoid register lockups.
- Improved e-Paper (SSD1680) driver: implemented proper reset sequence (HIGH-LOW-HIGH), pre-initialized pin levels to prevent startup glitches, and reduced SPI clock to 2MHz for high stability.
- Alternates between fast partial refreshes and periodic full refreshes (once per minute) to avoid screen damage and flickering.

### [0.1.30] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-07
- Implemented real-time AQI and eCO2 reading from ENS160 registers and removed simulated sensor fallback.
- Added e-Paper graphic drawing sections for real-time AQI levels and the list of detected gases (VOCs, Toluene, Hydrogen, Ethanol, NO2, Ozone).
- Integrated dual I2C address detection (auto-scanning addresses 0x53 and 0x52) for ENS160.
- Disabled SPI DMA to support safe stack-allocated transactions and resolved e-Paper boot display issue.
- Updated base station serialization and web dashboard interface to display AQI, eCO2, and monitored gas details.
- Removed deprecated atmospheric pressure fields.

### [0.1.29] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-07-07
- Implemented first software release of environmental monitor telemetry and display interface.
- Configured local I2C Master bus on GPIO 21 (SDA) and GPIO 22 (SCL) to read data from ENS160 (TVOC/eCO2) and AHT21 (Temp/Hum) sensors, with automated simulation fallback.
- Configured VSPI bus on GPIO 23 (DIN), GPIO 18 (CLK), GPIO 5 (CS), GPIO 25 (DC), GPIO 26 (RST), and GPIO 32 (BUSY) to drive Waveshare 2.9" portrait e-Paper display.
- Implemented graphics drawing library (with customized thermometer, droplet, wind, and CO2 cloud icons) and progressive TVOC bar graph.
- Exposed telemetry packet structured data (env_sensor_data_t) to Base station via CMD_GET_STATUS (0x03) over I2C Slave interface.

### [0.1.6] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Skeletons and build configurations.

### [0.1.5] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Deferred NVS flash writes.

---

## Modulo Smart Screen 128
### [0.1.45] - 2026-09-23
### Added
- Icone grafiche dedicate ad alta risoluzione (TrueColorAlpha 20x20 e 32x32) per tutte le principali applicazioni: WhatsApp, Telegram, Gmail, Instagram, Messenger, Chiamate, SMS e Calendario.
- Supporto unificato delle icone nei popup a tutto schermo e nei badge sui quadranti orologio digitale e analogico.

### [0.1.44] - 2026-09-23
### Added
- Animazione in dissolvenza morbida (Fade In / Fade Out a 350ms) per comparsa e scomparsa popup notifiche.
- Sincronizzazione cancellazione notifiche smartphone: azzeramento istantaneo badge e popup su rimozione notifica da Android.

### Fixed
- Risolto blocco controller LCD GC9A01 sostituendo disp_on_off con gestione diretta e sicura del Backlight GPIO 21.
- Filtraggio anti-rumore e cooldown su pulsante/touch fisico.

### [0.1.43] - 2026-09-23
### Added
- Spegnimento display universale da pulsante/touch GPIO 32 e da remoto via CMD_SCREEN_SET_POWER.
- Popup notifica a pieno quadrante stile Now Playing (sfondo #16161D, bordo accentuato da 5px).
- Timeout default popup esteso a 9 secondi.
- Icona TrueColorAlpha ufficiale WhatsApp con fumetto e baffetto distintivo rispetto alla chiamata vocale.

### [0.1.42] - 2026-09-23
### Added
- Sensore touch capacitivo su GPIO 32 (I2S_SDATA) per accendere/spegnere display e retroilluminazione.
- Gestione configurabile accensione schermo su notifica con salvataggio in NVS (CMD 0x58).
- Controllo alimentazione display via I2C (CMD 0x57).
- Badge e popup Telegram con colore ufficiale #2AABEE e lettera 'T'.
- Posizionamento popup notifica nella metÃ  inferiore del display circolare.

### [0.1.41] - 2026-09-23
### Added
- Badge notifiche reali dinamici su Vista 02 (Orologio Digitale + Meteo) e Vista 03 (Orologio Analogico).
- Supporto al comando di cancellazione notifiche per ripulire i quadranti dai badge.

### [0.1.39] - 2026-09-18
### Changed
- Rimosso watchdog 30s locale che riportava forzatamente su orologio digitale.
- Salvataggio vista orologio preferita in NVS (Digitale o Analogico).

### [0.1.38] - 2026-09-18
### Changed
- Uniformata la schermata di aggiornamento firmware al design brand Modulo (deep blue #002B70).
- Rimossa logica di auto-revert locale per consentire selezione permanente della vista dall'app.

### [0.1.37] - 2026-09-18
### Added
- Nuova Vista 03: Orologio Analogico Minimal di lusso.
- Lancetta dei secondi a scorrimento ultra-fluido (40 FPS, sweep continuo).
- Tacche perimetrali a 60 divisioni senza numeri numerici.
- Widget data maiuscola, meteo compatto e 3 badge notifiche (WhatsApp, Gmail, Telegram).
- Posizione GPS in basso e rimozione indicatori di paginazione.

### [0.1.36] - 2026-09-17
### Added
- Watchdog multimediale (20s) per intercettare fine o interruzione streaming.
- Fallback di sicurezza assoluto (5 min) per ritorno garantito da Vista Musica a Vista Orologio.
- Ripristino Vista Orologio dopo 30s di assenza comunicazioni master I2C.
- Preservazione persistente indirizzo I2C e NVS per prevenire disconnessioni dello slave.

### [0.1.35] - 2026-09-17
### Added
- Timeout automatico di inattivita 5 minuti per ritorno da Vista Musica a Vista Orologio.

### [0.1.34] - 2026-09-17
### Changed
- Schermata di default impostata su Vista 02 (Orologio + Meteo) all'avvio, al ripristino post-FOTA e come fallback predefinito del sistema.

### [0.1.33] - 2026-09-17
### Changed
- Rimozione totale di qualsiasi etichetta testuale di stato e dicitura dai processi di download FOTA: presente unicamente la corona circolare con percentuale al centro su sfondo #08080E.
- Reset NVS assigned ID su timeout di comunicazione con il master.

### [0.1.32] - 2026-09-17
### Changed
- Rimozione del testo di stato durante il download FOTA: interfaccia essenziale con sola corona circolare e percentuale centrata.

### [0.1.31] - 2026-09-17
### Fixed
- Risolto problema dello sfondo grigio sulle animazioni meteo grazie al passaggio al formato con canale alpha nativo LV_IMG_CF_TRUE_COLOR_ALPHA.
- Risolto mancato avanzamento percentuale sullo schermo durante il FOTA del modulo Smart Screen: avanzamento in tempo reale direttamente dal task di download.
- Ottimizzazione memoria: deallocazione dinamica del buffer copertina (+64.8 KB di heap interno libero).

### [0.1.30] - 2026-09-17
### Added
- Motore di micro-animazioni grafiche per le condizioni meteo (respiro zoom del sole, deriva fluttuante della nuvola, caduta ritmica pioggia, scarica lampo temporale, danza neve).
- Orologio con respiro d'opacitÃ  del separatore ":" a ritmo di 1s.
- Standby a respiro perimetrale e avanzamento fluido traccia su Music View.
- Entrata a molla ("overshoot drop") dei popup di notifica.

### [0.1.29] - 2026-09-17
### Added
- Set completo di icone meteo grafiche standard 24x24 px su LVGL: Sole dorato, Nuvola volumetrica, Pioggia con gocce azzurre, Temporale con fulmine giallo, Neve con fiocchi bianchi.
- Eliminato il sole colorato di grigio in caso di cielo nuvoloso.

### [0.1.28] - 2026-09-10
### Fixed
- Timer di sicurezza e auto-revert (4s su completamento 100%, 25s di timeout) sulla Vista 08 (FOTA) per evitare display bloccato.
- Gestione corretta dell'aggiornamento dinamico di data, ora e meteo in tempo reale su Vista 02.
- Sostituzione delle notifiche statiche con pop-up overlay temporanei universali su lv_layer_top.

### [0.1.27] - 2026-09-10
### Added
- Vista 02 Orologio + Meteo con dati reali e sincronizzazione automatica NTP.
- Pop-up notifiche interattive su overlay (lv_layer_top) con badge applicativi (WhatsApp, Gmail, ecc.).
- Gestione dedicata Vista 08 per FOTA con avanzamento progressivo e ripristino automatico.
### Changed
- Ristrette le schermate selezionabili da utente esclusivamente alle Viste 01 e 02.

### [0.1.26] - 2026-09-10
### Fixed
- Auto-ripristino e riavvio dopo fallimento FOTA per ripristinare la normale schermata display.
- Integrazione completa viste 02, 06, 08.

### [0.1.25] - 2026-09-10
### Added
- Schermata 02: Orologio + Meteo (Ora 48px, Data, icona Sole #F4C95D, temp 18Ã‚Â°, Soleggiato, 12Ã‚Â°/22Ã‚Â°, Torino).
- Schermata 06: Notifiche (WhatsApp, Gmail, Calendario con badge colorati, testi, orari e paginazione).
- Schermata 08: Aggiornamento OTA (Ghiera circolare celeste #4CC9F0, percentuale 68%, info MB e FW version).
- Supporto a tutte le viste da comando I2C CMD_SCREEN_SET_VIEW.

# Changelog

All notable changes to Modulo firmware will be documented in this file, structured by software component.

---


### [0.1.24] - Corona a Bordo Display, Schermo Nero Idle e Inversione Play/Pause
- Posizionata la corona di avanzamento sul perimetro esterno del display circolare (240x240 px).
- Invertito l'indicatore Play/Pause come visualizzatore di stato (ÃƒÂ¢Ã¢â‚¬â€œÃ‚Â¶ in riproduzione, ÃƒÂ¢Ã‚ÂÃ‚Â¸ in pausa).
- Implementato schermo nero e ghiera verde continua a riposo con dicitura 'Nothing playing'.
- Rimossa l'immagine vintage TV e gestito l'azzeramento istantaneo su cambio canzone.
- Esteso il buffer di ricezione copertine JPEG fino a 32 KB.
### [0.1.23] - Copertina Album ad Alta Risoluzione 180x180 px
- Aumentata la risoluzione della copertina album dinamica a 180x180 pixel (+125% di pixel).
- Ottimizzato lo scaling LVGL a 1.33x per una resa visiva ultra definita su LCD circolare 240x240.
### [0.1.22] - Ottimizzazione Coda I2C a 64 Frame e Rimozione Latenza UART
- Raddoppiata la coda di ricezione I2C a 64 slot per prevenire overflow.
- Ridotta la verbositÃƒÆ’Ã‚Â  UART in ricezione frame per elaborazione istantanea senza jitter.
- Aggiunto conteggio chunk ricevuti alla telemetria CMD_GET_STATUS.
### [0.1.21] - Fix Decompressione TJPGD 4:2:0 & Telemetria Copertina Dinamica
- Espanso il buffer di lavoro TJPGD a 4096 byte per supportare immagini JPEG 4:2:0 da iTunes.
- Aggiunta validazione dei chunk I2C accumulati e telemetria diagnostica estesa in CMD_GET_STATUS.
- Invalidazione cache immagine LVGL prima del render a schermo.
### [0.1.20] - Ottimizzazione Ricezione Streaming e Centramento Copertina Dinamica
- Ampliata la coda I2C a 32 frame per prevenire la perdita di pacchetti durante lo streaming della copertina.
- Ottimizzata la decompressione TJPGD in memoria e corretto il calcolo del pivot e centramento LVGL su display 240x240.
- Aggiunta ricostruzione reattiva della schermata Now Playing al completamento del caricamento copertina.
### [0.1.19] - Supporto Copertina Album Dinamica e Decompressione TJPGD
- Aggiunta ricezione streaming I2C e decompressione JPEG in memoria tramite decoder ROM TJPGD per copertine dinamiche 120x120.
- Aggiornato layout vista "Now Playing" con card copertina arrotondata (radius 16), corona circolare 360Ãƒâ€šÃ‚Â° e metadati dinamici.
- Aggiunto comando I2C CMD_SCREEN_LOAD_COVER (0x53) con supporto sotto-comandi START, CHUNK, FINISH, RESET.
### [0.1.18] - Implementazione Vista Now Playing e Gestione Viste I2C
- Aggiunta vista "Now Playing" con copertina album 240x240, corona di avanzamento circolare verde fluo e stato play/pause.
- Aggiunti comandi I2C per selezione vista (CMD_SCREEN_SET_VIEW, CMD_SCREEN_GET_VIEW, CMD_SCREEN_SET_MEDIA).
- Integrazione con Base Master per sincronizzazione automatica metadati dallo Speaker Bluetooth.
### [0.1.17] - Correzione Specchio Orizzontale (mirror_x = true)
- Abilitato mirror_x per compensare il cablaggio interno delle colonne del pannello GC9A01.
- Testi e grafica perfettamente leggibili da sinistra a destra (non piÃƒÆ’Ã‚Â¹ specchiati).
### [0.1.16] - Correzione Orientamento Display Diritto e Non Specchiato
- Disattivata l'inversione software 180Ãƒâ€šÃ‚Â° che causava l'effetto capovolto/specchiato.
- Il display ora visualizza grafica e scritte perfettamente dritte e orientate normalmente da sinistra a destra.
### [0.1.15] - Rotazione Software LVGL 180Ãƒâ€šÃ‚Â° & Discovery I2C Immediata
- Implementata la rotazione a 180Ãƒâ€šÃ‚Â° tramite software rotation nativa di LVGL (sw_rotate = 1 e rotated = LV_DISP_ROT_180).
- Risolto il mancato riconoscimento del modulo sulla Base: il modulo parte sempre su indirizzo 0x30 e risponde tempestivamente al discovery del Master.
### [0.1.14] - Fix OTA Wi-Fi / HTTPS & Ottimizzazione RAM
- Risolto crash/errore di out-of-memory durante il download OTA Wi-Fi HTTPS liberando 28 KB di RAM interna (task GUI e buffer DMA) prima dell'aggiornamento.
- Abilitati i buffer dinamici MbedTLS per ridurre il consumo di memoria durante l'handshake TLS.
- Aggiunta schermata di stato LCD "MODULO - AGGIORNAMENTO FIRMWARE... Attendere riavvio".
- Mantenuta la rotazione hardware dello schermo a 180Ãƒâ€šÃ‚Â°.
### [0.1.13] - Rotazione Schermo 180 Gradi Hardware
- Ruotato l'orientamento dello schermo di 180Ãƒâ€šÃ‚Â° tramite mirror hardware (X e Y) sul controller GC9A01.
- Schermo orientato correttamente secondo l'alloggiamento fisico sul PCB Modulo.
### [0.1.12] - Fix Retroilluminazione Fissa, Font Montserrat e Sincronizzazione DMA
- Rimosso il blink di test del backlight: ora retroilluminazione fissa e stabile su GPIO 21.
- Abilitati e integrati i font Montserrat in LVGL: scritte perfettamente nitide e leggibili.
- Sincronizzazione corretta del flush LVGL con il completamento delle transazioni SPI DMA (double-buffering a 20 linee).
- Centratura e layout raffinato per il display circolare GC9A01 240x240.
### [0.1.11] - Fix Retroilluminazione Fissa, Font Montserrat e Sincronizzazione DMA
- Rimosso il blink di test del backlight: ora retroilluminazione fissa e stabile su GPIO 21.
- Abilitati e integrati i font Montserrat in LVGL: scritte perfettamente nitide e leggibili.
- Sincronizzazione corretta del flush LVGL con il completamento delle transazioni SPI DMA (double-buffering a 20 linee).
- Centratura e layout raffinato per il display circolare GC9A01 240x240.
### [0.1.10] - Test Backlight Blink e Fix I2C
- Aggiunto test del Backlight (blink ogni 2 secondi) per debugging hardware.
- Silenziato il log I2C di CMD_GET_VBUS_VOLTAGE per stabilizzare la negoziazione Base-Slave.
### [0.1.9] - Fix SPI Clock a 20MHz e Colori LVGL Swap
- Ridotto clock SPI a 20MHz per garantire l'accensione dell'LCD.
- Aggiunto LV_COLOR_16_SWAP per correggere l'endianness dei colori RGB565 sul display GC9A01.
### [0.1.9] - Fix SPI Clock a 20MHz e Colori LVGL Swap
- Ridotto clock SPI a 20MHz per garantire l'accensione dell'LCD.
- Aggiunto LV_COLOR_16_SWAP per correggere l'endianness dei colori RGB565 sul display GC9A01.

### [0.1.6] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Skeletons and build configurations.

### [0.1.5] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Deferred NVS flash writes.

---

## Modulo USB-C Charger

### [0.1.6] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Skeletons and build configurations.

### [0.1.5] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Deferred NVS flash writes.

---

## Modulo Wireless Charger

### [0.1.6] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Skeletons and build configurations.

### [0.1.5] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Deferred NVS flash writes.

---

## Modulo LED Tower

### [0.1.33] - 2026-08-10
- **Fix AffidabilitÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â  OTA**: Eliminazione del task `led_btn_task` (TTP223) e disattivazione del timer PWM LEDC prima dell'avvio Wi-Fi per liberare heap SRAM ed evitare picchi di corrente.

### [0.1.32] - 2026-08-09
- **Fix Dimmer PWM**: Ridotta la frequenza PWM da 5 kHz a 1 kHz con risoluzione a 10 bit (0-1023) per la commutazione lineare dei MOSFET di potenza dall'1% al 100%.

### [0.1.31] - 2026-08-09
- **Bottone Capacitivo TTP223 su GPIO 32**: Abilitato il pulsante capacitivo per il controllo touch on/off e la memoria dell'ultimo livello di luminositÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â  impostato.

### [0.1.29] ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬ÃƒÂ¢Ã¢â€šÂ¬Ã‚Â 2026-07-04
- Implemented LEDC (PWM) controller on GPIO 19 (EXT_LEDOUT) to allow dimmer control (0-100% duty cycle).
- Added I2C commands handler for CMD_SET_STATE (to set dimmer brightness) and CMD_GET_STATUS (to retrieve current brightness).

### [0.1.6] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-15
- Skeletons and build configurations.

### [0.1.5] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented real I2C OTA firmware streaming updates writing directly to dual OTA partitions using `esp_ota_ops`.

### [0.1.4] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Removed `s_module_type` loading from NVS.

### [0.1.3] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Implemented CMD_GET_INFO (0x02) command.

### [0.1.2] ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¢ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â€šÂ¬Ã…Â¡Ãƒâ€šÃ‚Â¬ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬Ãƒâ€šÃ‚Â 2026-06-10
- Deferred NVS flash writes.
