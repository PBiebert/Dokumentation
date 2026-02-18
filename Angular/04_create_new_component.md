[Zurück zur Übersicht](../README.md)

# Eine neue Komponente in Angular erstellen

---

## Inhaltsverzeichnis

1. [Beispiel: LandingPage-Komponente](#beispiel-landingpage-komponente)
2. [Ordner und Datei anlegen](#1-ordner-und-datei-anlegen)
3. [Komponente erstellen](#2-komponente-erstellen)
4. [Komponente im Hauptmodul einbinden](#3-komponente-im-hauptmodul-einbinden)
5. [Komponente im Template anzeigen](#4-komponente-im-template-anzeigen)
6. [Zusammenfassung](#zusammenfassung)

---

## 1. Beispiel: LandingPage-Komponente

Hier wird Schritt für Schritt erklärt, wie du eine neue Komponente namens
`LandingPage` im Angular-Projekt anlegst, was dabei passiert und wofür die
einzelnen Teile da sind.

---

## 2. Ordner und Datei anlegen

Lege im `src/app`-Verzeichnis einen neuen Ordner für deine Komponente an. Das
sorgt für Übersichtlichkeit, besonders bei größeren Projekten.

```
src/
└── app/
    └── landingPage/
        └── landingPage.ts
```

- **Ordner `landingPage`**: Enthält alle Dateien, die zu dieser Komponente
  gehören.
- **Datei `landingPage.ts`**: Hier wird die Komponente definiert.

---

## 3. Komponente erstellen

In der Datei `landingPage.ts` definierst du die Komponente. Das sieht so aus:

```typescript
import { CommonModule } from "@angular/common";
import { Component } from "@angular/core";

// Der @Component-Dekorator macht aus der Klasse eine Angular-Komponente.
// Hier werden wichtige Einstellungen gemacht:
@Component({
  selector: "app-landingPage", // Mit diesem Tag kannst du die Komponente im HTML verwenden.
  standalone: true, // Die Komponente ist eigenständig und braucht kein Angular-Modul.
  imports: [CommonModule], // Importiert Standard-Direktiven wie *ngIf, *ngFor.
  templateUrl: "./landingPage.html", // Das HTML-Template wird aus einer eigenen Datei geladen.
  styleUrls: ["./landingpage.scss"], // Die Styles werden aus einer eigenen SCSS-Datei geladen.
})
export class LandingPageComponent {}
```

**Was passiert hier?**

- Mit `@Component` wird die Klasse als Komponente registriert.
- `selector` legt fest, wie du die Komponente im HTML einfügst
  (`<app-landingPage></app-landingPage>`).
- `standalone: true` bedeutet, dass die Komponente unabhängig von einem
  Angular-Modul funktioniert.
- `imports` bindet andere Module ein, die du im Template brauchst.
- `template` ist das HTML, das angezeigt wird.
- `styles` sind die spezifischen Styles für diese Komponente.

> **Hinweis:**  
> Alternativ kann man auch `template` verwenden, z.B.
> `template: "<h1>Hallo Welt</h1>"`  
> und `styles`, z.B. `styles: ["h1 {color: red;}"]`

---

## 4. Komponente im Hauptmodul einbinden

Damit Angular weiß, dass es die neue Komponente gibt, musst du sie in deiner
Hauptdatei `app.ts` importieren und in die `imports`-Liste aufnehmen:

```typescript
import { LandingPageComponent } from "./landingPage/landingPage";

@Component({
  selector: "app-root",
  imports: [RouterOutlet, LandingPageComponent], // Hier wird die neue Komponente eingebunden
  templateUrl: "./app.html",
  styleUrl: "./app.scss",
})
export class App {
  protected readonly title = signal("sakura");
}
```

**Was passiert hier?**

- Die `LandingPageComponent` wird importiert.
- In `imports` wird sie registriert, damit sie im Template von `App` verwendet
  werden kann.
- Der `selector: 'app-root'` ist der Einstiegspunkt deiner Anwendung.

---

## 5. Komponente im Template anzeigen

Jetzt kannst du die Komponente im Template (`app.html`) anzeigen, indem du ihren
Selector verwendest:

```html
<app-landingPage></app-landingPage> <router-outlet />
```

**Was passiert hier?**

- `<app-landingPage></app-landingPage>` zeigt die neue Komponente an der
  gewünschten Stelle an.
- `<router-outlet />` ist für das Routing zuständig (andere Seiten/Komponenten).

---

## 6. Zusammenfassung

- **Ordnerstruktur**: Jede Komponente bekommt ihren eigenen Ordner für
  Übersichtlichkeit.
- **Komponente definieren**: Mit `@Component` wird die Komponente samt Template
  und Styles erstellt.
- **Einbinden**: Die Komponente wird im Hauptmodul importiert und registriert.
- **Anzeigen**: Über den Selector kannst du die Komponente im Template
  platzieren.

Mit diesem Vorgehen kannst du beliebig viele eigene Komponenten in deinem
Angular-Projekt anlegen, strukturieren und wiederverwenden.
