# Email Cleaner & Organizer (n8n)

Dieses Projekt enthält einen n8n-Workflow zur automatischen Bereinigung und Organisation von Yahoo- und AOL-Postfächern via IMAP.

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

### 3. KI-Modell (Optional aber empfohlen)
Um "sinnvolle" Ordner zu erstellen und Spam intelligent zu erkennen, nutzt der Workflow idealerweise ein LLM (OpenAI oder Google Gemini).
- Benötigt: API Key (in n8n Credentials hinterlegen).

## Workflow Struktur

Die Datei `workflow.json` kann direkt in n8n importiert werden.

**Logik:**
1. **Trigger:** Startet periodisch (z.B. alle 15 Min) oder manuell.
2. **Fetch:** Holt die letzten 20-50 E-Mails via IMAP (Yahoo & AOL Nodes).
3. **Analyze:** Extrahiert Betreff, Absender und Body-Snippet.
4. **Classify (AI):** Die KI entscheidet eine Kategorie:
   - `Finance` (Rechnungen, Bank)
   - `Newsletter` (Werbung, News)
   - `Social` (Benachrichtigungen)
   - `Personal` (Echte Konversationen)
   - `Spam` (Unerwünscht)
5. **Route & Action:**
   - Verschiebt die E-Mail in den entsprechenden Ordner auf dem Server.
   - Löscht Spam (Verschiebt in Trash).

## Installation

1. Öffne n8n.
2. Erstelle einen neuen Workflow.
3. Klicke oben rechts auf das Menü (...) -> "Import from File".
4. Wähle die `workflow.json` aus diesem Repository.
5. Konfiguriere die "IMAP Credential" Nodes mit deinen Daten (User + App-Passwort).
