# Namron Updater Content Repo

Dette repoet inneholder alt dynamisk innhold for Namron Updater:
- Enhetsmetadata
- Firmware-lister
- Hjelpetekster og bilder
- Firmware-filer

Appen leser `catalog.json` i rot, og bruker stier derfra til per-enhet JSON-filer.

## Struktur

```text
catalog.json
devices/
  <itemCode>/
    device.json
    firmware.json
    help.json
    images/
      device/
      help/
    firmware/
```

Eksempel:

```text
devices/110086/
  device.json
  firmware.json
  help.json
  images/
    device/sensor.png
    help/hdu_1_update.png
    help/hdu_2_update.png
  firmware/
    sensor_tak_110086_v1.5.0.zip
```

## Viktige prinsipper

1. Bruk varekode (`itemCode`) som primær nøkkel overalt.
2. Hold alle filer for en enhet samlet i enhetsmappen.
3. Ikke overskriv gamle firmware-filer. Legg inn nye versjoner som nye filer.
4. Oppdater alltid `catalog.json` når en ny enhet legges til.

## Hvordan legge til en ny enhet

1. Opprett mappe: `devices/<itemCode>/`
2. Legg inn `device.json`, `firmware.json`, `help.json`
3. Legg inn enhetsbilder under `images/device/`
4. Legg inn hjelpebilder under `images/help/`
5. Legg inn firmware-filer under `firmware/`
6. Legg til enheten i `catalog.json`

## Filkrav

### catalog.json
Må inneholde:
- `schemaVersion`
- `updatedAt`
- `contentVersion`
- `devices[]` med:
  - `itemCode`
  - `name`
  - `shortDescription`
  - `chipset`
  - `status`
  - `paths.device`
  - `paths.firmware`
  - `paths.help`

### device.json
Må beskrive:
- Enhetsidentitet (`itemCode`, navn, beskrivelse)
- `chipset`
- BLE-navnprefix
- Modell/variant-mapping
- Bildepaths

### firmware.json
Må beskrive:
- Firmware-releases
- `version`
- `channel` (`stable`/`beta`)
- `variant`
- `models`
- `file`
- `changes[]`

### help.json
Må beskrive:
- Hjelpeseksjoner per enhet
- `title`
- `image`
- `text[]` (stegvis tekst)

## PR-sjekkliste

Før merge:

1. Alle JSON-filer er gyldige.
2. Alle paths i JSON peker til eksisterende filer.
3. Ny firmware-fil finnes fysisk i riktig mappe.
4. Enheten er lagt til i `catalog.json`.
5. Endringslogg (`changes`) er oppdatert.

## Publisering

- Main branch brukes som aktiv innholdskilde.
- Appen henter `catalog.json` fra GitHub raw URL.
- Endringer går via PR for kvalitetssikring.
