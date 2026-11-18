# ADR-0001: Anwendungseigener vollständiger 1-Bit-Framebuffer

- Datum: 2026-11-18
- Status: Angenommen als Architekturgrundlage; noch nicht implementiert
- Herkunft: [Quellenübersicht](README.md), jüngster Display-Entwurf

## Kontext

Die selbst entwickelte Graphics Library soll auf dem Host testbar sein und
mit austauschbaren Displaytreibern arbeiten. Der Treiber erhält ein fertiges
Bild. Weder die konkrete LCD-Auflösung noch verfügbare RAM-Bereiche sind belegt.

## Entscheidung

Die Anwendung stellt Speicher für einen vollständigen monochromen
Framebuffer bereit und besitzt ihn. Views sind nicht besitzend.

| Eigenschaft | Vertrag |
| --- | --- |
| Pixelformat | 1 Bit pro Pixel |
| Layout | Zeilenweise, Ursprung oben links |
| Bitreihenfolge im Speicher | Linkestes Pixel eines Bytes im höchstwertigen Bit |
| Polarität | 0 = Weiß, 1 = Schwarz |
| Stride | `width / 8 + (width % 8 != 0)` Bytes |
| Größe | `strideBytes * height` Bytes für das vollständige Bild |
| Padding | Ungenutzte niederwertige Bits der letzten Bytes einer Zeile sind 0 |
| Schreibzugriff | Graphics erhält eine nicht besitzende veränderbare View |
| Ausgabezugriff | Display erhält eine nicht besitzende konstante View |

`present()` überträgt synchron den vollständigen Bildinhalt. Speicher und
Bilddaten bleiben bis zur Rückkehr gültig und unverändert. Der Treiber
verändert den Eingabepuffer nicht und behält danach keine Referenz darauf.
Das gilt auch bei Fehlern: Ein gestarteter DMA-/Transportzugriff muss beendet
oder abgebrochen sein, bevor der Aufrufer den Speicher wiederverwenden darf.

MSB-first beschreibt das RAM-Format, nicht automatisch die SPI-Bitreihenfolge.
Eine notwendige Konvertierung der Polarität oder Reihenfolge übernimmt der
modellabhängige Encoder, ohne die Quelldaten zu verändern.

## Alternativen

- Treibereigener Framebuffer: koppelt Graphics an den Treiber und erschwert
  Austausch und Ownership-Verständnis.
- Zeichenbefehle in `IDisplay`: vermischen Rendering und Hardwareausgabe.
- Zeilenweises Rendering oder ausschließlich Dirty Rectangles: sparen unter
  Umständen RAM/Transfers, erhöhen aber den Umfang des ersten Vertrags.

## Konsequenzen und Prüfung

Der RAM-Bedarf wächst mit `ceil(width / 8) * height`; eine konkrete Zahl folgt
erst aus der bestätigten Auflösung. Speicherort und Allokationsart bleiben
offen. Keine Modellauflösung aus Gesprächsbeispielen ableiten.

Vor Speicherzugriffen Pointer, positive Dimensionen, exakten Stride und
Buffergröße sowie Überläufe bei Größenberechnungen prüfen. Der erste Vertrag
unterstützt keinen zusätzlichen Zeilenabstand. Eine spätere Erweiterung des
Strides oder asynchrone Ausgabe benötigt eine dokumentierte Vertragsänderung.

Tests sollen Bytegrenzen, nicht durch acht teilbare Breiten, Randpixel,
Padding und ungültige Views abdecken. Ein zweiter Framebuffer oder interne
Optimierungen werden damit nicht vorgegeben.
