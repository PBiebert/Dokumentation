[Zurück zur Übersicht](../README.md)

# Property Binding – Eigenschaften von DOM-Elementen in Angular steuern

---

## Was ist Property Binding?

Property Binding ist eine zentrale Technik in Angular, mit der du Werte aus
deiner Komponente direkt an Eigenschaften (Properties) von DOM-Elementen oder
Angular-Komponenten binden kannst.  
So kannst du z.B. Texte, Attribute, CSS-Klassen oder andere Eigenschaften
dynamisch steuern – ganz ohne direktes DOM-Manipulieren.

---

## Vorteile von Property Binding

- **Direkte Verbindung** zwischen Komponente und Template
- **Automatische Aktualisierung** bei Wertänderungen
- **Saubere Trennung** von Logik und Darstellung
- **Sicher**: Schützt vor Cross-Site-Scripting (XSS)

---

## Typische Anwendungsfälle

- Dynamisches Setzen von Attributen (z.B. `src`, `href`, `disabled`)
- Steuerung von CSS-Klassen und Styles
- Übergabe von Werten an Kind-Komponenten (Input-Properties)
- Dynamische Anzeige und Steuerung von UI-Elementen

---

## Beispiel: Einfache Property-Bindings

```typescript
// app.component.ts
import { Component } from "@angular/core";

@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  standalone: true,
})
export class AppComponent {
  isDisabled = true;
  imageUrl = "https://angular.io/assets/images/logos/angular/angular.png";
  inputText = "Hallo Angular!";
}
```

```html
<!-- app.component.html -->
<button [disabled]="isDisabled">Klick mich!</button>
<img [src]="imageUrl" alt="Beispielbild" />
<input [value]="inputText" />
```

> **Hinweis:**  
> Die Syntax `[property]="expression"` bindet den Wert der Expression aus der
> Komponente an die Property des Elements.

---

## Property Binding vs. Attribut-Binding

- **Property Binding** (`[property]`): Bindet an die Eigenschaft des
  DOM-Elements (z.B. `disabled`, `value`, `checked`).
- **Attribut-Binding** (`attr.attribute`): Setzt das HTML-Attribut direkt (z.B.
  `attr.aria-label`).

```html
<!-- Property Binding -->
<button [disabled]="isDisabled"></button>

<!-- Attribut-Binding -->
<button [attr.aria-label]="label"></button>
```

---

## Beispiel: Binding an eine Kind-Komponente

```typescript
// counter.component.ts
import { Component, Input } from "@angular/core";

@Component({
  selector: "app-counter",
  templateUrl: "./counter.component.html",
  standalone: true,
})
export class CounterComponent {
  @Input() count = 0;
}
```

```html
<!-- counter.component.html -->
<p>Aktueller Wert: {{ count }}</p>
```

```html
<!-- app.component.html -->
<app-counter [count]="currentCount"></app-counter>
```

```typescript
// app.component.ts
// ...existing code...
currentCount = 5;
// ...existing code...
```

---

## Schritt-für-Schritt: Property Binding verwenden

1. **Definiere eine Property** in deiner Komponente.
2. **Nutze die eckigen Klammern** im Template: `[property]="expression"`.
3. **Optional:** Binde an Input-Properties von Kind-Komponenten.
4. **Bei Attributen:** Nutze ggf. `attr.`-Syntax für reine HTML-Attribute.

---

## Methodenübersicht

| Syntax                      | Beschreibung                      |
| --------------------------- | --------------------------------- |
| `[disabled]="isDisabled"`   | Button deaktivieren               |
| `[src]="imageUrl"`          | Bildquelle dynamisch setzen       |
| `[value]="inputText"`       | Wert eines Inputs setzen          |
| `[ngClass]="cssClasses"`    | CSS-Klassen dynamisch zuweisen    |
| `[count]="currentCount"`    | Wert an Kind-Komponente übergeben |
| `[attr.aria-label]="label"` | HTML-Attribut direkt setzen       |

---

## Hinweise

- **Einweg-Bindung:** Property Binding ist immer eine Einweg-Bindung (von
  Komponente zum Template).
- **Keine Strings:** Schreibe keine Anführungszeichen um die Property, sonst
  wird ein String gesetzt.
- **Sicherheit:** Angular schützt Property Bindings automatisch vor XSS.

---

## Fazit

Mit Property Binding steuerst du DOM-Eigenschaften und Komponenten-Inputs in
Angular einfach, sicher und deklarativ.  
Das sorgt für übersichtlichen, wartbaren und dynamischen Code.

---

## Beispiel: Dynamische CSS-Klassen und Styles

```typescript
// app.component.ts
// ...existing code...
isActive = true;
myStyles = { color: "red", fontWeight: "bold" };
// ...existing code...
```

```html
<!-- app.component.html -->
<div [ngClass]="{ active: isActive }" [ngStyle]="myStyles">
  Dynamischer Text
</div>
```

> Mit `[ngClass]` und `[ngStyle]` kannst du CSS-Klassen und Inline-Styles
> dynamisch binden.
