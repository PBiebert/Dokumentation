[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Personal Access Token (PAT)

## Was ist ein Personal Access Token?

Ein Personal Access Token (PAT) wird zur Authentifizierung bei GitHub verwendet
– zum Beispiel beim Pushen oder Pullen von privaten Repositories – anstelle
eines Passworts.

## Erstellung eines PAT

1. Klicke auf dein Profil → **Einstellungen**
2. Gehe zu **Entwicklereinstellungen**
3. Wähle **Personal Access Tokens**

### Option 1: Fein abgestimmte Tokens

- Schritt für Schritt:
  1. Token-Namen festlegen
  2. Beschreibung (optional)
  3. Ablaufdatum festlegen
  4. Repository-Zugriff einstellen (z.B. nur bestimmte Repos)
  5. Einschränkungen für Aktionen festlegen (z.B. Lesen, Schreiben)
  6. **Token generieren** klicken
  7. Token speichern – Achtung: Nach Verlassen der Seite ist das Token nicht
     mehr sichtbar!

### Option 2: Klassische Tokens

- Ältere, weniger granulare Tokens
- Gewähren Zugriff auf alle Repositories je nach Berechtigungen
- Weniger flexibel und weniger fein abgestimmt

## Wichtig

**Fein abgestimmte Tokens** sind die sichere Wahl für moderne GitHub-Workflows,
da sie präzise Berechtigungen und ein Ablaufdatum bieten. **Klassische Tokens**
sollten nur verwendet werden, wenn fein abgestimmte Tokens nicht kompatibel
sind.
