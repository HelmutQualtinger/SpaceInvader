# Space Invaders

Browser-basiertes Space Invaders im Arcade-Stil — originalgetreu mit Pixelgrafiken, klassischem HUD und CRT-Effekt.

## Spielen

```
open index.html
```

Oder über einen lokalen Server:

```
python3 -m http.server 8080
# → http://localhost:8080
```

## Steuerung

| Taste | Aktion |
|-------|--------|
| `←` / `→` oder `A` / `D` | Bewegen |
| `Leertaste` / `Z` / `↑` | Schießen (nur 1 Schuss gleichzeitig) |
| `C` | Münze einwerfen (Credit +1) |

## Features

- **Pixelgenaue Sprites** — Cannon, UFO und Aliens als klassische 8×8-Pixelkunst
- **Authentisches HUD** — `SCORE<1> / HI-SCORE / SCORE<2>`, Lebensanzeige als Mini-Kanonen, Credit-Zähler
- **3 Alientypen** — Tintenfisch (30 Pkt.), Krabbe (20 Pkt.), Oktopus (10 Pkt.)
- **UFO** — erscheint zufällig, 50–300 Punkte
- **4 Schutzwälle** — erodieren blockweise in 3 Grünstufen
- **Klassische Explosions-Sprites** — kein Partikeleffekt, sondern das originale Sternmuster
- **Flüssige Animation** — kontinuierliche Bewegung jeden Frame; Marschrhythmus (Sound + Sprite-Flip) entkoppelt
- **CRT-Overlay** — Scanlines + Vignette
- **Authentische Sounds** — Square-Wave-Schüsse, Rausch-Explosionen, Marschrhythmus, UFO-LFO

## Dateistruktur

```
index.html   — Hauptspiel (komplettes Spiel in einer Datei)
game.html    — ältere Version mit modernem Neon-Stil
```

## Technik

Reines HTML5-Canvas + Web Audio API, keine Abhängigkeiten.
