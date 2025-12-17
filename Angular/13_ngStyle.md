[Zurück zur Übersicht](../README.md)

# [ngStyle] – Dynamische Styles

---

## Was ist ngStyle?

Mit dem Angular-Directive `ngStyle` kannst du CSS-Styles direkt und dynamisch im
Template setzen. Das ist besonders praktisch, wenn sich Styles abhängig von
Variablen oder Bedingungen ändern sollen.

**Wichtig:**  
Damit `ngStyle` funktioniert, muss das `CommonModule` im Modul (bzw. bei
Standalone-Komponenten im `imports`-Array) importiert werden.

---

## Syntax

```html
<p [ngStyle]="{ color: 'red', 'background-color': 'black' }">Text</p>
```

- Die Styles werden als Objekt übergeben:
  - Schlüssel = CSS-Eigenschaft (im CamelCase oder mit Bindestrich in
    Anführungszeichen)
  - Wert = Ausdruck oder Variable

---

## Dynamische Werte

Du kannst Werte aus der Komponente verwenden:

```html
<p [ngStyle]="{ color: fontColor }">Text</p>
```

```typescript
fontColor = "blue";
```

---

## Ternary Operator (Kurz-If) mit ngStyle

Du kannst auch den ternären Operator direkt im ngStyle-Objekt nutzen, um Styles
abhängig von Bedingungen zu setzen:

```html
<p [ngStyle]="{ color: item.stars > 3 ? fontColorGood : fontColorBad }">
  ({{ item.stars }})
</p>
```

---

## Mehrere Styles dynamisch setzen

Du kannst beliebig viele Styles gleichzeitig setzen:

```html
<div
  [ngStyle]="{
  color: isActive ? 'green' : 'gray',
  'font-size': fontSize + 'px',
  'background-color': bgColor
}"
>
  Dynamischer Style!
</div>
```

---

## Typische Anwendungsfälle

- Farben je nach Status (z.B. Fehler = rot)
- Schriftgröße abhängig von Benutzereingaben
- Sichtbarkeit oder Hervorhebung dynamisch steuern

---

## Unterschied zu [style.property]

Mit `[style.property]` setzt du nur eine einzelne CSS-Eigenschaft:

```html
<p [style.color]="fontColor">Text</p>
```

Mit `ngStyle` kannst du mehrere Eigenschaften gleichzeitig setzen.

---

## Zusammenfassung

- `ngStyle` erlaubt das dynamische Setzen mehrerer CSS-Styles im Template.
- Werte können direkt aus der Komponente kommen.
- Das `CommonModule` ist Voraussetzung für die Nutzung von `ngStyle`.
- Ideal für dynamische, zustandsabhängige Gestaltung.

---
