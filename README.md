# Jarvis

Ein persönlicher, sprachzentrierter JARVIS-Assistent für natürliche Dialoge, Telefonie, Nachrichten, Kalender, Erinnerungen, Smart-Home-, Computer-, Web- und MCP-Aktionen.

> <span style="color: red"><strong>Wichtiger Hinweis zur Telefonie:</strong> Das Tätigen echter Telefonanrufe muss selbst eingerichtet werden. Dafür ist eine autorisierte Telefonie- oder VoIP-Integration wie Twilio erforderlich. Der JARVIS-Skill allein stellt keine Telefonleitung bereit.</span>

## Fähigkeiten

- natürliche Sprachbefehle und Text-Fallback
- sichere Orchestrierung von Aktionen und MCP-Tools
- Telefonie und Nachrichten über ausdrücklich konfigurierte Connectoren
- Kalender, Erinnerungen und proaktive Hilfe nach vorheriger Freigabe
- Smart-Home-, Computer- und Web-Aktionen
- Risiko- und Bestätigungsstufen für externe oder dauerhafte Aktionen

## Telefonie einrichten

Für echte Anrufe muss zuerst ein Telefonie-Connector eingerichtet und autorisiert werden. Geeignete Optionen sind beispielsweise Twilio Programmable Voice oder ein kompatibler VoIP-Dienst. Der Adapter sollte Anrufvorbereitung, explizite Bestätigung, Anrufstart und Statusprüfung getrennt behandeln.

Vor jedem Anruf müssen Zielnummer, Zweck und Gesprächseröffnung eindeutig bestätigt werden. Zugangsdaten, Tokens und vollständige Gesprächsdaten gehören nicht in dieses Repository.

## Skill-Dateien

Die vollständige Skill-Beschreibung und die Referenzdokumente befinden sich unter [`jarvis-assistant/`](./jarvis-assistant/).
