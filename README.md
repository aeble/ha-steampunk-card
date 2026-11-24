# Steampunk

![Screenshot](images/steampunk-card.png)

Riveted brass plates on dark wood.

- Nixie tubes as a clock.
- One dial gauge per room, warning zone from 25 °C.
- Boiler house with gauges for boiler and hot water, a fire hatch, gears that turn while the burner runs, and a mechanical counter for gas.
- Toggle switches with light bulbs for the lights.
- Gauges for power, outdoor temperature and network throughput.
- Telegraph tape for departures, who is home, open windows and flat batteries.

## Configuration

All cards share the same keys. Anything missing is hidden or shown as "–".

```yaml
type: custom:steampunk-card
title: Home
language: en                          # any language with a file in i18n/; defaults to the Home Assistant language
outside_temperature: sensor.outdoor_temperature
outside_humidity: sensor.outdoor_humidity
weather: weather.forecast_home        # condition, and fallback for the outdoor temperature
flame: binary_sensor.burner_flame     # heating, optional
boiler: sensor.boiler_temperature
hot_water: sensor.hot_water_temperature
gas_today: sensor.gas_today
gas_cost: sensor.gas_cost_today
power: sensor.house_power             # W
download: sensor.wan_download         # optional, converted to Mbit/s whatever the unit
upload: sensor.wan_upload
battery_threshold: 20                 # battery sensors below this are reported
idle_timeout: 300                     # idle screen after this many seconds without touch (omit = off)
idle_wake:                            # optional, keeps the dashboard awake while active
  - binary_sensor.kitchen_presence
rooms:
  - name: Living room
    temperature: sensor.living_room_temperature
    humidity: sensor.living_room_humidity
    co2: sensor.living_room_co2           # optional, ppm; yellow from 1000, red from 1200
    pm25: sensor.living_room_pm25         # optional, µg/m³; yellow from 15, red from 35
lights:
  - name: Kitchen
    entity: light.kitchen
departure:                            # optional, sensor holding the timestamp of the next departure
  name: Bus 12 › Centre
  entity: sensor.next_departure
  delay: sensor.delay
contacts:                             # optional, windows and doors
  - name: Bathroom window
    entity: binary_sensor.bathroom_window
todo: todo.shopping_list              # optional, a to-do list such as Bring! or the built-in shopping list
```

A short tap on a light toggles it; press and hold opens the Home Assistant light dialog with brightness and colour.

With `todo` the card shows the open items of that list, each in its own style. Tapping it opens the list in Home Assistant for editing.

The idle screen puts a black layer with a faint clock over the dashboard; a tap wakes it without switching anything underneath. A browser cannot dim the tablet's backlight itself. While idle, all animations pause and the card redraws only every 30 seconds, which helps older tablets.

People come from all `person.*` entities, low batteries from all sensors with `device_class: battery`.

The cards speak English and German. Without `language` they follow the Home Assistant language; languages without a translation fall back to English.

## Translations

All texts live in `i18n/<language>.json`, one file per language, keyed by short names such as `boiler` or `departs_in`. Placeholders in braces (`{mins}`, `{name}`) are filled in by the card and may move freely within the sentence. To add a language, copy `i18n/en.json` to e.g. `i18n/fr.json`, translate the values and open a pull request. Missing keys fall back to English.

## Installation

Open HACS, search for `steampunk-card` and download it. HACS adds the dashboard resource itself; reload the browser afterwards.

By hand: copy `steampunk-card.js` to `/config/www/` and add `/local/steampunk-card.js` as a JavaScript module under Settings → Dashboards → Resources.

A discreet signature sits in the bottom right corner; `signature: false` hides it.

The card is meant for a panel view (`panel: true`) so it fills the screen.

```yaml
views:
  - title: Home
    panel: true
    cards:
      - type: custom:steampunk-card
        # configuration as above
```

The fonts are embedded in the card file; the card loads nothing from external servers.

## License

Apache License 2.0, copyright 2026 Axel Eble. If you redistribute or modify the card, include `LICENSE` and `NOTICE`.


---

## Deutsch

### Steampunk

![Screenshot](images/steampunk-card-de.png)

Messingtafeln mit Nieten auf dunklem Holz.

- Nixie-Röhren als Uhr.
- Ein Zeigerinstrument je Raum, Warnbereich ab 25 °C.
- Kesselhaus mit Instrumenten für Kessel und Warmwasser, Feuerluke, Zahnrädern, die sich bei laufendem Brenner drehen, und mechanischem Zählwerk für das Gas.
- Kippschalter mit Glühbirnen für die Lampen.
- Instrumente für Leistung, Außentemperatur und Netzdurchsatz.
- Telegraphenstreifen für Abfahrten, Anwesende, offene Fenster und erschöpfte Batterien.
#### Konfiguration

Alle Karten teilen sich dieselben Schlüssel. Was fehlt, wird ausgeblendet oder als „–“ angezeigt.

```yaml
type: custom:steampunk-card
title: Haus
language: de                          # jede Sprache mit Datei in i18n/; Standard ist die Sprache von Home Assistant
outside_temperature: sensor.aussen_temperatur
outside_humidity: sensor.aussen_feuchte
weather: weather.forecast_home        # Zustand und Ersatz für die Außentemperatur
flame: binary_sensor.brenner_flamme   # Heizung, optional
boiler: sensor.kesseltemperatur
hot_water: sensor.warmwassertemperatur
gas_today: sensor.gas_heute
gas_cost: sensor.gaskosten_heute
power: sensor.hausleistung            # W
download: sensor.wan_download         # optional, Einheit wird nach Mbit/s umgerechnet
upload: sensor.wan_upload
battery_threshold: 20                 # Batteriesensoren darunter werden gemeldet
idle_timeout: 300                     # Ruhemodus nach so vielen Sekunden ohne Berührung (weglassen = aus)
idle_wake:                            # optional, hält das Dashboard wach, solange aktiv
  - binary_sensor.kueche_praesenz
rooms:
  - name: Wohnzimmer
    temperature: sensor.wohnzimmer_temperatur
    humidity: sensor.wohnzimmer_feuchte
    co2: sensor.wohnzimmer_co2            # optional, ppm; gelb ab 1000, rot ab 1200
    pm25: sensor.wohnzimmer_pm25          # optional, µg/m³; gelb ab 15, rot ab 35
lights:
  - name: Küche
    entity: light.kueche
departure:                            # optional, Sensor mit Zeitstempel der nächsten Abfahrt
  name: Linie 3 › Stadt
  entity: sensor.naechste_abfahrt
  delay: sensor.verspaetung
contacts:                             # optional, Fenster und Türen
  - name: Fenster Bad
    entity: binary_sensor.fenster_bad
todo: todo.einkaufsliste              # optional, To-do-Liste, etwa Bring! oder die eingebaute Einkaufsliste
```

Kurzes Tippen auf eine Lampe schaltet sie um, langes Drücken öffnet den Lichtdialog von Home Assistant mit Helligkeit und Farbe.

Mit `todo` zeigt die Karte die offenen Einträge der Liste, jede in ihrem Stil. Antippen öffnet die Liste in Home Assistant zum Bearbeiten.

Der Ruhemodus legt ein schwarzes Bild mit schwacher Uhr über das Dashboard; Antippen weckt es, ohne darunter etwas zu schalten. Die Helligkeit des Tablets selbst lässt sich aus dem Browser nicht steuern. Solange er aktiv ist, stehen alle Animationen still und die Karte zeichnet nur alle 30 Sekunden neu; das entlastet ältere Tablets.

Die Karten sprechen Deutsch und Englisch. Ohne `language` folgen sie der Sprache von Home Assistant; Sprachen ohne Übersetzung erscheinen auf Englisch.

Alle Texte stehen in `i18n/<sprache>.json`, eine Datei je Sprache, mit kurzen Schlüsseln wie `boiler` oder `departs_in`. Platzhalter in geschweiften Klammern (`{mins}`, `{name}`) füllt die Karte; sie dürfen im Satz beliebig stehen. Für eine neue Sprache `i18n/en.json` kopieren, etwa nach `i18n/fr.json`, die Werte übersetzen und einen Pull Request stellen. Fehlende Schlüssel fallen auf Englisch zurück.

Personen kommen automatisch aus allen `person.*`-Entitäten, schwache Batterien aus allen Sensoren mit `device_class: battery`.

#### Einbau

HACS öffnen, nach `steampunk-card` suchen und herunterladen. HACS trägt die Dashboard-Ressource selbst ein; danach den Browser neu laden.

Von Hand: `steampunk-card.js` nach `/config/www/` kopieren und unter Einstellungen → Dashboards → Ressourcen als JavaScript-Modul `/local/steampunk-card.js` eintragen.

Unten rechts steht eine dezente Signatur; `signature: false` blendet sie aus.

Die Karte ist für eine Panel-Ansicht gedacht (`panel: true`), damit sie den ganzen Bildschirm füllt.

```yaml
views:
  - title: Haus
    panel: true
    cards:
      - type: custom:steampunk-card
        # Konfiguration wie oben
```

Die Schriften stecken in der Kartendatei; die Karte lädt nichts von fremden Servern.

#### Lizenz

Apache License 2.0, Copyright 2026 Axel Eble. Wer die Karte weitergibt oder verändert, liefert `LICENSE` und `NOTICE` mit.