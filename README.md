# 🎸 HELION GUITARS — DOUBLE CUT S1
### Electric Guitar Emulator · PWA

> Un emulatore di chitarra elettrica a corpo solido con motore audio **Karplus-Strong**, tre modalità di suono, accordi, arpeggi e installabile come app offline.

---

## 📦 Struttura del progetto

```
helion-s1/
├── index.html            ← Applicazione principale (single-file)
├── manifest.json         ← PWA Web App Manifest
├── sw.js                 ← Service Worker (cache offline)
├── icon.svg              ← Icona vettoriale sorgente
├── icon-192.png          ← Icona Android / Chrome (192×192)
├── icon-512.png          ← Icona splash / maskable (512×512)
└── apple-touch-icon.png  ← Icona iOS "Aggiungi a schermata" (180×180)
```

---

## 🚀 Deploy (GitHub Pages)

1. Copia la cartella `helion-s1/` nel tuo repository
2. Vai su **Settings → Pages → Branch: main / root**
3. L'app sarà disponibile su `https://tuousername.github.io/helion-s1/`
4. Il Service Worker richiede **HTTPS** — GitHub Pages è compatibile nativamente

---

## 📲 Installazione come PWA

| Piattaforma | Procedura |
|-------------|-----------|
| **iOS Safari** | Condividi → "Aggiungi a schermata Home" |
| **Android Chrome** | Menu ⋮ → "Installa app" (o banner automatico) |
| **Desktop Chrome / Edge** | Icona ⊕ nella barra degli indirizzi |

Una volta installata, l'app funziona **completamente offline** grazie al Service Worker.

---

## 🎵 Motore Audio

Il suono è generato in tempo reale tramite l'algoritmo **Karplus-Strong** implementato in JavaScript puro via **Web Audio API**:

1. Un buffer di rumore bianco viene riempito con campioni casuali di lunghezza `N = sampleRate / frequenza`
2. Un filtro passa-basso con damping iterativo simula la perdita di energia della corda
3. Il parametro **brightness** varia in base alla posizione del pickup (0.95 Bridge → 0.15 Neck)
4. Il segnale attraversa la catena: `Pickup EQ → Distortion (WaveShaper) → Tone 1 → Tone 2 → Delay → Reverb → Master`
5. Il riverbero è un **impulse response** sintetico generato a runtime (rumore esponenzialmente decadente)

### Accordatura corde (standard)

| Corda | Nota | Frequenza |
|-------|------|-----------|
| 1 (alta) | E4 | 329.63 Hz |
| 2 | B3 | 246.94 Hz |
| 3 | G3 | 196.00 Hz |
| 4 | D3 | 146.83 Hz |
| 5 | A2 | 110.00 Hz |
| 6 (bassa) | E2 | 82.41 Hz |

---

## 🎮 Come si suona

### Modalità SINGLE (click diretto)

Clicca su qualsiasi intersezione **corda × tasto** del manico per suonare la nota corrispondente.  
La frequenza viene calcolata automaticamente: `f = fOpen × 2^(tasto/12)`

**Scorciatoie tastiera:**

| Tasto | Effetto |
|-------|---------|
| `1` – `6` | Corde da 1 a 6 (nota aperta) |
| `Q` `W` `E` `R` `T` `Y` | Corde 1–6 al tasto 1 |
| `A` `S` `D` `F` `G` `H` | Corde 1–6 al tasto 2 |
| `P` | Cicla la posizione del pickup |
| `SPAZIO` | Strum accordo selezionato (in Chord Mode) |
| `INVIO` | Play/Stop arpeggio (in Arpeggio Mode) |

---

### Modalità CHORD (accordi)

1. Seleziona il tab **CHORD** nel pannello centrale
2. Clicca un accordo dalla griglia (Em, Am, Dm, G, C, F, A, E, D, Bm, A7, E7, G7, Cadd9)
3. Scegli la direzione dello strum: **↓ DOWN** · **↑ UP** · **↕ AUTO** (down + up rimbalzato)
4. Regola la velocità di strumming con il cursore **SPEED** (5–60 ms tra una corda e l'altra)
5. Il **diagramma di diteggiatura** si aggiorna automaticamente ad ogni accordo selezionato:
   - `●` = tasto da premere
   - `○` = corda aperta
   - `✕` = corda silenziata

**Accordi disponibili:**

| Accordo | Diteggiatura (corda 6→1) |
|---------|--------------------------|
| Em | 0-2-2-0-0-0 |
| Am | x-0-2-2-1-0 |
| Dm | x-x-0-2-3-1 |
| G  | 3-2-0-0-0-3 |
| C  | x-3-2-0-1-0 |
| F  | 1-3-3-2-1-1 (barrè) |
| A  | x-0-2-2-2-0 |
| E  | 0-2-2-1-0-0 |
| D  | x-x-0-2-3-2 |
| Bm | x-2-4-4-3-2 (barrè) |
| A7 | x-0-2-0-2-0 |
| E7 | 0-2-2-1-3-0 |
| G7 | 3-2-0-0-0-1 |
| Cadd9 | x-3-2-0-3-3 |

---

### Modalità ARPEGGIO

1. Seleziona il tab **ARPEGGIO**
2. Scegli un **accordo** dalla griglia
3. Scegli il **pattern**:

| Pattern | Descrizione |
|---------|-------------|
| ↑ ASC | Dalla corda bassa alla corda alta, in sequenza |
| ↓ DESC | Dalla corda alta alla corda bassa |
| ◇ BROKEN | Alternanza basso/treble (picking armonico) |
| ♩ TRAVIS | Travis picking: basso alternato + melodia interna |

4. Imposta il **BPM** (40–220 BPM, unità = 8th note / croma)
5. Premi **▶ PLAY** o il tasto `INVIO` per avviare — **■ STOP** per fermare
6. La corda attiva viene evidenziata visivamente ad ogni passo

---

## 🎛️ Controlli

### Pickup selector (5-way switch)

| Posizione | Suono |
|-----------|-------|
| 1 — BRIDGE | Brillante, twangy, molto presente |
| 2 — BRIDGE+MID | Quack caratteristico, in fase |
| 3 — MIDDLE | Neutro, bilanciato |
| 4 — MID+NECK | Morbido con presenza mid |
| 5 — NECK | Caldo, bassi rotundi, jazz/blues |

### Knob rotanti (drag verticale)

- **VOLUME** — guadagno master output (0–100)
- **TONE 1** — filtro passa-basso principale (300 Hz – 15 kHz)
- **TONE 2** — filtro passa-basso secondario (500 Hz – 14 kHz)

### Effetti (slider)

- **DISTORTION** — WaveShaper con saturazione progressiva (0–100%)
- **REVERB** — riverbero sintetico con impulse response (0–100%)
- **DELAY** — eco con feedback (0–100%, delay time max 500ms)

### Preset

| Preset | Pickup | Carattere |
|--------|--------|-----------|
| CLEAN | Middle | Suono cristallino, poca distorsione |
| CRUNCH | Bridge | Blues/rock, distorsione moderata |
| LEAD | Neck | Solista, distorsione alta, riverbero pieno |

---

## 🖥️ Compatibilità

| Browser | Supporto |
|---------|----------|
| Chrome 80+ | ✅ Pieno |
| Firefox 75+ | ✅ Pieno |
| Safari 14.1+ (iOS) | ✅ Pieno |
| Edge 80+ | ✅ Pieno |
| Samsung Internet 12+ | ✅ Pieno |

> **Nota:** l'audio richiede un gesto utente (click/tap) prima dell'inizializzazione, per rispettare le policy dei browser moderni sull'`AudioContext`.

---

## 🔧 Sviluppo locale

```bash
# Serve la cartella su localhost (richiesto per SW e PWA)
cd helion-s1
npx serve .          # oppure
python3 -m http.server 8080
```

Apri `http://localhost:8080` nel browser. Per testare l'installazione PWA usa Chrome DevTools → Application → Manifest.

---

## 📁 Tecnologie utilizzate

- **Web Audio API** — sintesi e catena effetti
- **Karplus-Strong Algorithm** — modello fisico corde pizzicate
- **SVG** — chitarra vettoriale animata
- **Service Worker + Cache API** — funzionamento offline
- **Web App Manifest** — installazione PWA
- **Canvas API** — visualizzatore waveform in tempo reale

---

## 📜 Licenza e Copyright

```
Copyright © 2026 PezzaliApp Instruments
Tutti i diritti riservati.

Autore: Alessandro Pezzali
Brand:  PezzaliApp — pezzaliapp.com

Questo software è di proprietà esclusiva di PezzaliApp Instruments.
È vietata la riproduzione, distribuzione o modifica senza autorizzazione
scritta dell'autore.

Il nome "HELION GUITARS — DOUBLE CUT S1" è un marchio fittizio creato
esclusivamente per questa applicazione. Non ha alcuna affiliazione con
produttori di strumenti musicali reali.

Il motore audio Karplus-Strong è implementato in linguaggio JavaScript
originale da PezzaliApp Instruments e non deriva da librerie di terze parti.

"HELION GUITARS — DOUBLE CUT S1" is a fictional brand created solely
for this application. It has no affiliation with any real instrument
manufacturer.
```

---

*PezzaliApp Instruments · pezzaliapp.com · 2026*
