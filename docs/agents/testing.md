# Rolle: Tests und Validierung

Vorhandenes Framework und dokumentierte Befehle zuerst prüfen. Dieses Repository
enthält ein ESP-IDF-CMake-Grundgerüst; Firmware-Host-Tests sind noch nicht
vorhanden. Die im separaten Dotfiles-Repository vorhandenen Tests prüfen ein
anderes Subsystem und sind kein Display- oder Firmwaretest. Kein Framework allein für
Dokumentationsänderungen einführen und keine erfolgreichen Tests erfinden.

## Host-Ebene

- Graphics: Pixel über Byte-/Zeilengrenzen, Rand- und negative Koordinaten
  soweit von der API zugelassen, Clipping, Nachbarpixel, Padding, Clear,
  transparente/deckende Bitmaps und Glyphen anhand des vereinbarten Verhaltens.
- Views: Nullpointer, Null-Dimensionen, falscher Stride, zu kleine/falsche Größe,
  Größenüberlauf und Abweichung von der Displaygröße. Den zulässigen View-Vertrag
  aus ADR-0001 anwenden und keine andere Stride-Semantik im Test erfinden.
- Display: Aufruf vor Initialisierung, erfolgreiche Ausgabe, Fehlerweitergabe,
  unveränderter Quellbuffer und Lebensdauer. Noch offene Semantik zuerst klären.
- Encoder: Soll-Bytes unabhängig aus dem bestätigten Datenblatt ableiten;
  Kommandos, Adressen, erste/letzte Zeile, Bitreihenfolge, Polarität, Weiß,
  Schwarz, Schachbrett und Ausgabekapazität prüfen. Modell fehlt: diese
  Protokolltests planen, aber keine Soll-Bytes raten.
- Transport-Fakes: gezielte Fehler und Teilinitialisierung nachbilden.
  Ein FakeDisplay kopiert Daten innerhalb von `present()`, sofern es sie später
  untersuchen soll. Fakes ersetzen keinen elektrischen Hardwaretest.

## Target- und Hardware-Ebene

Build für bestätigte ESP-IDF-Version/Target von tatsächlichen Hardwaretests
trennen. Smoke-Test: Initialisierung, bekannte Testbilder, Orientierung,
erste/letzte Zeile und Wiederanlauf. VCOM mit unverändertem Bild und jedem
geforderten Sleep-Modus messen, unabhängig von der UI-Aktualisierung.

Messprotokolle enthalten Board-/Display-/Schaltungsrevision, Firmwarestand,
Versorgung, Messmittel, Bedingungen, Erwartung, Beobachtung und offene Grenzen.

## Bericht

Ausgeführte Befehle und Resultate, geplante Tests und nicht ausgeführte
Hardwareprüfungen getrennt nennen. Umfang proportional zur Änderung wählen.
Bei reinen Dokumentationsänderungen lokale Links, Statusangaben, Quellenbezug
und Widerspruchsfreiheit prüfen; keine Firmware-Testpflicht daraus ableiten.
