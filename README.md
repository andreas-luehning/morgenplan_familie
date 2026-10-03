# Unser Morgen

Checkliste für zwei Kinder als installierbare Web-App (PWA). Sie funktioniert ohne Internet und ohne Konto, alle Daten bleiben im Browser des Geräts.

## Funktionen

- **🌞 Unser Morgen:** Feste Schritte (Aufstehen bis Schuhe & Jacke) mit eigener Liste und Fortschritt je Kind. Am Wochenende entfallen Tasche und Schuhe.
- **📋 Heute noch:** Zweite Seite für das tägliche To-do am Nachmittag, z. B. Hausaufgaben, Leseübung, Hörspiel. Erreichbar per Wischen oder über die Punkte unten.
- **Abfahrt:** Unter der Woche bleibt das Display bis 5 Minuten nach der Abfahrt an. Zur Abfahrtszeit klingelt ein Wecker mit Hinweis „Jetzt geht's los!“. Am Wochenende kommt stattdessen eine leise Erinnerung, falls noch etwas offen ist.
- **Besondere Tage** (unten auf der Seite): Punkte, die nur an bestimmten Tagen auftauchen. Sie können regelmäßig sein (cron-Regel, z. B. `* * 2` = dienstags, `* * 1#1` = 1. Montag im Monat) oder einmalig (an einem Datum bzw. nur morgen). Hinzufügen und Löschen nur per langem Drücken, damit Kinder nichts versehentlich ändern.
- **Zusätzlicher Punkt:** Knopf unter den Karten für schnelle Punkte für heute oder morgen, z. B. am Vorabend „Geschenk mitnehmen“.
- **Liste testen für Tag:** Vorschau der Checkliste für ein beliebiges Datum.
- **Version** steht klein unten auf der Seite.

## Nutzung

1. Die Seite über https öffnen (GitHub Pages, siehe unten).
2. Zum Homescreen hinzufügen. Auf iPhone/iPad danach nur noch die Homescreen-App nutzen, denn Safari und Homescreen-App speichern getrennt.
3. Namen, Abfahrtszeit und Weg eintragen. Einmal auf den Bildschirm tippen, damit der Wecker-Ton freigeschaltet ist.

Die Daten (Namen, Zeiten, Regeln, Häkchen) liegen in `localStorage` und bleiben bei Updates erhalten. Wer die Website-Daten im Browser löscht, verliert sie.

## Deployment

GitHub Pages: Settings → Pages → Branch wählen, Ordner `/`. Die App läuft dann unter
`https://andreas-luehning.github.io/morgenplan_familie/`.

Zum Testen eines Feature-Branches den Branch in Pages umstellen. Die Adresse und damit die gespeicherten Daten bleiben gleich.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | Die ganze App: HTML, CSS und JS inline |
| `sw.js` | Service Worker für Offline-Betrieb, enthält `VERSION` |
| `manifest.webmanifest`, `icons/` | PWA-Installation |
| `styleguide/` | Design-System „Playful UI“; das CSS ist in `index.html` eingebettet |
| `BACKLOG.md` | Ideen, Prioritäten und Änderungsprotokoll |
| `morgenregeln_*.md` | Die Morgenregeln der Familie für Eltern und Kinder (Inhaltsvorlage) |
