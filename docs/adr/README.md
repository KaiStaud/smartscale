# Architecture Decision Records

Dokumentiert am 2026-11-18 auf Grundlage des aktuellen Auftrags und des
gelesenen Gesprächs [Smartscale mit ChatGPT entwickeln](chatgpt-conversation://6af9ef30-3bd4-83ed-a7c6-4480ae820e4b).
Maßgeblich ist der jüngste Architekturentwurf mit vollständigem 1-Bit-
Framebuffer, festen Bitmap-Fonts und selbst implementierter Graphics Library.
Ältere Vorschläge im Gespräch gelten nicht automatisch als Entscheidung.

## Entscheidungsstand

| ADR | Thema | Status |
| --- | --- | --- |
| [0001](0001-application-owned-framebuffer.md) | Anwendungseigener 1-Bit-Framebuffer | Angenommen als Architekturgrundlage |
| [0002](0002-display-and-graphics-boundaries.md) | Graphics, IDisplay und Sharp-Treiber | Angenommen; C++-API noch Entwurf |
| [0003](0003-testable-sharp-protocol.md) | Testbarer Protokollencoder und Transportgrenze | Angenommen; Modellkodierung offen |
| [0004](0004-vcom-and-sleep.md) | VCOM und Sleep | Offen; Anforderungen dokumentiert |

„Angenommen“ bezeichnet die durch den aktuellen Auftrag zu dokumentierende
Softwarearchitektur. Es bedeutet weder implementiert noch auf Hardware
validiert. Vorschläge für Signaturen und zukünftige Verzeichnisse sind als
solche gekennzeichnet. Für VCOM ist ausdrücklich keine Lösung beschlossen.

## Quellen und Grenzen

1. Aktueller Nutzerauftrag: ADRs und Agenten-Anweisungen im Projekt anlegen;
   unbekannte Hardwaredetails nicht erfinden.
2. Referenziertes Gespräch: Display-Vertrag, Schichten, Lernaufteilung,
   API-Vorschlag und offene VCOM-/Sleep-Fragen. Das Gespräch ist eine Quelle
   für Designabsichten, kein Hardware-Datenblatt oder Repository-Nachweis.
3. Lokale Bestandsaufnahme im Firmware-Git-Repository
   `/work/Projects/vscode/smartscale/retrofit`: ESP-IDF-Grundgerüst im Root und
   eigene Devcontainer-Konfiguration. Ein zusätzlicher Devcontainer im
   übergeordneten Ordner und das separate Dotfiles-Repository wurden als
   Umgebungskontext beobachtet. Details und Quellen stehen in
   [development-setup.md](../development-setup.md).

ESP32-S3 wird durch generierte Build-Konfiguration gestützt; ESP-IDF 6.1.1
ist in Editorpfad und generierter Konfiguration sichtbar, aber nicht durch
gepinntes Container-Image reproduzierbar festgelegt. Die Dotfiles-README
beschreibt das VS-Code-Profil Smartscale und die Einbindung. Die tatsächlich
aktiven persönlichen Profileinstellungen wurden nicht geprüft. Vorhandene
Konfiguration ist ein Befund, keine nachträglich erfundene Architekturentscheidung.

## Pflege

Neue wesentliche Entscheidungen als fortlaufend nummeriertes ADR mit Datum,
Status, Kontext, Entscheidung, Alternativen, Konsequenzen und Nachweisen
erfassen. Ersetzte Entscheidungen mit Verweis auf das Nachfolge-ADR markieren.
Offene Fragen erst mit belegter Entscheidung schließen; historische Gründe
erhalten. Kleinere Dokumentationskorrekturen brauchen kein neues ADR.
