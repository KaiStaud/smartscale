# Spezialisierte Arbeitsrollen

Die [AGENTS.md](../../AGENTS.md) im Projektroot ist der Einstieg. Sie weist
bei passender Aufgabe auf die folgenden ergänzenden Anweisungen hin:

| Rolle | Datei | Ergebnis |
| --- | --- | --- |
| Architektur | [architecture.md](architecture.md) | Vertragsprüfung, begründete ADRs und offene Entscheidungen |
| ESP-IDF | [esp-idf.md](esp-idf.md) | Versionsbezogene Integration und klare Ressourcen-/Kontextregeln |
| C++-Mentoring | [cpp-mentor.md](cpp-mentor.md) | Kleine Lernschritte und verständliches Feedback |
| Tests | [testing.md](testing.md) | Verhaltenstests und nachvollziehbare Nachweise |
| Reviews | [review.md](review.md) | Priorisierte, belegte Findings |

Die Rollendateien sind gewöhnliche Markdown-Arbeitsanweisungen, keine
registrierten Agenten oder ausführbaren Agentenkonfigurationen. Sie werden
über die Aufgabe und den Verweis in `AGENTS.md` gelesen; allein ihr Dateiname
aktiviert keine Rolle. Mehrere Rollen können in derselben Bearbeitung gelten.
Weitere Agenten werden dadurch nicht automatisch gestartet.

Beispielauftrag: „Prüfe den IDisplay-Entwurf anhand von
`docs/agents/architecture.md` und `docs/agents/review.md`.“

Die Platzierung der Root-Datei orientiert sich an der
[offiziellen OpenAI-Dokumentation zu AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md).
Sie beschreibt die Ermittlung von Projektanweisungen; eigene Rollendateien
wie diese sind eine zusätzliche Projektkonvention. Die vollständige
[AGENTS.md](../../AGENTS.md) liegt direkt im Root dieses Firmware-Repositories;
ein Verweis auf eine übergeordnete Agentendatei ist nicht erforderlich.
Alle Rollendateien und ADRs liegen ebenfalls innerhalb dieses Checkouts.
