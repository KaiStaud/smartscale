# Table of Contents – Smartscale-Firmware

Verlinktes Inhaltsverzeichnis der Unterlagen und relevanten Einstiegsdateien
innerhalb dieses Git-Repositories. Stand: 2026-11-18.

## Projekt und Entwicklungsumgebung

- [Projektübersicht](README.md)
- [Entwicklungsumgebung, Bestand und offene Setup-Fragen](docs/development-setup.md)
- [ESP-IDF-CMake-Projekt](CMakeLists.txt)
- [Firmware-Einstieg app_main](main/main.c)
- [Main-Komponentenregistrierung](main/CMakeLists.txt)
- [CMake-Presets](CMakePresets.json)
- [Devcontainer-Konfiguration](.devcontainer/devcontainer.json)
- [Devcontainer-Dockerfile](.devcontainer/Dockerfile)
- [Editor-Einstellungen](.vscode/settings.json)

## Architecture Decision Records

- [Entscheidungsübersicht, Status und Quellen](docs/adr/README.md)
- [ADR-0001: Anwendungseigener vollständiger 1-Bit-Framebuffer](docs/adr/0001-application-owned-framebuffer.md)
- [ADR-0002: Graphics, IDisplay und Sharp-Treiber](docs/adr/0002-display-and-graphics-boundaries.md)
- [ADR-0003: Testbarer Sharp-Protokollencoder](docs/adr/0003-testable-sharp-protocol.md)
- [ADR-0004: VCOM und Sleep – offene Entscheidung](docs/adr/0004-vcom-and-sleep.md)

## Agenten-Anweisungen

- [Repository-weite AGENTS.md](AGENTS.md)
- [Rollenübersicht und Verwendung](docs/agents/README.md)
- [Architektur](docs/agents/architecture.md)
- [ESP-IDF und Hardwareintegration](docs/agents/esp-idf.md)
- [C++-Mentoring](docs/agents/cpp-mentor.md)
- [Tests und Validierung](docs/agents/testing.md)
- [Architektur- und Code-Reviews](docs/agents/review.md)

## Implementierungsaufträge

- [Sharp Memory LCD Driver: Umfang, Vorbedingungen und Akzeptanzkriterien](docs/tasks/sharp-memory-lcd.md)

## Grenzen

Das Displaymodell und die VCOM-Strategie sind offen. Die C++-Signaturen sind
noch ein Entwurf. Vorhandene Konfiguration und Build-Artefakte belegen keinen
in diesem Chat erfolgreich ausgeführten Build oder Hardwaretest. Separater
Workspace- und Dotfiles-Kontext ist im Setup-Dokument festgehalten, ohne
lokale Links auf Dateien außerhalb dieses Repositories vorauszusetzen.
