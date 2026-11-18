# Implementierungsauftrag: Sharp Memory LCD Driver

Status: vorbereiteter Arbeitsauftrag; kein bereits implementierter Treiber.
Die konkrete Hardwareintegration bleibt bis zur Klärung der unten genannten
Modell- und Setup-Fragen offen. Aktuell beauftragt ist die Dokumentationsgrundlage.

## Ziel und Umfang

Einen austauschbaren C++-Displaytreiber auf Basis der [ADRs](../adr/README.md)
entwickeln. Gemeinsame Display-Verträge, Sharp-Treiber, Protokollencoder und
kleine ESP-IDF-Transportgrenze gehören zum Umfang eines späteren Auftrags.
Graphics und UI entwickelt der Nutzer separat; sie nicht mitimplementieren.

Die [Projektanweisungen](../../AGENTS.md) und für den jeweiligen Arbeitsschritt
die Rollen Architektur, ESP-IDF und Tests beachten. Die tatsächliche
Repository-Struktur vor jeder Implementierung neu prüfen.

## Vorbedingungen und Arbeitsaufteilung

Hardwareunabhängig möglich sind Review und Konkretisierung der API,
View-Validierung, FakeDisplay und Vertragstests. Die API in ADR-0002 ist ein
Entwurf; noch nicht vorhandene Header nicht als freigegeben bezeichnen.

Vor modellabhängigem Encoder und Transport müssen belegt sein:

- exakte Modellnummer, Datenblatt/Revision, Auflösung, Panel/Breakout;
- Schaltung, Pins, Versorgung, SPI-/Chip-Select- und Timing-Anforderungen;
- VCOM-Modus und gewählte Lösung mit Nachweis für Idle und geforderte Sleep-Modi;
- tatsächliche ESP-IDF-Version, Target, C++-Standard, Build und Testverfahren.

Fehlende Angaben dokumentieren und nur die davon abhängige Arbeit zurückstellen.
Kein Beispielmodell, Protokollbyte oder GPIO als Projektwert übernehmen.

## Anforderungen

- `IDisplay` übernimmt einen vollständigen konstanten 1-Bit-Framebuffer gemäß
  ADR-0001. Synchronität und Lebensdauer auch auf Fehlerpfaden einhalten.
- Initialisierung, Eingabevalidierung und typisierte Fehler nachvollziehbar
  implementieren. Zustands- und Recovery-Verhalten vorher festlegen.
- Encoder ohne ESP-IDF entwickeln; deterministische Ausgaben und explizite
  Pufferkapazitäten. Kodierung mit Datenblattstellen begründen.
- ESP-IDF-Transport injizierbar halten, Ressourcen-Ownership und Fehler bei
  Teilinitialisierung definieren. Keine unbegründeten Thread-/ISR-Zusagen.
- VCOM unabhängig von `present()` lösen und die Entscheidung in einem Folge-ADR
  zu ADR-0004 mit Hardware- und Sleep-Bezug festhalten.
- Keine eigenständige Änderung etablierter Display-Verträge. Notwendige
  Abweichungen als Architekturentscheidung mit Auswirkung konkret vorlegen.

## Akzeptanz und Lieferung

1. Host-Vertragstests für gültige/ungültige Views, Zustand, Fehlerweitergabe,
   unveränderte Eingaben und Buffer-Lebensdauer bestehen.
2. Nach Modellklärung prüfen Encoder-Tests unabhängige Soll-Bytes für Kommandos,
   Zeilenadressen, Randzeilen, Bitreihenfolge und definierte Testbilder.
3. Build mit bestätigter ESP-IDF-Version und Zielkonfiguration nachweisen.
4. Hardware-Smoke-Test und VCOM-/Sleep-Messplan bereitstellen; tatsächliche
   Ausführung und Beobachtungen gesondert protokollieren. Ohne Messung keine
   Hardwarefreigabe behaupten.
5. Änderungen und Pfade, Ressourcen-/Aufrufkonzept, Quellen, ausgeführte
   Testbefehle, Ergebnisse und offene Hardwarefragen liefern.

Kleine, überprüfbare Schritte wählen. Ein späterer ausdrücklicher
Implementierungsauftrag autorisiert die vereinbarte Arbeit; dieser vorbereitete
Text fordert keine wiederholte Freigabe für bereits beauftragte Routinearbeiten.
