[Zurück zur Übersicht](../README.md)

# `DOCUMENT` – Zugriff auf das globale Dokument-Objekt in Angular

---

## Was ist `DOCUMENT`?

`DOCUMENT` ist ein Injection Token aus `@angular/common`, mit dem du in Angular
auf das globale `document`-Objekt des Browsers zugreifen kannst.  
Damit kannst du z.B. direkt mit dem DOM arbeiten, Styles oder Klassen am
`<body>` setzen oder globale Events behandeln – und das plattformunabhängig.

---

## Vorteile von `DOCUMENT`

- **Direkter Zugriff** auf das native DOM-Dokument
- **Plattformunabhängig** (z.B. für Server-Side Rendering)
- **Testbarkeit** durch Dependency Injection
- **Saubere Trennung** von Logik und Template

---

## Typische Anwendungsfälle

- Manipulation von `<body>`-Klassen (z.B. Scroll sperren)
- Zugriff auf globale DOM-Elemente
- Arbeiten mit Cookies, Meta-Tags oder globalen Styles
- Integration von externen Skripten oder Tracking

---

## Beispiel: Klasse am `<body>` setzen

```typescript
// example.component.ts
import { Component, inject } from "@angular/core";
import { DOCUMENT } from "@angular/common";

@Component({
  selector: "app-example",
  template: '<button (click)="toggleScroll()">Scroll sperren</button>',
  standalone: true,
})
export class ExampleComponent {
  private document = inject(DOCUMENT);

  toggleScroll() {
    this.document.body.classList.toggle("no-scroll-y");
  }
}
```

> **Hinweis:**  
> Mit `inject(DOCUMENT)` erhältst du das native Dokument-Objekt.  
> So kannst du z.B. Klassen am `<body>` setzen oder entfernen.

---

## Beispiel: Scrollposition auslesen

```typescript
// scroll-position.component.ts
import { Component, inject } from "@angular/core";
import { DOCUMENT } from "@angular/common";

@Component({
  selector: "app-scroll-position",
  template:
    '<button (click)="logScrollPosition()">Scrollposition anzeigen</button>',
  standalone: true,
})
export class ScrollPositionComponent {
  private document = inject(DOCUMENT);

  logScrollPosition() {
    // Scrollposition des Fensters auslesen
    const scrollY = this.document.defaultView?.scrollY ?? 0;
    console.log("Aktuelle Scrollposition (Y):", scrollY);
  }
}
```

> **Was passiert in dieser Zeile?**  
> `const scrollY = this.document.defaultView?.scrollY ?? 0;`
>
> - `this.document.defaultView` gibt das globale Window-Objekt zurück, ähnlich
>   wie `window` in JavaScript.
> - Mit `?.scrollY` wird die aktuelle vertikale Scrollposition des Fensters
>   abgefragt.
> - Das `?? 0` sorgt dafür, dass bei `undefined` oder `null` als Fallback der
>   Wert `0` verwendet wird.
>
> **Vergleich zu klassischem JavaScript:**  
> In reinem JavaScript würdest du einfach `window.scrollY` schreiben.  
> In Angular nutzt du aber das injizierte `DOCUMENT`, um plattformunabhängig und
> testbar zu bleiben.
>
> **Kurz gesagt:**  
> Die Zeile liest die aktuelle Scrollposition des Fensters aus – und
> funktioniert auch bei SSR oder Tests, weil sie nicht direkt auf das globale
> Window zugreift.

---

## Schritt-für-Schritt: `DOCUMENT` verwenden

1. **Importiere `DOCUMENT`** aus `@angular/common`.
2. **Injiziere das Token** mit `inject(DOCUMENT)` oder im Konstruktor.
3. **Nutze das Dokument-Objekt** für DOM-Manipulationen.

---

## Methodenübersicht

| Syntax                        | Beschreibung                         |
| ----------------------------- | ------------------------------------ |
| inject(DOCUMENT)              | Dokument-Objekt injizieren           |
| document.body.classList.add() | Klasse am `<body>` hinzufügen        |
| document.querySelector()      | Element im DOM suchen                |
| document.cookie               | Zugriff auf Cookies                  |
| document.defaultView?.scrollY | Scrollposition des Fensters auslesen |

---

## Hinweise

- **Plattformunabhängig:**  
  Das `DOCUMENT`-Token funktioniert auch bei Server-Side Rendering (Angular
  Universal).
- **Testbarkeit:**  
  Durch Injection kannst du das Dokument-Objekt in Tests mocken.
- **Direkte DOM-Manipulation:**  
  Nutze `DOCUMENT` nur, wenn Angular-Mechanismen (z.B. Renderer2) nicht
  ausreichen.

---

## Beispiel: Direkter Zugriff auf `window.document` (nicht empfohlen)

Für sehr einfache, rein browserbasierte Angular-Projekte kannst du auch direkt
auf das globale `window.document` zugreifen – das ist aber **nicht best
practice** und kann bei Server-Side Rendering (SSR) oder beim Testen zu Fehlern
führen.

```typescript
// example.component.ts
import { Component } from "@angular/core";

@Component({
  selector: "app-direct-document",
  template:
    '<button (click)="logScrollPosition()">Scrollposition anzeigen</button>',
  standalone: true,
})
export class DirectDocumentComponent {
  logScrollPosition() {
    // Direkter Zugriff auf das globale window-Objekt
    const scrollY = window.scrollY;
    console.log("Aktuelle Scrollposition (Y):", scrollY);
  }
}
```

> **Achtung:**  
> Dieser Ansatz funktioniert **nur im Browser**.  
> Bei SSR oder in Unit-Tests ist `window` ggf. nicht definiert und dein Code
> wirft Fehler.  
> Für professionelle Angular-Projekte solltest du immer das `DOCUMENT`-Token
> verwenden.

---

### Zugriff auf das `<body>`-Element mit `window.document`

```typescript
const body = window.document.body;
body.classList.add("no-scroll-y");
```

Das ist technisch das Gleiche wie mit dem `DOCUMENT`-Token, aber **nicht
plattformunabhängig** und nicht testbar.  
Empfohlen wird weiterhin der Zugriff über `DOCUMENT`:

```typescript
const body = this.document.body;
body.classList.add("no-scroll-y");
```

> **Fazit:**  
> Beide Wege funktionieren im Browser, aber nur der Weg über `DOCUMENT` ist für
> Angular-Projekte best practice.

---

## Fazit

Mit dem `DOCUMENT`-Token kannst du in Angular sicher und plattformunabhängig auf
das globale Dokument-Objekt zugreifen.  
Das ist besonders nützlich für globale DOM-Manipulationen, die nicht direkt über
Angular-Templates oder Renderer2 möglich sind.

---

## Beispiel: Dynamisches Einfügen eines Skripts

```typescript
// script-loader.service.ts
import { Injectable, inject } from "@angular/core";
import { DOCUMENT } from "@angular/common";

@Injectable({ providedIn: "root" })
export class ScriptLoaderService {
  private document = inject(DOCUMENT);

  loadScript(src: string) {
    const script = this.document.createElement("script");
    script.src = src;
    script.async = true;
    this.document.body.appendChild(script);
  }
}
```

> In diesem Beispiel wird ein externes Skript dynamisch ins `<body>` eingefügt –
> plattformunabhängig und testbar.

---

## Weitere Infos & Links

Die offizielle Angular-Dokumentation zu `DOCUMENT` ist tatsächlich sehr knapp,
weil es sich um ein sogenanntes **Injection Token** handelt, das meist nur
intern oder von fortgeschrittenen Entwicklern genutzt wird.

- **API-Referenz:**  
  [Angular API: DOCUMENT](https://angular.io/api/common/DOCUMENT)  
  (Hier steht meist nur, dass es ein Injection Token für das globale Dokument
  ist.)

- **Weitere Infos & Beispiele:**
  - [StackOverflow: How to use DOCUMENT in Angular?](https://stackoverflow.com/search?q=angular+DOCUMENT)
  - [Angular Universal Guide (SSR)](https://angular.io/guide/universal#using-the-document-token)

**Tipp:**  
Suche nach Beispielen und Blogartikeln, wenn du mehr Praxisbeispiele brauchst –
die offizielle Doku ist hier bewusst minimal, weil das Token meist für spezielle
Anwendungsfälle gedacht ist.
