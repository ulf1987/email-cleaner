# Email Cleaner & Organizer (n8n)

Dieses Projekt enthält einen n8n-Workflow zur automatischen Bereinigung und Organisation von Yahoo- und AOL-Postfächern via IMAP. Es nutzt **Google Gemini** zur intelligenten Klassifizierung.

## Voraussetzungen

### 1. n8n
Eine laufende Instanz von n8n (lokal oder Cloud).

### 2. E-Mail Zugangsdaten (WICHTIG!)
Für Yahoo und AOL können **nicht** die normalen Login-Passwörter für den IMAP-Zugriff verwendet werden. Du musst sogenannte **App-Passwörter** generieren.

**Yahoo:**
1. Gehe zu den Account-Sicherheits-Einstellungen.
2. Scrolle zu "App-Passwort generieren".
3. Wähle "Andere App" und nenne sie z.B. "n8n".
4. Kopiere das generierte Passwort.

**AOL:**
1. Gehe zu den Account-Sicherheits-Einstellungen (ähnlich wie Yahoo).
2. Generiere ein App-Passwort.

### 3. Google Gemini API Key
Der Workflow nutzt das Google Gemini Modell (eingestellt auf `gemini-3.0-pro`, anpassbar).
1. Hole dir einen API Key via [Google AI Studio](https://aistudio.google.com/).
2. Erstelle in n8n ein Credential für "Google Gemini API".

## Datenschutz & Sicherheit

Bei der Analyse der E-Mails werden Daten (Betreff, Absender, Inhalt) an die Google API gesendet.

*   **Training:** Bei Nutzung von kostenpflichtigen Diensten oder Vertex AI werden deine Daten laut Google **nicht** zum Training der Modelle verwendet. Bei kostenlosen "Free Tier" Keys in AI Studio *kann* Google Daten zur Verbesserung nutzen. Prüfe bitte die aktuellen [Google Generative AI Nutzungsbedingungen](https://policies.google.com/terms).
*   **Keine lokale Speicherung:** Dieser Workflow speichert keine E-Mail-Inhalte permanent in n8n, er leitet sie nur durch.
*   **Empfehlung:** Für maximale Privatsphäre nutze keine privaten "Free Tier" Accounts für hochsensible Daten oder schalte in Google AI Studio die Datennutzung (falls Option verfügbar) explizit aus.

## Workflow Struktur

Die Datei `workflow.json` kann direkt in n8n importiert werden.

**Logik:**
1. **Trigger:** Startet periodisch (z.B. alle 30 Min).
2. **Fetch:** Holt E-Mails via IMAP (Yahoo & AOL).
3. **Analyze (Gemini):** Sendet Metadaten an Gemini zur Klassifizierung:
   - `Finance`, `Newsletter`, `Social`, `Travel`, `Personal`, `Junk`
4. **Switch & Action:**
   - Verschiebt die E-Mail in den entsprechenden Ordner.
   - Löscht Spam.

## Installation

1. Öffne n8n.
2. Erstelle einen neuen Workflow.
3. Klicke oben rechts auf das Menü (...) -> "Import from File".
4. Wähle die `workflow.json` aus diesem Repository.
5. Konfiguriere die IMAP Credentials.
6. **WICHTIG:** Der Node "AI Classify (Placeholder)" ist nur ein Platzhalter (um Import-Fehler zu vermeiden).
   - Lösche diesen Node.
   - Füge einen **Google Gemini Chat** Node an derselben Stelle ein.
   - Verbinde ihn (Input von Yahoo/AOL, Output zu Switch Category).
   - Prompt: "Analysiere diese E-Mail und kategorisiere sie in: Finance, Newsletter, Social, Travel, Personal, Junk. Antworte NUR mit dem Kategorie-Namen."
   - Übergebe Betreff/Body als User Message.