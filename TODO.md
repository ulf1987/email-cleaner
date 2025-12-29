# Offene Aufgaben (Stand: 29.12.2025)

## Wichtig für Morgen
- [ ] **Gemini Integration:**
    - Den Platzhalter-Node "AI Classify (Placeholder)" durch den echten `Google Gemini Chat` Node ersetzen.
    - Prompt optimieren: Sicherstellen, dass nur sauberes JSON oder ein einzelnes Wort zurückkommt (für den Switch-Node).
- [ ] **E-Mail Verschieben (Move Logic):**
    - Die "NoOp" Nodes (Mock Move...) ersetzen.
    - **Herausforderung:** Standard IMAP kann in n8n oft nicht verschieben.
    - **Lösung:** Python-Script Node einbauen (`imaplib`) ODER Gmail-Integration prüfen (falls Accounts verknüpfbar).
- [ ] **Fehlerbehandlung:**
    - Was passiert, wenn Gemini "Ich bin mir nicht sicher" antwortet? -> Fallback-Ordner.

## Langfristig
- [ ] Cron-Job Zeitplan finalisieren (aktuell alle 30 Min).
- [ ] Logging: Protokollieren, welche E-Mail wohin geschoben wurde (z.B. in ein Google Sheet oder Log-File).
