# Playful UI — Style Guide

Die wiederverwendbare Design-Sprache aus dem *Genius Square Trainer*. Farben aus
der „Playful"-Buchpalette, plus die über die Iterationen entstandenen Tokens und
Komponenten (haptische 3D-Buttons, Pillen-Menü, Elfenbein-Kacheln, Holzdübel-Brett).

## Dateien
- `playful-ui.css` — Tokens (Farben, Radien, Schatten, Schrift) + Komponentenklassen. **Das ist die Bibliothek.**
- `styleguide.html` — visuelle Übersicht/Doku. Im Browser öffnen zum Durchschauen und Theme-Testen.
- `README.md` — diese Datei.

## Einbinden
```html
<link rel="stylesheet" href="playful-ui.css">
<html data-theme="standard">   <!-- standard | koralle | mint -->
```

## Prinzipien
- **Tokens statt fester Farben.** Immer `var(--pf-…)` nutzen, nie Hex direkt. Ein
  Theme-Wechsel färbt dann die ganze App um.
- **Rollen, nicht Einzelfarben.** Ein Theme belegt dieselben Variablen:
  `--pf-bg`, `--pf-board`, `--pf-btn1`, `--pf-btn2`, `--pf-accent`,
  `--pf-title-a/-b`, `--pf-eyebrow`, `--pf-label`.
- **Materialien sind themenunabhängig:** Elfenbein-Kacheln (`--pf-ivory-*`) und
  Holzdübel (`--pf-wood-*`) bleiben in allen Themen gleich.
- **Haptik:** Buttons und Brett haben eine feste Unterkante (`box-shadow: 0 5px 0 …`)
  und sinken beim Tippen ein — für ein „drückbares", kindgerechtes Gefühl.
- **Offline-fest:** nur System-Schriften, keine externen Ressourcen.
- **Barrierearm:** `prefers-reduced-motion` wird respektiert; Aktionsschrift ist kräftig (800).

## Komponentenklassen (Auszug)
`pf-btn` (`--primary`/`--secondary`/`--danger`), `pf-card`, `pf-pill-nav` + `pf-seg` +
`pf-pill-ind`, `pf-chip`, `pf-toggle`, `pf-readout`, `pf-tile`, `pf-board`, `pf-socket`
(`.is-filled` = Holzdübel).

## Neues Theme anlegen
Einen Block mit denselben Variablen ergänzen:
```css
[data-theme="name"]{
  --pf-bg:…; --pf-board:…; --pf-board-edge:…; --pf-label:…;
  --pf-btn1:…; --pf-btn1-ink:…; --pf-btn1-edge:…;
  --pf-btn2:…; --pf-btn2-ink:…; --pf-btn2-edge:…;
  --pf-accent:…; --pf-eyebrow:…; --pf-title-a:…; --pf-title-b:…;
}
```

## Offen
- **Theme „Original"** (Originalfarben des Spiels) ist als Platzhalter vorgesehen —
  Farben aus der offiziellen PDF-Anleitung ziehen und als eigenen `[data-theme="original"]`-Block eintragen.
