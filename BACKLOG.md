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

**Umsetzung:** Regeln im cron-Format `Tag Monat Wochentag`, einstellbar in der App ganz unten unter „📅 Besondere Tage einstellen“. Erweiterungen: `1#1` = 1. Montag, `4L` = letzter Donnerstag, `L` im Tag = Monatsletzter. Voreingestellt: Sportbeutel dienstags für Kind 1 und freitags für Kind 2, Spielzeugtag am 1. Montag im Monat für Kind 2. Noch offen: „alle n Wochen“ (in cron nicht ausdrückbar) und Ferien-Ausnahmen.

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

### 6. ✅ Besondere Tage nur per langem Tippen ändern

**Ziel:** Kinder können Regeln nicht versehentlich löschen oder anlegen.

- „🗑️ Löschen“ und „➕ Hinzufügen“ reagieren nur auf langes Drücken (umgesetzt: 1,5 Sekunden), mit sichtbarem Fortschritt im Knopf, z. B. einem Füllbalken.
- Kurzes Tippen zeigt nur den Hinweis „Zum Löschen lange drücken“.
- Auch per Tastatur bedienbar lassen, z. B. Enter gedrückt halten oder eine zusätzliche Rückfrage.
- Alternative: Schon das Aufklappen von „📅 Besondere Tage einstellen“ braucht langes Drücken.
- Passt zur Eltern-Sperre aus Punkt 4; dieselbe Lösung später für die editierbare Liste nutzen.

### 7. ✅ Wochenend-Profil und Frisur

**Ziel:** Am Wochenende passt die Liste zum freien Tag, und jeden Tag gibt es einen Punkt für die Haare.

- „🎒 Tasche ist bereit“ und „👟 Schuhe & Jacke an“ gelten nur Mo–Fr. Technisch bekommen feste Schritte optional eine cron-Regel (`* * 1-5`), wie die besonderen Tage.
- Neuer fester Schritt jeden Tag: „🎀 Haare gebürstet & Frisur gemacht“ (z. B. Zopf).
- Mit „Liste testen für Tag“ lässt sich das Wochenende vorab ansehen.
- Am Wochenende kein Wecker und kein Dauer-Display. Das Zeitfeld heißt dann „Am Wochenende erinnern um“ (eigene Zeit, Standard 10:00), die Auswahl „Heute geht's …“ ist ausgeblendet.
- Ist zur Erinnerungszeit noch etwas offen, kommt einmal ein kurzer, leiser Hinweiston und das Fenster „🌞 Schon alles fertig?“ mit den offenen Punkten je Kind. Ist schon alles erledigt, kommt nichts.
- Wie der Wecker funktioniert die Erinnerung nur, wenn die App offen und im Vordergrund ist.

### 9. ✅ Zusätzliche Punkte für einen Tag

**Ziel:** Einmalige Punkte, z. B. Ausflug, Fotograf oder Laternenfest, schnell eintragen, ohne dass sie jedes Jahr wiederkommen.

- **Wann? mit drei Möglichkeiten** bei „Besondere Tage“: Regelmäßig (cron), An einem Datum, Nur morgen. Einmalige Punkte haben ein Datum statt einer cron-Regel und werden nach dem Tag automatisch gelöscht. In der Liste stehen sie mit „Mi., 14.10. · einmalig“.
- **Schnell-Hinzufügen:** Knopf „➕ Zusätzlicher Punkt“ unter den Karten. Er öffnet per langem Drücken ein Fenster mit Bild, Text, Kind und heute/morgen; „heute“ ist vorgewählt.
- **Vorlagen** zum Antippen, in beiden Formularen: 🏊 Schwimmsachen, 🖍️ Bastelsachen, 🎁 Geschenk, ☂️ Regensachen. Brotdose und Trinkflasche sind selbstverständlich, Sportbeutel und Spielzeug laufen über regelmäßige Regeln.
- Kontrolle über „Liste testen für Tag“.

### 10. ✅ Zweite Seite „Heute noch“ (tägliches To-do)

**Ziel:** Nachmittags-Aufgaben abhaken, ohne die App kompliziert zu machen.

- Zwei Seiten, „🌞 Unser Morgen“ und „📋 Heute noch“. Umschalten per Wischen über die Karten oder über die Seitenpunkte unten am Bildschirmrand. Die Überschrift wechselt mit der Seite.
- Keine neue Logik: To-do-Punkte sind besondere Tage mit „Seite: Heute noch“. Damit regelt sich auch das Wochenende über die cron-Regel.
- Voreingestellt für beide Kinder: 📚 Hausaufgaben, 📖 Leseübung, ➗ Mathe üben (Mo–Fr), 🎧 Hörspiel hören (jeden Tag). Geräte mit gespeicherten Regeln bekommen diese einmalig dazu.
- Auf „Heute noch“ sind die Einstellungen (Abfahrt, Weg) und der Spiel-Tipp ausgeblendet. Ist die Liste leer, steht dort „Heute steht hier nichts an. 🎉“.
- Schnell-Hinzufügen: Die Seite ist wählbar, vorgewählt ist die aktuelle. Ab 17 Uhr ist „morgen“ vorgewählt (z. B. Geschenk oder Regensachen am Vorabend).
- Idee für später: Der Abend-Modus (Punkt 5) könnte eine dritte Seite werden.

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

### 8. Farb-Makeover

**Ziel:** Alle Knöpfe und Flächen sind gut zu erkennen.

- Orange Knöpfe (`pf-btn--secondary`, z. B. „↺ Häkchen löschen“, „🗑️ Löschen“, „↩ Zurück zu heute“) sind teils schlecht zu sehen. Gemessen: Die Schrift auf Orange ist gut lesbar (Kontrast 5,2 : 1), aber der Knopf hebt sich kaum vom Hintergrund ab (1,9 : 1 auf Creme, 2,1 : 1 auf Weiß; empfohlen sind mindestens 3 : 1).
- Ideen: kräftigeres Orange oder dunklerer Rand, Umriss-Stil für zweitrangige Knöpfe, und die Farben insgesamt mit den Lieblingsfarben der Kinder (Blau/Rot, Lila) abstimmen.
- Die Farben kommen aus `styleguide/playful-ui.css` und sind in `index.html` eingebettet. Änderungen am besten im Styleguide machen und neu einbetten.
- Danach in hellem Licht und auf dem echten Gerät prüfen.

---

## Notiert, ohne Priorität

- **Countdown bis zur Abfahrt:** Groß „noch 12 Min“ anzeigen, mit Farbwechsel grün → gelb → rot. Passt zu Punkt 1.
- **Zwischenzeiten:** z. B. „Anziehen bis 7:20“, „Frühstück bis 7:35“, mit leiser Erinnerung, wenn etwas noch offen ist.
- **Belohnung:** Animation oder Stern, wenn ein Kind vor der Abfahrt fertig ist, dazu eine Wochenübersicht.
- **Vorlesen der Punkte:** Eher unwichtig, weil die Kinder die Punkte an den Icons erkennen.
- **Kartenhöhe bei unterschiedlich langen Listen:** Im Alltag beobachten. Bisher sind beide Karten gleich hoch, die kürzere hat unten Leerraum. Idee: das Statusfeld („… ist startklar!“) in beiden Karten an den unteren Rand setzen. Alternative: Karten passen sich der Liste an. Betrifft nur die Ansicht nebeneinander, nicht das Handy.
- **Sync zwischen Geräten:** Nice to have. Aktuell wird nur ein Gerät genutzt.

---

## Änderungsprotokoll

Neueste Version oben. Die Version steht in `sw.js` und unten in der App.

| Version | Branch | Änderung |
|---|---|---|
| v13 | `feature/6-heute-noch` | Pillen-Menü oben entfernt (wirkte überladen). Stattdessen kleine Seitenpunkte unten am Bildschirmrand, Wischen bleibt. Überschrift und Untertitel wechseln: „🌞 Unser Morgen“ und „📋 Heute noch“. Auswahl „Seite“ heißt jetzt „Unser Morgen“ statt „Morgen“ (klang nach dem nächsten Tag). |
| v12 | `feature/6-heute-noch` | Backlog 10: Zweite Seite „📋 Heute noch“ mit Pillen-Menü und Wischen. To-do-Punkte sind besondere Tage mit „Seite: Heute noch“; voreingestellt Hausaufgaben, Leseübung, Mathe üben (Mo–Fr) und Hörspiel (täglich). Schnell-Hinzufügen ab 17 Uhr wieder mit „morgen“ vorgewählt. |
| v11 | `feature/5-zusatzpunkte` | Vorlagen gekürzt auf Schwimmsachen, Bastelsachen, Geschenk, Regensachen (statt Regenjacke). Knopf heißt „➕ Zusätzlicher Punkt“, „heute“ ist immer vorgewählt. |
| v10 | `feature/5-zusatzpunkte` | Backlog 9: Einmalige Punkte (nur morgen oder an einem Datum), die danach automatisch verschwinden. Schnell-Hinzufügen „➕ Punkt für morgen“ unter den Karten (langes Drücken, Fenster mit heute/morgen). Vorlagen zum Antippen. Außerdem behoben: Ausgeblendete Felder wurden teils trotzdem angezeigt, z. B. „Heute geht's …“ am Wochenende. |
| v9 | `feature/4-wochenende` | Am Wochenende kein Wecker und kein Dauer-Display. Stattdessen eine Erinnerung zu einer eigenen Uhrzeit (Standard 10:00) mit kurzem Ton und Liste der offenen Punkte, nur wenn noch etwas offen ist. „Heute geht's …“ ist am Wochenende ausgeblendet. |
| v8 | `feature/4-wochenende` | Backlog 6 und 7: „Hinzufügen“ und „Löschen“ bei den besonderen Tagen nur per langem Drücken (1,5 s, Füllbalken im Knopf; per Tastatur mit Rückfrage). Am Wochenende entfallen „Tasche“ und „Schuhe & Jacke“; neuer Schritt „🎀 Haare gebürstet & Frisur gemacht“ jeden Tag. Die heutigen Häkchen werden beim Update einmalig zurückgesetzt. |
| v7 | `feature/3-tagesplan` | „Besondere Tage“ als Karte mit Hintergrund, damit sie lesbar ist. Neu: „Liste testen für Tag“ zeigt die Checkliste für ein beliebiges Datum als Vorschau, ohne Häkchen zu speichern. Darüber steht eine Hinweisleiste mit „Zurück zu heute“. |
| v6 | `feature/3-tagesplan` | Einstellung umbenannt in „📅 Besondere Tage einstellen“ (statt „Extra-Punkte“) und zugeklappt ans Seitenende verschoben, weil sie selten gebraucht wird. |
| v5 | `feature/3-tagesplan` | Spielzeugtag (🧸 „Spielzeug mitnehmen“) am 1. Montag im Monat für Kind 2 voreingestellt. Greift nur auf Geräten ohne gespeicherte Regeln. |
| v4 | `feature/3-tagesplan` | Backlog 3: Punkte an bestimmten Tagen per cron-Regel (`Tag Monat Wochentag`, mit `1#1`, `4L`, `L`), in der App anlegen und löschen, mit Vorschau der nächsten Termine. Voreingestellt: Sportbeutel Di für Kind 1, Fr für Kind 2. |
| v3 | `feature/2-version` | Backlog 2: Versionsanzeige unten, gelesen aus `sw.js`; Hinweis zum Neuladen bei neuer Version. |
| v2 | `feature/1-wecker` | Backlog 1: Display bleibt bis 5 Min. nach der Abfahrt an; zur Abfahrt Wecker-Ton (1 Min.) und bildschirmfüllender Hinweis. |
| v1 | `main` | Als PWA aufgesetzt (Manifest, Service Worker, Icons). |
