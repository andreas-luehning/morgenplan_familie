# Backlog

## Aktuell wichtig

### 1. ✅ Display bis zur Abfahrt anlassen, Wecker zur Abfahrtszeit

**Ziel:** Morgens geht das Display nicht aus und zur Abfahrt hört und sieht man, dass es losgeht.

- Das Display bleibt an (Screen Wake Lock API), solange die App offen ist und die Abfahrtszeit (Feld `#departure`) noch nicht erreicht ist.
- Bei Erreichen der Abfahrtszeit:
  - ertönt ein Wecker-Ton (Web Audio oder eine Audiodatei, die vorher durch eine Nutzer-Interaktion freigeschaltet wurde),
  - zeigt die App gut sichtbar und bildschirmfüllend an, dass man jetzt losmuss.
- 5 Minuten nach der Abfahrtszeit wird der Wake Lock freigegeben, und das Display darf wieder ausgehen.

**Hinweise:**
- Der Wake Lock geht verloren, wenn der Tab in den Hintergrund wechselt. Bei `visibilitychange` muss er neu angefordert werden.
- Browser spielen Ton nur nach einer Nutzer-Interaktion ab. Deshalb den Audio-Kontext beim ersten Antippen entsperren, z. B. beim ersten Abhaken.
- Wenn sich die Abfahrtszeit ändert, die Timer neu setzen.
- Nach jeder Änderung `VERSION` in `sw.js` hochzählen.

### 2. ✅ Versionskennung in der Fußzeile

**Ziel:** Man sieht auf einen Blick, welche Version gerade geladen ist.

- Klein und unauffällig unten auf der Seite anzeigen, z. B. „v3“.
- Die Version soll nur an einer Stelle gepflegt werden. Aktuell steht sie als `VERSION` in `sw.js`. Damit sie dort nicht doppelt gepflegt wird, entweder per `postMessage` vom Service Worker abfragen oder aus dem Cache-Namen (`caches.keys()`) ableiten.
- Optional: Wenn eine neue Version bereitliegt, einen Hinweis „Update verfügbar – tippen zum Neuladen“ anzeigen.

### 3. ✅ Tagesabhängige Punkte mit Wiederholungsmustern

**Umsetzung:** Regeln im cron-Format `Tag Monat Wochentag`, einstellbar in der App unter „📅 Extra-Punkte an bestimmten Tagen“. Erweiterungen: `1#1` = 1. Montag, `4L` = letzter Donnerstag, `L` im Tag = Monatsletzter. Voreingestellt: Sportbeutel dienstags für Kind 1 und freitags für Kind 2, Spielzeugtag am 1. Montag im Monat für Kind 2. Noch offen: „alle n Wochen“ (in cron nicht ausdrückbar) und Ferien-Ausnahmen.

**Ziel:** Punkte erscheinen nur an den Tagen in der Checkliste, an denen sie gebraucht werden, z. B. „🧸 Spielzeugtag“ oder „⚽ Sportbeutel“.

- Einem Punkt lassen sich eine oder mehrere Regeln zuordnen:
  - **wöchentlich** an bestimmten Wochentagen (z. B. jeden Di und Do),
  - **n-ter Wochentag im Monat** (z. B. jeden 1. Montag → Spielzeugtag),
  - **letzter Wochentag im Monat** (z. B. jeden letzten Donnerstag),
  - **alle n Wochen** ab einem Startdatum (z. B. alle 2 Wochen Mi),
  - **einzelne Termine** (z. B. 14.11. Ausflug).
- Regeln gelten pro Kind oder für alle Kinder.
- Optional sind Ausnahmen wie Ferien oder Feiertage, an denen ein Punkt entfällt.
- Die Checkliste des Tages ergibt sich aus den festen Punkten und den Punkten, deren Regel heute zutrifft.
- Hilfreich wäre eine Vorschau: „Was steht morgen an?“. Das passt gut zum Abend-Modus (Punkt 5).
- Da die editierbare Liste (Punkt 4) später kommt, stehen die Regeln zunächst fest im Code. Das Datenformat so wählen, dass Punkt 4 es später bearbeiten kann.

## Wenn Zeit und Lust ist

### 4. Liste editierbar machen

**Ziel:** Kinder und Checklisten-Punkte ohne Codeänderung anpassen.

- Kinder hinzufügen, umbenennen und entfernen.
- Checklisten-Punkte hinzufügen, ändern, löschen und umsortieren, pro Kind oder für alle.
- Speicherung in `localStorage`, getrennt vom täglichen Abhak-Status. Ein Reset am Morgen darf die Konfiguration nicht löschen.
- Bearbeiten-Modus hinter einem Schalter oder einer einfachen Eltern-Sperre, damit die Kinder nicht versehentlich etwas ändern.

### 5. Abend-Modus

**Ziel:** Ein eigener Modus fürs Abendprogramm, mit eigener Checkliste, z. B. Kleidung rauslegen, Ranzen packen, Zähne putzen, Schlafanzug.

- Umschalten zwischen Morgen und Abend, entweder per Schalter oder automatisch nach Uhrzeit.
- Eigene Punkte und eigener Abhak-Status, unabhängig vom Morgen.
- Er kann die Punkte von morgen aus Punkt 3 anzeigen, z. B. „Morgen ist Sport, Sportbeutel packen“.

---

## Notiert, ohne Priorität

- **Countdown bis zur Abfahrt:** Groß „noch 12 Min“ anzeigen, mit Farbwechsel grün → gelb → rot. Passt zu Punkt 1.
- **Zwischenzeiten:** z. B. „Anziehen bis 7:20“, „Frühstück bis 7:35“, mit leiser Erinnerung, wenn etwas noch offen ist.
- **Belohnung:** Animation oder Stern, wenn ein Kind vor der Abfahrt fertig ist, dazu eine Wochenübersicht.
- **Vorlesen der Punkte:** Eher unwichtig, weil die Kinder die Punkte an den Icons erkennen.
- **Sync zwischen Geräten:** Nice to have. Aktuell wird nur ein Gerät genutzt.
