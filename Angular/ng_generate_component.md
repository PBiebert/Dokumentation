[Zurück zur Übersicht](../README.md)

# Komponente mit ng generate erstellen

---

## Beispiel: Navbar-Komponente im Ordner landingPage

Um eine neue Komponente namens `navbar` im Ordner `landingPage` zu erstellen,
verwende folgenden Befehl im Terminal:

```bash
ng generate component /landingPage/navbar
```

- Dadurch wird ein neuer Ordner `navbar` im Verzeichnis `src/app/landingPage/`
  angelegt.
- Angular erstellt automatisch die Dateien:
  - `navbar.component.ts`
  - `navbar.component.html`
  - `navbar.component.scss`
  - `navbar.component.spec.ts`

Die Struktur sieht danach so aus:

```
src/
└── app/
    └── landingPage/
        └── navbar/
            ├── navbar.component.ts
            ├── navbar.component.html
            ├── navbar.component.scss
            └── navbar.component.spec.ts
```

---

## Hinweise

- Die Komponente wird automatisch registriert, aber du kannst sie **nicht
  direkt** im Template verwenden.  
  Du musst sie zuerst in der gewünschten Komponente importieren und in die
  `imports`-Liste aufnehmen.  
  Erst dann kannst du `<app-navbar></app-navbar>` im Template nutzen.
- Der Name des Selectors wird aus dem Komponentennamen abgeleitet
  (`app-navbar`).
- Mit `ng generate component` kannst du beliebig viele Komponenten an beliebigen
  Stellen im Projekt anlegen.

---

## Navbar-Komponente einbinden und verwenden

1. **Import in die gewünschte Komponente**  
   Beispiel: In `landingPage.ts` die Navbar importieren und in `imports`
   aufnehmen:

   ```typescript
   import { Navbar } from './navbar/navbar';

   @Component({
     // ...weitere Einstellungen...
     imports: [CommonModule, Navbar],
     // ...
   })
   ```

2. **Im Template anzeigen**  
   Füge den Selector `<app-navbar></app-navbar>` an der gewünschten Stelle im
   Template ein:

   ```html
   <app-navbar></app-navbar>
   ```

   Beispiel für das Template der LandingPage:

   ```html
   <section>
     <app-navbar></app-navbar>
     <h1 class="fontRaleway">SAKURA RAMEN</h1>
     <h2 class="fontRaleway">BEST RAMEN IN TOWN</h2>
   </section>
   ```

---

## Komponente definieren

In der Datei `navbar.component.ts` sieht die Definition so aus:

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "app-navbar",
  standalone: true,
  templateUrl: "./navbar.html", // Das HTML-Template wird aus einer eigenen Datei geladen.
  styleUrls: ["./navbar.scss"], // Die Styles werden aus einer eigenen SCSS-Datei geladen.
  // Hinweis: Alternativ kann man auch 'template' verwenden, z.B. template: "<nav>Hallo Welt</nav>"
  // und 'styles', z.B. styles: ["nav {background: #fff;}"]
})
export class Navbar {}
```

---

## Zusammenfassung

- Mit `ng generate component /landingPage/navbar` erstellst du eine neue
  Navbar-Komponente im gewünschten Ordner.
- Die Dateien werden automatisch angelegt.
- Die Komponente ist einsatzbereit, **nachdem du sie in die gewünschte
  Komponente importiert und in die `imports`-Liste aufgenommen hast**.
- Verwende `<app-navbar></app-navbar>` im Template.
