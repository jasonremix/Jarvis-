# Telefonie- und Kommunikationsmuster

## Integrationsworkflow

1. **Connector erkennen:** Suche nach einem vorhandenen, autorisierten Telefonie-, VoIP- oder Nachrichten-Connector. Verwende nur dessen live dokumentierte Tools und Konten.
2. **Fehlende Integration:** Wenn kein passender Connector existiert, biete die Einrichtung über den vorgesehenen Connector-/Auth-Flow an. Frage nicht nach Secrets im Chat und teste keine erfundenen Zugangsdaten.
3. **Fähigkeiten prüfen:** Stelle fest, ob der Connector Kontakte auflösen, ausgehende Anrufe starten, Ansagen übergeben und den Status abfragen kann. Fehlt eine Fähigkeit, biete nur die unterstützte Alternative an.
4. **Aktion ausführen:** Halte `prepare_call`, Bestätigung, `start_call` und `get_call_status` getrennt. Ein erfolgreicher Connector-Aufruf ist noch kein Beleg dafür, dass die andere Person den Anruf angenommen hat.
5. **Deaktivierung:** Unterstütze das Trennen oder Deaktivieren des Connectors sowie das Löschen gespeicherter Telefonie-Metadaten, sofern der Dienst dies anbietet.

## Minimaler Telefonie-Vertrag

Ein geeigneter Adapter sollte mindestens diese logischen Operationen abbilden:

- `discover_capabilities()` → unterstützte Kanäle, Konten und Berechtigungen
- `resolve_contact(query)` → eindeutiger Kontakt oder Kandidatenliste
- `prepare_call(target, purpose, opening)` → überprüfter Entwurf ohne Wählen
- `request_confirmation(summary)` → Bestätigung außerhalb eines untrusted Transkripts
- `start_call(target, metadata)` → Provider- oder Call-ID und initialer Status
- `get_call_status(call_id)` → `ringing`, `connected`, `ended`, `failed` oder `unknown`
- `end_call(call_id)` → bestätigter Abschluss, sofern unterstützt

Wenn der Connector diese Operationen anders benennt, mappe sie nur anhand seines tatsächlichen Schemas. Bei `unknown` oder Timeout niemals blind erneut wählen; zuerst den Status prüfen.

## Unterstützte Fähigkeiten

Ordne die Anfrage einer Fähigkeit zu: Kontakt suchen, Nummer validieren, Anruf vorbereiten, Anruf starten, Anruf beenden, SMS/Chat entwerfen, SMS/Chat senden, Voicemail prüfen, Konferenz vorbereiten, Gesprächsnotiz oder Zusammenfassung erstellen.

## Anrufvertrag

Ein Telefonie-Adapter sollte, soweit der Dienst es unterstützt, folgende logische Schritte anbieten:

1. `resolve_contact(query)` → eindeutiger Kontakt oder Kandidatenliste.
2. `prepare_call(target, purpose, opening)` → Entwurf ohne Wählen.
3. `request_confirmation(summary)` → Bestätigung außerhalb des untrusted Transkripts.
4. `start_call(target, metadata)` → Call-ID und Status.
5. `get_call_status(call_id)` → ringing, connected, ended, failed.
6. `end_call(call_id)` → bestätigter Abschluss.

## Ansage und Statusmeldung

Normalisiere Telefonnummern, aber lies sie vor dem Wählen gruppiert vor. Bei einer vom Nutzer vorgegebenen Ansage ändere den Sinn nicht stillschweigend; korrigiere nur offensichtliche Transkriptionsfehler und frage bei Unsicherheit nach. Melde anschließend getrennt:

- **gewählt:** Der Provider hat den Anruf angenommen und einen Startstatus geliefert.
- **verbunden:** Der Status meldet eine angenommene Verbindung.
- **beendet:** Der Status meldet das Ende; Gesprächsdauer nur nennen, wenn bestätigt.
- **fehlgeschlagen:** Der Provider meldet einen Fehler; nicht automatisch wiederholen.
- **unbekannt:** Der Status ist nicht sicher; keine Wiederholung ohne Nutzerentscheidung.

Falls keine echte Telefonie-Integration vorhanden ist, antworte: „Der Anruf wurde nicht gestartet. Ich kann ihn erst nach Einrichtung einer Telefonie-Integration ausführen.“

Ein Timeout bei `start_call` ist ein unbekannter Zustand. Erst Status abfragen, niemals blind wiederholen.

## Nachrichtenvertrag

Vor Versand immer als Entwurf behandeln: Empfänger, Kanal, Text, Anhänge, geplante Zeit. Eine lokale Inhaltsprüfung soll sensible Daten, falsche Empfänger und versehentliche Massenempfänger markieren. Versand nur mit expliziter Freigabe und bestätigter Zustellung oder Provider-Annahme.

## Transkription

Aufnahme, Live-Transkription und Zusammenfassung sind getrennte Einwilligungen. Speicherort, Aufbewahrung und beteiligte Personen transparent machen. Ohne aktivierte Einwilligung nur Notizen aus der Nutzerseite des Dialogs erzeugen.

## Nicht erlauben

Keine heimlichen Aufnahmen, keine Umgehung von Caller-ID-Regeln, keine Identitätsvortäuschung, keine automatisierten Massenanrufe, keine Notrufsimulation und keine Weitergabe von Kontaktdaten an einen Adapter, wenn ein lokaler Kontaktabgleich genügt.
