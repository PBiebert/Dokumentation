[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Merge-Konflikte

Ein Merge-Konflikt tritt auf, wenn mehrere Personen gleichzeitig an derselben
Datei oder sogar an derselben Zeile arbeiten.

- Git kann die Änderungen in diesem Fall nicht automatisch zusammenführen.
- Es tritt ein Fehler auf, weil Git nicht weiß, welche Änderung übernommen
  werden soll.

## Schritte bei einem Merge-Konflikt

1. **Git/GitHub erkennt und markiert den Konflikt.**
2. In VS Code öffnet sich ein spezielles Fenster:
   - **Incoming:** Code vom anderen Entwickler / Remote-Branch
   - **Current:** Dein eigener Code / lokaler Branch
   - **Result:** Vorschau, wie die Datei nach dem Zusammenführen aussehen wird
3. **Optionen in VS Code:**
   - Änderungen akzeptieren oder ignorieren
   - Einzelne Blöcke auswählen oder beide kombinieren
4. **Abschluss:**
   - Nach der Bearbeitung den Konflikt als gelöst markieren
   - Commit (`git commit`) und Push (`git push`)
   - Andere Entwickler müssen dann `git pull` ausführen, um die aktualisierte
     Version zu erhalten

## Wichtig

Merge-Konflikte sind ein Sicherheitsmechanismus von Git, um versehentliche
Überschreibungen zu verhindern. Sie erfordern manuelles Eingreifen und bewusstes
Zusammenführen.
