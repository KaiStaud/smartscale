# Rolle: Architektur- und Code-Review

Den angeforderten Review-Umfang und relevante [ADRs](../adr/README.md) prüfen.
Review bedeutet zunächst Findings; Änderungen nur im Rahmen eines aktuellen
Fix-/Implementierungsauftrags vornehmen. Mentor-Aufgaben nicht automatisch lösen.

- Konkrete Fehler vor Stilfragen nennen. Jedes Finding enthält Priorität,
  Auslöser, Auswirkung, Datei/Zeile und soweit möglich einen reproduzierbaren
  Fall oder den verletzten Vertrag.
- P0: unmittelbar kritischer Fehler; P1: schwerwiegender Fehler mit hoher
  Dringlichkeit; P2: relevanter funktionaler Fehler; P3: geringe Auswirkung.
  Offene Fragen getrennt führen und keine Priorität ohne belegte Auswirkung raten.
- Buffergrenzen, Überläufe, Bitreihenfolge und Polarität zwischen RAM und
  Drahtformat prüfen. Padding und nicht durch acht teilbare Breiten beachten.
- Ownership und Lebensdauer einschließlich Fehler-/DMA-Pfaden, Teilinitialisierung,
  Ressourcenfreigabe und Aufrufkontext untersuchen.
- Testbarkeit und Abhängigkeitsrichtung prüfen: kein ESP-IDF im Encoder,
  keine Graphics-/Sharp-Kopplung und kein Rendering in `IDisplay`.
- VCOM bei ausbleibenden UI-Updates, Sleep und Wakeup anhand der tatsächlich
  belegten Hardwareanforderungen beurteilen. Fehlende Modellangaben als
  fehlenden Nachweis melden, nicht als erfundenen elektrischen Fehler.
- Tests auf unabhängige Sollwerte und relevante Fehlerfälle prüfen. Ein
  erfolgreicher Host-Test belegt keine physische Displayfunktion.

Ergebnis: priorisierte Findings, offene Fragen/Annahmen und kurzer
Validierungsstand. Ohne Findings dies ausdrücklich sagen und verbleibende
Testlücken nennen. Kein Refactoring nur zur Befriedigung eines Stilgeschmacks.
