[Zurück zur Übersicht](../README.md)

# @HostBinding – Eigenschaften und Klassen am Host-Element binden in Angular

---

## Inhaltsverzeichnis

1. [Was ist @HostBinding?](#was-ist-hostbinding)
2. [Vorteile von @HostBinding](#vorteile-von-hostbinding)
3. [Typische Anwendungsfälle](#typische-anwendungsfälle)
4. [Beispiel: CSS-Klasse am Host-Element binden](#beispiel-css-klasse-am-host-element-binden)
5. [Beispiel: Attribut am Host-Element binden](#beispiel-attribut-am-host-element-binden)
6. [Beispiel: Style am Host-Element binden](#beispiel-style-am-host-element-binden)
7. [Schritt-für-Schritt: @HostBinding verwenden](#schritt-für-schritt-hostbinding-verwenden)
8. [Methodenübersicht](#methodenübersicht)
9. [Hinweise](#hinweise)
10. [Fazit](#fazit)
11. [Beispiel: Mehrere Bindings in einer Directive](#beispiel-mehrere-bindings-in-einer-directive)

---

## Was ist @HostBinding?

`@HostBinding` ist ein Decorator in Angular, mit dem du Eigenschaften,
Attribute, Klassen oder Styles direkt am Host-Element deiner Komponente oder
Directive binden kannst.  
So kannst du z.B. CSS-Klassen, Attribute oder Inline-Styles dynamisch setzen,
ohne das Template anpassen zu müssen.

---

## Vorteile von @HostBinding

- **Direkte Bindung** von Eigenschaften am Host-Element
- **Saubere Trennung** von Logik und Template
- **Einfache Syntax** für viele Anwendungsfälle
- **Plattformunabhängig** und testbar

---

## Typische Anwendungsfälle

- Dynamisches Hinzufügen/Entfernen von CSS-Klassen am Host-Element
- Setzen von Attributen wie `disabled`, `aria-*`, `tabindex` etc.
- Dynamische Styles direkt am Host-Element

---

## Beispiel: CSS-Klasse am Host-Element binden

```typescript
// highlight.directive.ts
import { Directive, HostBinding } from "@angular/core";

@Directive({
  selector: "[appHighlight]",
  standalone: true,
})
export class HighlightDirective {
  @HostBinding("class.active") isActive = true;
}
```

> **Hinweis:**  
> Mit `@HostBinding("class.active")` wird die CSS-Klasse `active` am
> Host-Element gesetzt, wenn `isActive` true ist.

---

## Beispiel: Attribut am Host-Element binden

```typescript
// disabled.directive.ts
import { Directive, HostBinding } from "@angular/core";

@Directive({
  selector: "[appDisabled]",
  standalone: true,
})
export class DisabledDirective {
  @HostBinding("attr.disabled") isDisabled = true;
}
```

> **Hinweis:**  
> Mit `@HostBinding("attr.disabled")` wird das Attribut `disabled` am
> Host-Element gesetzt, wenn `isDisabled` true ist.

---

## Beispiel: Style am Host-Element binden

```typescript
// color.directive.ts
import { Directive, HostBinding } from "@angular/core";

@Directive({
  selector: "[appColor]",
  standalone: true,
})
export class ColorDirective {
  @HostBinding("style.backgroundColor") bgColor = "yellow";
}
```

---

## Schritt-für-Schritt: @HostBinding verwenden

1. **Importiere HostBinding** aus `@angular/core`.
2. **Deklariere eine Property** mit `@HostBinding("...")`.
3. **Setze den gewünschten Wert** in der Property (z.B. boolean, string,
   number).
4. **Optional:** Ändere den Wert dynamisch in deiner Logik.

---

## Methodenübersicht

| Syntax                            | Beschreibung                   |
| --------------------------------- | ------------------------------ |
| @HostBinding("class.klassenname") | Setzt/entfernt CSS-Klasse      |
| @HostBinding("attr.attributname") | Setzt/entfernt Attribut        |
| @HostBinding("style.stilname")    | Setzt Inline-Style             |
| @HostBinding("propertyName")      | Bindet native Property am Host |

---

## Hinweise

- **Mehrere Bindings:** Du kannst beliebig viele `@HostBinding` Properties in
  einer Klasse verwenden.
- **Namensgebung:** Der String im Decorator gibt an, was am Host-Element
  gebunden wird (z.B. `"class.active"`, `"attr.aria-label"`).
- **Dynamik:** Die Werte können jederzeit im Code geändert werden, z.B. durch
  Methoden oder Lifecycle-Hooks.

---

## Fazit

Mit `@HostBinding` kannst du Eigenschaften, Attribute, Klassen und Styles direkt
am Host-Element binden.  
Das sorgt für übersichtlichen Code und eine klare Trennung zwischen Template und
Logik.

---

## Beispiel: Mehrere Bindings in einer Directive

```typescript
// multi-binding.directive.ts
import { Directive, HostBinding } from "@angular/core";

@Directive({
  selector: "[appMultiBinding]",
  standalone: true,
})
export class MultiBindingDirective {
  @HostBinding("class.active") isActive = true;
  @HostBinding("attr.aria-label") ariaLabel = "Aktives Element";
  @HostBinding("style.border") border = "2px solid red";
}
```

> In diesem Beispiel werden gleichzeitig eine CSS-Klasse, ein Attribut und ein
> Style am Host-Element gebunden.
