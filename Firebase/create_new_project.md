[Zurück zur Übersicht](../README.md)

# Firebase-Projekt anlegen und mit Angular verbinden

## Inhaltsverzeichnis

1. [Firebase-Projekt erstellen](#1-firebase-projekt-erstellen)
2. [Firebase CLI installieren und einrichten](#2-firebase-cli-installieren-und-einrichten)
3. [Firebase mit Angular verbinden](#3-firebase-mit-angular-verbinden)

## 1. Firebase-Projekt erstellen

1. Gehe auf [firebase.google.com](https://firebase.google.com/).
2. Klicke oben rechts auf **Console**.
3. Erstelle ein neues Firebase-Projekt.
4. Erstelle eine Firestore-Datenbank:  
   Navigiere zu **Entwickeln → Firestore Database** und klicke auf **Datenbank
   erstellen**.

## 2. Firebase CLI installieren und einrichten

1. Installiere die Firebase CLI global:
   ```bash
   npm install -g firebase-tools
   ```
2. Melde dich in der CLI an:
   ```bash
   firebase login
   ```
3. Teste, ob die CLI korrekt installiert ist und auf dein Konto zugreift:
   ```bash
   firebase projects:list
   ```

## 3. Firebase mit Angular verbinden

1. Installiere AngularFire in deinem Angular-Projekt:
   ```bash
   ng add @angular/fire
   ```
2. Wähle zusätzliche Funktionalitäten (z.B. Firestore) aus.  
   (Mit der Leertaste auswählen, mit Enter bestätigen.)
3. Wähle das zuvor angelegte Firebase-Projekt aus.
4. Die Konfigurationsdaten findest du in der `app.config`.  
   Sollte es zu einem Fehler bei `projectNumber` oder `version` in der
   `app.config` kommen, entferne diese Felder aus dem `initializeApp`-Block.
