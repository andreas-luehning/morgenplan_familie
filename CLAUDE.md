# CLAUDE.md

Familien-Checkliste als PWA. Überblick für Nutzer: `README.md`. Stand, Prioritäten und Änderungsprotokoll: `BACKLOG.md`.

## Architektur

- Alles steckt in `index.html` (CSS und JS inline, kein Build, keine Abhängigkeiten). Die Seite muss auch als einzelne lokale Datei laufen, also keine externen Skripte, Stylesheets oder Fonts.
- Das Styleguide-CSS (`styleguide/playful-ui.css`) ist eingebettet. Farben nur über `var(--pf-…)`-Tokens.
- Zustand in `localStorage` unter `morgenplan-zwei-kinder-v1`. Diesen Key nicht ändern, sonst verlieren Geräte ihre Daten. Neue Felder in `load()` validieren und mit Standardwerten auffüllen.
- Tageslogik über einen kleinen cron-Parser (`parseCron`, Felder `Tag Monat Wochentag`, plus `#n` und `L`). Regeln in `state.rules` haben entweder `cron` oder `date` und optional `list:'heute'` für die zweite Seite.

## Konventionen

- Bei jeder Änderung an gecachten Dateien `VERSION` in `sw.js` hochzählen.
- Jede Änderung oben im Änderungsprotokoll in `BACKLOG.md` eintragen (Version, Branch, Änderung). Erledigte Punkte mit ✅ markieren.
- Pro Feature ein eigener Branch `feature/<n>-<name>`, aufbauend auf dem vorherigen, und gleich pushen. Der Nutzer deployt ihn zum Testen über GitHub Pages. Den PR nach `main` erstellt er später selbst.
- UI-Texte auf Deutsch, kindgerecht. Kein Wettbewerb zwischen den Kindern.

## Prüfen

Es gibt keine Testsuite. Mindestens die Syntax des Inline-Skripts prüfen:

```sh
python3 -c "import re;print(re.findall(r'<script>(.*?)</script>',open('index.html').read(),re.S)[0])" > /tmp/app.js && node --check /tmp/app.js
```

Wake Lock, Ton und Service Worker lassen sich nur im echten Browser über https prüfen.
