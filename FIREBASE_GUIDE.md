# Firebase Security & Setup Guide

Dieses Dokument hilft Ihnen, Ihre Firebase-Datenbank abzusichern und Probleme beim Speichern zu beheben.

## 1. Daten speichern funktioniert nicht? (Security Rules)

Wenn Sie die Meldung "Missing or insufficient permissions" erhalten, sind Ihre Schreibrechte in Firebase deaktiviert.

**Lösung:**
1. Gehen Sie in die [Firebase Console](https://console.firebase.google.com/).
2. Wählen Sie Ihr Projekt aus.
3. Klicken Sie links auf **Firestore Database**.
4. Wechseln Sie zum Tab **Rules** (Regeln).
5. Ersetzen Sie die vorhandenen Regeln durch diese (für den Anfang):

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /messungen/{document=**} {
      // Erlaubt jedem das Lesen und Schreiben (Nur für die Entwicklung!)
      // Später sollten Sie hier eine Authentifizierung hinzufügen.
      allow read, write: if true;
    }
  }
}
```

6. Klicken Sie auf **Publish** (Veröffentlichen).

## 2. API-Schlüssel ist öffentlich

In einer reinen Browser-App (HTML/JS) ist der Schlüssel technisch immer sichtbar. Damit niemand anderes Ihren Schlüssel für seine Projekte nutzt, müssen Sie ihn einschränken.

**Lösung (Einschränkung auf Ihre Domain):**
1. Gehen Sie zur [Google Cloud Console - API & Services](https://console.cloud.google.com/apis/credentials).
2. Suchen Sie nach dem Schlüssel (meist "Browser key" oder ähnlich dem in `firebase-config.js`).
3. Klicken Sie auf den Namen des Schlüssels, um ihn zu bearbeiten.
4. Unter **Set an application restriction** wählen Sie **Websites**.
5. Fügen Sie unter **Website restrictions** Ihre Domain hinzu (z. B. `https://ihre-domain.de/*` oder `http://localhost/*` für lokale Tests).
6. Speichern Sie die Änderungen.

## 3. Lokale Entwicklung (Wichtig!)

Da wir jetzt `type="module"` und separate Dateien verwenden, müssen Sie die `index.html` über einen Webserver starten (z. B. VS Code Extension "Live Server"). Das einfache Doppelklicken auf die Datei im Explorer funktioniert evtl. nicht mehr wegen Sicherheitsbeschränkungen des Browsers für Module.
