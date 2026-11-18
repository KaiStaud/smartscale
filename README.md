# Smartscale – Firmware

ESP-IDF-Firmware und Architekturunterlagen im Git-Repository
`/work/Projects/vscode/smartscale/retrofit`. Stand: 2026-11-18.

## Einstieg

- [Vollständiges Inhaltsverzeichnis](TABLE_OF_CONTENTS.md)
- [Agenten-Anweisungen](AGENTS.md)
- [ADRs und Herkunft der Entscheidungen](docs/adr/README.md)
- [Entwicklungsumgebung und belegbarer Bestand](docs/development-setup.md)
- [Spezialisierte Arbeitsrollen](docs/agents/README.md)
- [Implementierungsauftrag für den Sharp-Treiber](docs/tasks/sharp-memory-lcd.md)

## Display-Architektur

Die Anwendung besitzt einen vollständigen 1-Bit-Framebuffer. Eine selbst
entwickelte Graphics Library zeichnet hinein; `IDisplay` nimmt das fertige
Bild entgegen. Der Sharp-Treiber übernimmt Ausgabe und gerätespezifische
Verantwortlichkeiten. Modell, Beschaltung und VCOM-Lösung sind offen.
Die konkreten C++-Signaturen sind noch ein Entwurf.

## Repository-Struktur

- [CMakeLists.txt](CMakeLists.txt): ESP-IDF-Projekt `smartscale`.
- [main/main.c](main/main.c): aktuell leeres `app_main()` in C.
- `components/`: Platz für benötigte Komponenten; noch keine Implementierung.
- [.devcontainer/](.devcontainer/devcontainer.json): Firmware-Devcontainer.
- [.vscode/](.vscode/settings.json): vorhandene Editor-Einstellungen.
- [CMakePresets.json](CMakePresets.json): Entwicklungs- und Produktionspreset.
- `docs/`: ADRs, Setup-Nachweise, Rollen und Implementierungsauftrag.

Alle Projektanweisungen, ADRs und dieses Inhaltsverzeichnis liegen innerhalb
dieses Git-Repositories. Das persönliche Dotfiles-Repository und der
übergeordnete VS-Code-Workspace sind separater Umgebungskontext; das Setup-
Dokument hält die beobachteten Angaben und offenen Fragen fest. Sie sind
keine Voraussetzung, um die hier enthaltenen Architekturunterlagen zu lesen.
Firmwarebuild und Hardwarefunktion wurden in diesem Dokumentationsschritt
nicht geprüft.
