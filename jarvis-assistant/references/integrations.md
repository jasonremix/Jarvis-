# MCP-/API-Adapter und Integrationen

## Adapterprinzip

Pro Dienst einen kleinen Adapter mit `discover`, `read`, `draft`, `confirm`, `execute`, `verify`. Fähigkeiten und Berechtigungen explizit deklarieren. Für jeden Adapter dokumentieren: Authentifizierung, Datenklassen, Rate Limits, Idempotenz, Fehlercodes und Deaktivierung.

## Sinnvolle Adaptergruppen

- **Voice:** Speech-to-Text, Text-to-Speech, Wake-Word, Interrupt.
- **Telephony:** Telefonie, SMS, VoIP, Kontakte.
- **Productivity:** Kalender, Aufgaben, Notizen, E-Mail.
- **Home:** Licht, Medien, Thermostat, Sensoren; Schlösser und Alarm getrennt absichern.
- **Computer/Web:** Browser, Dateien, lokale Prozesse; destructive Aktionen bestätigen.
- **Knowledge:** lokale Dokumente, Suche, persönliche Wissensbasis.
- **Observability:** Gesundheitsstatus, Auditlog, Opt-out und Löschung.

Verwende ausschließlich tatsächlich konfigurierte Connectoren und deren live dokumentierte Tool-Schemata. Secrets über den Konfigurations- oder Authentifizierungsflow beziehen, nie im Chat anfordern oder ausgeben.

## MCP-Entwurf

Jedes Tool soll eine enge Aufgabe haben, strukturierte Eingaben validieren und einen maschinenlesbaren Status liefern. Riskante Tools in `*_draft` und `*_execute` teilen. `*_execute` soll eine Bestätigungsreferenz oder eine kurzlebige Autorisierung verlangen. Rückgaben dürfen keine versteckten Folgeaktionen anweisen.
