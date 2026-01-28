[Zurück zur Übersicht](../README.md)

# Sicheres Ablegen des Firebase API Keys in Angular

## 1. Ordner für Umgebungsvariablen anlegen

Lege im `src/app`-Verzeichnis einen Ordner namens `environments` an (falls noch
nicht vorhanden):

```
src/app/environments/
```

## 2. Datei `environments.ts` erstellen

Erstelle in diesem Ordner die Datei `environments.ts` und speichere dort das
Firebase-Konfigurationsobjekt:

```typescript
// src/app/environments/environments.ts
export const firebaseEnvironments = {
  projectId: "DEIN_PROJECT_ID",
  appId: "DEINE_APP_ID",
  storageBucket: "DEIN_STORAGE_BUCKET",
  apiKey: "DEIN_API_KEY",
  authDomain: "DEINE_AUTH_DOMAIN",
  messagingSenderId: "DEINE_MESSAGING_SENDER_ID",
};
```

> **Hinweis:** Ersetze die Platzhalter durch deine echten
> Firebase-Konfigurationsdaten.

## 3. Verwendung in der `app.config.ts`

Importiere das Objekt in deiner `app.config.ts` und verwende es für die
Initialisierung:

```typescript
// src/app/app.config.ts
import { firebaseEnvironments } from './environments/environments';
// ...bestehender Code...
provideFirebaseApp(() => initializeApp(firebaseEnvironments)),
// ...bestehender Code...
```

## 4. Datei in `.gitignore` eintragen

Füge den Ordner `environments` zur `.gitignore` hinzu, damit sensible Daten
nicht ins Repository gelangen:

```
# Environments
/src/app/environments
```

## 5. Hinweise

- Teile die Datei `environments.ts` niemals öffentlich.
- Gib die Datei nur an Teammitglieder weiter, die Zugriff auf das Projekt
  benötigen.
