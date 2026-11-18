# ADR-0003: Sharp-Protokollencoder unabhängig vom Transport testen

- Datum: 2026-11-18
- Status: Angenommen; konkrete Modellkodierung offen
- Herkunft: [Quellenübersicht](README.md), jüngster Display-Entwurf

## Kontext

Framebuffer-Format und LCD-Drahtformat sind unterschiedliche Verträge.
Protokollfehler sollen ohne angeschlossenen ESP32 oder Display prüfbar sein.
Es liegt noch kein bestätigtes Sharp-Modell mit Datenblatt vor.

## Entscheidung

Der Sharp-Treiber wird in drei Verantwortlichkeiten gegliedert:

1. `SharpMemoryLcd`: Initialisierung, Zustände, View-Validierung,
   Fehlerweitergabe und Orchestrierung.
2. `SharpProtocolEncoder`: deterministische Umwandlung in modellkonforme
   Kommandos, Zeilenadressen und Pixeldaten; keine ESP-IDF-, GPIO-, FreeRTOS-
   oder Zeitquellen-Abhängigkeit. Notwendiger Protokollzustand wird explizit
   als Eingabe bereitgestellt.
3. ESP-IDF-Transport: physische Transfers, Chip Select und benötigte
   Hardwareoperationen hinter einer kleinen injizierbaren Grenze.

Ausgabepuffer oder eine begrenzte Streaming-Schnittstelle explizit übergeben;
Kapazität und Fehlerfälle müssen prüfbar sein. Die konkrete Encoder-API ist
noch zu entwerfen. Keine unnötigen dynamischen Allokationen oder globalen
Hardwarezustände einführen.

## Alternativen

- Kodierung direkt in SPI-Aufrufen: kurz, aber nur schwer isoliert testbar.
- Gesamten ESP-IDF-Treiber im Host simulieren: mehr Kopplung und Testaufwand
  als eine kleine Transportgrenze.

## Konsequenzen und Nachweise

Vor der modellabhängigen Kodierung müssen Datenblatt und Revision,
Auflösung, Adressformat, Kommandoformat, Polarität, Bitreihenfolge, Trailer,
Chip-Select-Verhalten und Timing feststehen. Diese ADR legt keine Bytes,
Taktrate, GPIOs oder Zeilennummerierung fest.

Host-Tests vergleichen mit unabhängig aus dem bestätigten Datenblatt
abgeleiteten Soll-Bytes: Kommandos, erste/letzte Zeile, Adressen,
Bitreihenfolge, Weiß/Schwarz/Schachbrett und Bufferkapazitätsgrenzen.
Nicht den Encoder selbst zur Erzeugung der erwarteten Daten benutzen.
Treibervertragstests injizieren Transportfehler und prüfen Eingabeschutz,
Initialisierungszustand und Ressourcenbehandlung.

Host-Tests belegen weder elektrische Pegel noch reale SPI-Timings oder VCOM.
Diese Nachweise benötigen einen getrennten Hardware-Test mit protokolliertem
Aufbau und Messergebnis. Solange das Modell fehlt, sind API-Vertragstests und
Testplanung möglich, aber keine Behauptung korrekter Sharp-Protokollbytes.
