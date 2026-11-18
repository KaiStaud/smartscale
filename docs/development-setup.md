# Entwicklungsumgebung und belegbarer Repository-Stand

Bestandsaufnahme: 2026-11-18. Repository-Root:
`/work/Projects/vscode/smartscale/retrofit`.
Alle ADRs, Rollen, Agenten-Anweisungen und das Inhaltsverzeichnis liegen hier
innerhalb des Firmware-Git-Repositories. Pfade in diesem Dokument sind relativ
zum Repository-Root, sofern nicht ausdrücklich als externer Kontext bezeichnet.

## Repository-Struktur

| Bereich | Beobachteter Bestand |
| --- | --- |
| [CMakeLists.txt](../CMakeLists.txt) | ESP-IDF-CMake-Projekt `smartscale` |
| [main/main.c](../main/main.c) | Leeres `app_main()` in C |
| `components/`, `doc/`, `tools/`, `LICENSES/` | Bei Bestandsaufnahme ohne Dateien; neue Unterlagen liegen unter `docs/` |
| [.devcontainer/](../.devcontainer/devcontainer.json) | Firmware-Devcontainer und Dockerfile |
| [.vscode/](../.vscode/settings.json) | Vorhandene Editor-Konfiguration |
| [CMakePresets.json](../CMakePresets.json) | Entwicklungs- und Produktionspreset |
| `docs/`, `AGENTS.md`, `README.md`, `TABLE_OF_CONTENTS.md` | Architektur- und Arbeitsunterlagen dieses Repositories |

Die zuvor angelegte `AGENTS.md` war nur ein Verweis auf den übergeordneten
Ordner. Sie wird durch die vollständigen Repository-Anweisungen ersetzt.
Firmware-, Container-, Workspace- und Dotfiles-Konfiguration werden durch die
Dokumentationskorrektur nicht verändert.

## ESP-IDF, CMake und Target

- [CMakeLists.txt](../CMakeLists.txt) bindet
  `$IDF_PATH/tools/cmake/project.cmake` ein und benennt das Projekt `smartscale`.
  [main/CMakeLists.txt](../main/CMakeLists.txt) registriert `main.c`.
  Ein C++-Standard ist dort noch nicht festgelegt.
- [CMakePresets.json](../CMakePresets.json) definiert `default` und
  `production`, jeweils mit eigenem Build-Verzeichnis und `SDKCONFIG`-Pfad.
- [Editor-Einstellungen](../.vscode/settings.json) nennen
  `/home/vscode/.espressif/v6.1.1/esp-idf` und
  `board/esp32s3-builtin.cfg`. Das sind Konfigurationshinweise, kein Nachweis
  einer aktuell aktiven oder funktionierenden Installation.
- Vorhandene generierte Dateien `build/project_description.json` und
  `build/config/sdkconfig.json` nennen `esp32s3`; letztere nennt
  `IDF_INIT_VERSION` gleich `6.1.1`. Generierte Artefakte sind Momentaufnahmen,
  keine dauerhafte Versionsbindung und kein in diesem Chat ausgeführter Build.
- Firmware-Host-Tests oder ein Displaytreiber sind noch nicht vorhanden.
  Das exakte Board, das Sharp-Modell und die Beschaltung bleiben ungeklärt.

## Devcontainer im Repository

- [devcontainer.json](../.devcontainer/devcontainer.json): Name ESP-IDF QEMU,
  Build über lokales Dockerfile, Bash, ESP-IDF- und ESP-IDF-Web-Erweiterungen,
  `--privileged`.
- [Dockerfile](../.devcontainer/Dockerfile): `ARG DOCKER_TAG=latest`, Basis
  `espressif/idf:${DOCKER_TAG}`, udev, IDF-Export in Bash und Espressif-Entrypoint.

Der Standardwert `latest` bindet keine feste IDF-Version. Der Name „QEMU“
allein belegt keine funktionsfähige QEMU-Testumgebung. Ein tatsächlicher
Containerstart wurde nicht beobachtet.

## Externer Workspace-Kontext – separat vom Repository

Bei der Bestandsaufnahme enthielt der übergeordnete Ordner
`/work/Projects/vscode/smartscale` weitere Dateien und Unterprojekte.
Die folgenden Angaben sind dokumentierte Beobachtungen; diese externen
Dateien gehören nicht zum Firmware-Checkout und werden hier nicht verändert.

| Externe Quelle | Beobachteter Inhalt |
| --- | --- |
| `../smartscale.code-workspace` | Multi-Root-Workspace mit `retrofit`, `mobile-app` und `espressif-dotfiles` |
| `../.devcontainer/devcontainer.json` | Name ESP-IDF, `image: espressif/idf:latest` plus `build.dockerfile`, Mount nach `/workspaces/smartscale`, Extension-Cache, Bash, ESP-IDF-Erweiterung, `--privileged` |
| `../.devcontainer/Dockerfile` | Basis `mcr.microsoft.com/devcontainers/cpp:3-debian13`, optionale CMake-Neuinstallation; keine sichtbare ESP-IDF-Installation |
| `../espressif-dotfiles/` | Eigenes Git-Repository mit Installer, Bash-/Git-Konfiguration und Tests |
| `../mobile-app/` | Zum Prüfzeitpunkt keine lesbaren Projektdateien |

Die externe Containerkonfiguration nennt Image und Build zusammen, während
ihr Dockerfile von einer anderen Basis ausgeht. Welche Konfiguration genutzt
wird und wie die Abweichung aufgelöst werden soll, bleibt offen. Diese
Beobachtung ist keine Aufforderung, externe Konfiguration automatisch zu ändern.

## Dotfiles und VS-Code-Profil

Die beim Prüfen gelesene externe Datei
`../espressif-dotfiles/README.md` beschreibt das Profil **Smartscale** mit
Dev Containers und folgenden persönlichen Profileinstellungen:

```json
{
  "dotfiles.repository": "KaiStaud/espressif-dotfiles",
  "dotfiles.targetPath": "~/dotfiles",
  "dotfiles.installCommand": "install.sh"
}
```

Die aktiven VS-Code-Benutzereinstellungen oder ein Profil-Export wurden nicht
gelesen. Ob die Werte im aktuellen Profil bereits gesetzt sind, bleibt offen.
Laut dieser README erfolgt die Einbindung über Dev Containers beim Einrichten
des Containers. Lokale uncommittete Dotfiles-Änderungen werden dadurch nicht
automatisch übertragen.

Der dort gelesene `install.sh` ergänzt einen markierten Bash-Block und fehlende
globale Git-Defaults. Er ist persönliche Konfiguration und installiert keine
ESP-IDF-Toolchain. Er wurde hier nicht ausgeführt. Die externe README nennt
`python3 tests/test_install.py` als Dotfiles-Testbefehl. Dieser ist kein
Firmwaretest und wurde für diese Dokumentationsänderung nicht ausgeführt.

## Noch zu klären und anschließend zu prüfen

1. Tatsächlich genutzten Devcontainer auswählen; externe und Repository-
   Konfiguration vor Änderungen bewusst abgleichen.
2. Gewünschte ESP-IDF-Version und Imagebindung festlegen; aktive Umgebung,
   Target, C++-Standard sowie Build-/Test-Befehle verifizieren.
3. Reales VS-Code-Profil und Dotfiles-Einbindung prüfen, ohne persönliche
   Einstellungen oder lokale Änderungen des separaten Repositories zu ersetzen.
4. Build-Ausgabe und Editorpfade abgleichen: Presets nutzen `build/default`
   beziehungsweise `build/production`, während clangd auf `retrofit/build` zeigt.
5. Displaymodell, Datenblatt, Schaltung und VCOM-/Sleep-Anforderungen gemäß
   [ADR-0004](adr/0004-vcom-and-sleep.md) klären.
6. Containerstart, Firmwarebuild sowie später Host- und Hardwaretests getrennt
   nachweisen. Vorhandene Build-Artefakte reichen dafür nicht.

## Validierungsstand

Die Korrektur betrifft ausschließlich Markdown-Unterlagen und deren Ablage.
Lokale Links werden auf Ziele innerhalb dieses Repositories geprüft. Die
bisherigen Dokumentkopien im übergeordneten Ordner werden nach erfolgreicher
Prüfung entfernt. Firmwarebuild, Container-Rebuild, Dotfiles-Installation und
Hardwaretests wurden in diesem Arbeitsschritt nicht ausgeführt. Es wurde
kein Git-Repository initialisiert und kein Commit erstellt.
