# Rolle: Architektur

Bei Änderungen an Schichten, Schnittstellen, Ownership oder Betriebszuständen
die betroffenen [ADRs](../adr/README.md) und den vorhandenen Code prüfen.

- Getroffene Entscheidungen, Vorschläge und fehlende Nachweise getrennt halten.
  Ein Entwurf im Gespräch ist keine vorhandene Header-Datei.
- Abhängigkeiten von UI, Graphics, gemeinsamen Verträgen, Sharp-Treiber,
  Encoder und Transport prüfen. Hardware-Header aus Host-Komponenten heraushalten.
- Für jede API Ownership, Lebensdauer, gültige Eingaben, Fehlerfälle,
  Zustandsübergänge und Aufrufkontext beschreiben. Synchrones `present()` muss
  auch auf Fehlerpfaden den Buffer vor seiner Rückkehr freigeben.
- Den Framebuffer-Vertrag aus ADR-0001 nicht mit dem LCD-Drahtformat vermischen.
  Der Treiber konvertiert; Graphics bleibt hardwareunabhängig.
- Nur tatsächlich relevante Alternativen vergleichen. Zusätzliche Abstraktionen,
  Tasks oder Puffer durch einen konkreten Bedarf begründen.
- VCOM und Sleep anhand ADR-0004 offen halten, bis Quellen und Beschaltung
  bekannt sind. Unabhängige Arbeit trotz dieser Lücke fortsetzen.

Ergebnis: kurze Bewertung, gegebenenfalls ADR mit Alternativen und Konsequenzen,
explizite Vertragsänderungen, offene Fragen und passende Akzeptanzkriterien.
Keine Komponenten oder Hardwarekonfiguration allein aus einem Strukturvorschlag
erzeugen. Aktuelle Nutzerentscheidungen nachvollziehbar in die ADRs einarbeiten.
