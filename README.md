# Steampunk

![Screenshot](images/steampunk-card.png)

Messingtafeln mit Nieten auf dunklem Holz.

- Nixie-Röhren als Uhr.
- Ein Zeigerinstrument je Raum, Warnbereich ab 25 °C.
- Kesselhaus mit Instrumenten für Kessel und Warmwasser, Feuerluke, Zahnrädern, die sich bei laufendem Brenner drehen, und mechanischem Zählwerk für das Gas.
- Kippschalter mit Glühbirnen für die Lampen.
- Instrumente für Leistung, Außentemperatur und Netzdurchsatz.
- Telegraphenstreifen für Abfahrten, Anwesende, offene Fenster und erschöpfte Batterien.

## Konfiguration

Alle Karten teilen sich dieselben Schlüssel. Was fehlt, wird ausgeblendet oder als „–“ angezeigt.

```yaml
type: custom:steampunk-card
title: Haus
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
```

Der Ruhemodus legt ein schwarzes Bild mit schwacher Uhr über das Dashboard; Antippen weckt es, ohne darunter etwas zu schalten. Die Helligkeit des Tablets selbst lässt sich aus dem Browser nicht steuern. Solange er aktiv ist, stehen alle Animationen still und die Karte zeichnet nur alle 30 Sekunden neu; das entlastet ältere Tablets.

Personen kommen automatisch aus allen `person.*`-Entitäten, schwache Batterien aus allen Sensoren mit `device_class: battery`.

## Einbau

Über HACS als benutzerdefiniertes Repository (Typ Dashboard) oder von Hand: `steampunk-card.js` nach `/config/www/` kopieren und unter Einstellungen → Dashboards → Ressourcen als JavaScript-Modul `/local/steampunk-card.js` eintragen.

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

Schriften kommen von Google Fonts. Ohne Internetzugang fällt die Karte auf Systemschriften zurück.

## Lizenz

Apache License 2.0, Copyright 2026 Axel Eble. Wer die Karte weitergibt oder verändert, liefert `LICENSE` und `NOTICE` mit.
