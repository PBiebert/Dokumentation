[Back to Table of Contents](../README.md)

# Angular-Projekt bauen (Build)

---

## Inhaltsverzeichnis

1. [Projekt bauen](#projekt-bauen)
2. [Build-Optionen](#build-optionen)
3. [Hinweis zu Routing und Source Maps](#hinweis-zu-routing-und-source-maps)
4. [Ergebnis](#ergebnis)
5. [Zusammenfassung](#zusammenfassung)

---

## Projekt bauen

1. **Terminal öffnen**  
   Öffne ein Terminal im Hauptverzeichnis deines Projekts.

2. **Build-Befehl ausführen**  
   Führe folgenden Befehl aus:

   ```bash
   ng build
   ```

   Dadurch wird das Projekt für die Produktion gebaut.  
   Die gebauten Dateien findest du im Ordner `dist/`.

---

## Build-Optionen

- **Produktions-Build (empfohlen für Deployment):**

  ```bash
  ng build --configuration production
  ```

- **Build für Deployment in einem Unterordner (z.B. auf einem Server unter
  `/angular-projects/PROJEKTNAME/`):**

  ```bash
  ng build --base-href "/angular-projects/PROJEKTNAME/"
  ```

- **Build mit Source Maps (für Debugging):**

  ```bash
  ng build --source-map=true
  ```

- **Kombination aus Source Maps und Base-Href:**

  ```bash
  ng build --source-map=true --base-href "/angular-projects/PROJEKTNAME/"
  ```

---

## Hinweis zu Routing und Source Maps

> **Achtung:**  
> Wenn du dein Projekt mit `--source-map=true` baust, kann es passieren, dass
> das Routing auf dem Server mit `.htaccess` nicht richtig funktioniert.  
> Zum Beispiel: Wenn du eine Unterseite direkt im Browser aufrufst oder die
> Seite neu lädst, bekommst du einen Fehler (z.B. „Not Found“), obwohl die Route
> eigentlich existiert.  
> In solchen Fällen schicke bitte immer den Link zu deinem Git-Repository mit,
> damit das Problem genauer analysiert werden kann.

---

## Ergebnis

- Nach dem Build findest du die fertigen Dateien im Ordner `dist/`.
- Diese Dateien kannst du auf einen Webserver hochladen.

---

## Zusammenfassung

- Mit `ng build` erstellst du eine produktionsreife Version deiner Angular-App.
- Die gebauten Dateien liegen im `dist/`-Ordner.
