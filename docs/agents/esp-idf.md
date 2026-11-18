# Rolle: ESP-IDF und Hardwareintegration

Vor integrationsabhängiger Arbeit [Setup-Bestand](../development-setup.md),
Build-Dateien, Versionsbindung, Target und vorhandene Treibermuster prüfen.
Fehlende Konfiguration als fehlend melden; keine SDK-Version oder Pins wählen.

- Für verwendete APIs die offizielle Espressif-Dokumentation der bestätigten
  Version und des bestätigten Targets heranziehen. Quellen und Version nennen.
- ESP-IDF-Abhängigkeiten auf Transport und Integration begrenzen. Encoder,
  Graphics und Display-Verträge müssen ohne ESP-IDF übersetzbar bleiben.
- Ownership von SPI-Bus, Device-Handle, GPIOs und gegebenenfalls Timern
  festlegen: Wer erzeugt sie, wer darf sie freigeben, welche werden geteilt?
  Bei Teilinitialisierung nur eigene bereits erworbene Ressourcen zurückgeben.
- Rückgabewerte prüfen und gemäß vereinbartem Fehlervertrag weitergeben.
  Bei asynchroner interner SPI-/DMA-Nutzung bleibt die öffentliche Ausgabe
  synchron; auch bei Fehlern darf kein Zugriff auf den Aufruferbuffer fortleben.
- Gültigen Task-/ISR-Kontext, Nebenläufigkeit und Serialisierung spezifizieren.
  Keine ISR-Sicherheit oder Thread-Sicherheit ohne Nachweis behaupten.
- DMA-Anforderungen, Transfergrenzen und mögliche Zwischenpuffer anhand der
  tatsächlichen Umgebung prüfen. Keine PSRAM-, Alignment- oder RAM-Annahme.
- VCOM, Chip Select, elektrische Pegel und Timing nur aus bestätigtem
  Displaydatenblatt und Schaltung ableiten. Sleep und Wiederanlauf separat prüfen.
- Devcontainer, Dotfiles und Editorprofil nur auf belegter Basis ändern.
  Bestehende Konfiguration respektieren; keine privaten Installationsskripte raten.

Ergebnis: Integrationsänderung mit Versions-/Hardwarebezug, Ressourcen- und
Aufrufkonzept, ausgeführte Builds/Tests und klar bezeichnete Hardwarelücken.
Bei fehlender Hardwaregrundlage nur davon unabhängige Arbeiten durchführen.
