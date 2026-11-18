# ADR-0002: Graphics, IDisplay und Sharp-Treiber trennen

- Datum: 2026-11-18
- Status: Schichtentrennung angenommen; konkrete C++-API ist Entwurf
- Herkunft: [Quellenübersicht](README.md), jüngster Display-Entwurf

## Kontext

Der Nutzer entwickelt die Graphics Library selbst. Der erste Sharp-Treiber
kann separat delegiert und später ersetzt werden. UI und Graphics sollen
weder ESP-IDF-Header noch Sharp-spezifische Operationen benötigen.

## Entscheidung

```text
Application/UI ---> Graphics ---> display_core
Application/UI ---> IDisplay (in display_core)
                         ^
                         | implementiert
                  SharpMemoryLcd ---> Encoder + ESP-IDF-Transport
                         ^
               alternativ: FakeDisplay für Host-Tests
```

Die Anwendung erzeugt Speicher und konkrete Objekte und injiziert `IDisplay`.
Graphics zeichnet Pixel, Formen, Bitmaps und Strings mit festen Bitmap-Fonts.
Graphics kennt den Sharp-Treiber nicht; der Sharp-Treiber kennt Graphics nicht.
`display_core` enthält nur hardwareunabhängige Verträge.

`IDisplay` bietet Größe, Initialisierung und synchrones `present()` gemäß
[ADR-0001](0001-application-owned-framebuffer.md). Es bietet keine
`drawPixel()`-, `drawText()`- oder Font-Funktionen. Initialisierung und
Transportfehler werden explizit gemeldet; eine erfolgreiche Übertragung
beweist keine optisch korrekte Anzeige.

## C++-API-Vorschlag aus dem Gespräch

Diese Deklarationen dienen als Review-Grundlage. Es existieren noch keine
freigegebenen Header; vor einer Implementierung Typen, Validierung und
Fehlerpriorität konkretisieren. Kein C++-Standard ist hiermit gewählt.

```cpp
#include <cstddef>
#include <cstdint>

namespace smartscale::display {
struct FramebufferView {
    const std::uint8_t* data;
    std::size_t sizeBytes;
    std::uint16_t width;
    std::uint16_t height;
    std::size_t strideBytes;
};

struct DisplaySize {
    std::uint16_t width;
    std::uint16_t height;
};

enum class DisplayResult {
    Ok, InvalidArgument, NotInitialized, TransportError
};

class IDisplay {
public:
    virtual ~IDisplay() = default;
    virtual DisplaySize size() const = 0;
    virtual DisplayResult initialize() = 0;
    virtual DisplayResult present(const FramebufferView& frame) = 0;
};
} // namespace smartscale::display
```

Für Graphics ist eine entsprechende `MutableFramebufferView` mit veränderbarem
Pointer vorgesehen. Beide Views teilen Format und Größenregeln. Fontformat,
Glyphen-Metadaten, fehlende Zeichen und Clipping-Verhalten werden beim
Graphics-Entwurf festgelegt; feste Bitmap-Fonts sind bereits gewählt.
Transparente und deckende Bitmap-Ausgabe sollen explizit unterscheidbar sein.

`InvalidArgument` betrifft ungültige Views und Abweichungen von der Displaygröße.
`NotInitialized` betrifft Ausgabe vor erfolgreicher Initialisierung.
`TransportError` steht für erforderliche fehlgeschlagene Hardwareoperationen.
Welche Meldung bei mehreren gleichzeitigen Fehlerbedingungen Vorrang hat,
ist noch festzulegen. Wiederholte Initialisierung, Recovery und Nebenläufigkeit
sind ebenfalls noch nicht spezifiziert.

## Alternativen

- Ein gemeinsames Graphics-/Display-Objekt reduziert zunächst die Zahl der
  Typen, verhindert aber die gewünschte unabhängige Entwicklung.
- Eine universelle HAL für alle Busse erhöht den Entwurfsumfang ohne bisher
  belegten Bedarf. Eine kleine Transportgrenze im Sharp-Treiber genügt.

## Konsequenzen

Host-Tests können Graphics direkt am Speicher und die UI mit `FakeDisplay`
prüfen. Ein Fake muss den Inhalt während `present()` kopieren, wenn er ihn
nach Rückkehr vergleichen möchte; bloßes Speichern der View reicht nicht.

Die Komponentennamen `display_core`, `graphics`, `sharp_memory_lcd` und eine
Integration in `main` sind eine geplante Zuordnung. Das ESP-IDF-Projekt ist
dieses Repository, mit `main/main.c` und bislang leeren Komponentenverzeichnissen.
Neue Komponenten gegebenenfalls direkt unter `components/` integrieren. Das im Gespräch vorgeschlagene
`firmware/components/` nicht parallel erzeugen. Keine leeren Komponenten
allein zur Abbildung des Diagramms erzeugen.
