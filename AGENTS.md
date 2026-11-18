# Smartscale – Projektanweisungen

## Kontext und Arbeitsweise

Smartscale ist ein Embedded-C++-Lern- und Entwicklungsprojekt. Ziel ist auch
die Entwicklung des Nutzers zum Senior Embedded Software Engineer.
ESP32-S3 und ESP-IDF sind im bisherigen Gespräch als Zielkontext genannt;
prüfe vor Implementierung die tatsächlichen Projektdateien. Dieses Verzeichnis
ist der Root des Firmware-Git-Repositories: `main/`, `components/`, `.devcontainer/`
und `docs/` liegen direkt darin. Persönliche Dotfiles liegen in einem separaten
Repository außerhalb dieses Checkouts. Der aktuelle Bestand und der beobachtete
Umgebungskontext sind in [docs/development-setup.md](docs/development-setup.md)
festgehalten. Alle hier referenzierten Arbeitsanweisungen liegen im Repository;
keine übergeordneten Projektdateien zum Lesen der Architektur voraussetzen.

Untersuche für jede Aufgabe die relevanten vorhandenen Dateien und Änderungen.
Respektiere bestehende Arbeit. Ein expliziter aktueller Nutzerauftrag bestimmt
den Umfang; für bereits beauftragte Dokumentation, Tests und reversible
Routinearbeiten ist keine zusätzliche Planfreigabe nötig.

## Display-Verträge

Bei Display-Arbeit die einschlägigen [ADRs](docs/adr/README.md) lesen:

- Anwendung besitzt den vollständigen 1-Bit-Framebuffer; zeilenweise,
  MSB-first im Speicher, 0 = Weiß, 1 = Schwarz, Ursprung oben links.
- Zeilenlänge ist `ceil(width / 8)`; ungenutzte Bits sind 0.
- Graphics zeichnet Pixel, Text und Bitmaps; `IDisplay` überträgt synchron
  einen vollständigen Framebuffer und übernimmt weder Speicher noch Rendering.
- Graphics und Sharp-Treiber hängen nur von gemeinsamen Display-Verträgen ab,
  nicht voneinander. Die Anwendung verdrahtet die Implementierungen.
- Sharp-Protokollencoder bleibt ohne ESP-IDF auf dem Host testbar. Eine kleine
  lokale Transportgrenze reicht; keine pauschale SPI-Abstraktionshierarchie.
- VCOM muss unabhängig von UI-Updates betrachtet werden. Modell, Pins,
  Protokollbytes, SPI-Timing und Sleep-Verhalten nicht raten.

Die C++-Signaturen in ADR-0002 sind ein Entwurf, keine bereits vorhandenen
oder freigegebenen Header. Einen etablierten Vertrag nur mit expliziter
Architekturentscheidung ändern; Widersprüche benennen und betroffene Arbeiten
abgrenzen. Keine Auflösung oder Modellnummer aus einem Beispiel übernehmen.

## Lernbereich und Rollen

Der Nutzer implementiert die Graphics Library selbst. In diesem Bereich
standardmäßig erklären, kleine Übungen stellen und vorhandenen Code reviewen.
Keine vollständige Lernlösung oder automatische Überarbeitung liefern, außer
der aktuelle Nutzerauftrag verlangt sie ausdrücklich. Delegierter Treibercode
darf innerhalb des beauftragten Umfangs implementiert werden.

Je nach Aufgabe die passende Anweisungsdatei lesen; mehrere sind kombinierbar:

| Aufgabe | Anweisungen |
| --- | --- |
| Architektur und Schnittstellen | [architecture.md](docs/agents/architecture.md) |
| ESP-IDF, Transport, Ressourcen, Sleep | [esp-idf.md](docs/agents/esp-idf.md) |
| C++-Lernaufgaben und Graphics | [cpp-mentor.md](docs/agents/cpp-mentor.md) |
| Testentwurf und Validierung | [testing.md](docs/agents/testing.md) |
| Design- und Code-Review | [review.md](docs/agents/review.md) |

Diese Rollen sind Arbeitsanweisungen, keine Aufforderung, weitere Agenten
zu starten. Es gibt keine hiermit eingerichtete automatische Delegation.

## Implementierung und Nachweise

- Ownership, Lebensdauer, Fehlerverhalten und Aufrufkontext explizit machen.
  RAII für Ressourcen nutzen, soweit mit vorhandener Umgebung vereinbar.
- C++-Standard, ESP-IDF-Version, Exceptions/RTTI und Allokationsregeln aus dem
  Bestand prüfen. Konfigurationshinweise von tatsächlich geprüfter Toolchain
  und erfolgreichen Build-Befehlen unterscheiden.
- Hardwareabhängige Entscheidungen anhand des konkreten Datenblatts,
  Schaltungsstands und passender offizieller ESP-IDF-Dokumentation belegen.
  Fehlende Angaben dokumentieren und unabhängige Aufgaben fortsetzen.
- Relevante bestehende Tests ausführen; neue Verhaltenstests bei funktionalen
  Änderungen ergänzen. Für reine Dokumentationsänderungen Links, Konsistenz
  und Vollständigkeit prüfen. Keine Firmware-Tests vortäuschen.
- Tatsächlich ausgeführte Befehle, Ergebnisse und nicht ausgeführte Prüfungen
  klar unterscheiden. Abschluss: Änderungen, Pfade und verbleibende Grenzen.

## Code Review Rules

Priorisiere konkrete Fehler mit Auslöser, Auswirkung und Datei/Zeile.
Prüfe insbesondere Buffergrenzen, Lebensdauer bei synchronem `present()`,
Formatkonvertierung, Fehlerpfade, Ressourcenfreigabe und unabhängige VCOM-
Versorgung während unverändertem Bild und geplanten Sleep-Phasen.
Ungeklärte Hardwarefragen sind offene Fragen, keine bewiesenen Codefehler.
