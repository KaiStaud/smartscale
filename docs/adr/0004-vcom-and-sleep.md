# ADR-0004: VCOM-Strategie und Verhalten im Sleep

- Datum: 2026-11-18
- Status: Offen – keine Hardwarelösung beschlossen
- Herkunft: [Quellenübersicht](README.md), jüngster Display-Entwurf

## Kontext

Der bisherige Entwurf nennt VCOM als kritische modellabhängige Anforderung.
Eine notwendige Umschaltung muss auch bei unverändertem Bild und den
vorgesehenen Sleep-Zuständen gewährleistet sein. Das konkrete Display,
seine Beschaltung und die geplanten Sleep-Modi sind noch nicht belegt.

## Feststehende Architekturvorgabe

Die Erfüllung der VCOM-Anforderung darf nicht von der Häufigkeit von
UI-Aufrufen an `present()` abhängen. Die Verantwortung liegt auf Treiber- und
Hardwareintegrationsseite, nicht in Graphics oder der UI.

Diese Vorgabe entscheidet noch nicht, welche Hardware oder Software die
Umschaltung ausführt. Ein periodischer Task allein ist kein Nachweis für
Funktion im Sleep. Keine Frequenz oder zulässige Unterbrechungszeit annehmen.

## Offene Alternativen

- Externe Signalerzeugung über einen dafür vorgesehenen Displayeingang,
  soweit Modell und Beschaltung dies unterstützen.
- Ein geeigneter Hardware-Timing-Pfad des Systems, sofern dessen Funktion in
  allen geforderten Betriebszuständen nachgewiesen ist.
- Umschaltung über Display-Kommandos mit unabhängigem Zeitplan, sofern Modell
  und Energie-/Sleep-Anforderungen dies zulassen.

Es ist keine dieser Alternativen ausgewählt. Auch Abschalten des Displays
im Sleep wäre eine neue Produkt- und Hardwareentscheidung samt definiertem
Abschalt-/Wiederanlaufablauf, keine stillschweigende VCOM-Lösung.

## Zum Schließen erforderlich

| Fehlender Nachweis | Zu klärender Inhalt |
| --- | --- |
| Display | Exakte Modellnummer, Datenblatt und Revision |
| Aufbau | Nacktes Panel oder Breakout, Schaltungsrevision, VCOM-Modus und Anschlüsse |
| Elektrik/Timing | Zulässige Pegel, Umschaltbedingungen und Grenzwerte laut Datenblatt |
| Produktverhalten | Anzeigeinhalt und Versorgung während Idle, Light Sleep und Deep Sleep |
| System | Tatsächlich geforderte Sleep-Modi und verfügbare Zeit-/Signalquellen |
| Lebenszyklus | Start, Transferfehler, Sleep-Eintritt, Wakeup und Abschalten |
| Validierung | Messung des relevanten Signals in allen geforderten Zuständen |

## Konsequenzen und Abschlusskriterium

Hardwareunabhängige Verträge, Fakes und Graphics-Arbeit können fortgesetzt
werden. VCOM-Implementierung und Hardwarefreigabe bleiben offen.

Ein Folge-ADR muss die gewählte Alternative mit Quellen, Schaltungsbezug,
Timingbudget, Fehlerverhalten, Energieauswirkung und Messergebnissen begründen.
Ein erfolgreicher Bildtransfer allein schließt diese ADR nicht.
