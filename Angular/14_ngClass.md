[Zurück zur Übersicht](../README.md)

# [ngClass] – Dynamische CSS-Klassen

---

## Was ist ngClass?

Mit dem Angular-Directive `ngClass` kannst du CSS-Klassen dynamisch im Template
setzen – abhängig von Variablen oder Bedingungen. Das ist ideal, um z.B.
Statusfarben, Hervorhebungen oder Sichtbarkeiten zu steuern.

**Wichtig:**  
Damit `ngClass` funktioniert, muss das `CommonModule` im Modul (bzw. bei
Standalone-Komponenten im `imports`-Array) importiert werden.

---

## Syntax

### Objekt-Syntax

```html
<p [ngClass]="{ className: condition }">Text</p>
```

- Die Klassen werden als Objekt übergeben:
  - Schlüssel = CSS-Klassenname (als String)
  - Wert = Ausdruck oder Variable (boolean)

### Array-Syntax

```html
<p [ngClass]="['class1', 'class2']">Text</p>
```

- Es werden alle Klassen im Array gesetzt.

### String-Syntax

```html
<p [ngClass]="'class1 class2'">Text</p>
```

- Ein String mit Leerzeichen-getrennten Klassennamen.

---

## Beispiel: Dynamische Klassen mit Bedingungen

Du kannst mehrere Klassen abhängig von Bedingungen setzen:

```html
<p [ngClass]="{ fontColorBad: item.stars < 3, fontColorGood: item.stars >= 3 }">
  ({{ item.stars }})
</p>
```

- Ist `item.stars` kleiner als 3, wird die Klasse `fontColorBad` gesetzt.
- Ist `item.stars` mindestens 3, wird die Klasse `fontColorGood` gesetzt.

---

## Dynamische Klassen mit Variablen

Auch Variablen können genutzt werden:

```html
<p [ngClass]="{ active: isActive, disabled: !isActive }">Text</p>
```

---

## Mehrere Klassen als Array

Du kannst auch ein Array von Klassennamen übergeben:

```html
<p [ngClass]="['class1', 'class2']">Text</p>
```

---

## Klassen als String

Auch ein String mit mehreren Klassennamen ist möglich:

```html
<p [ngClass]="'class1 class2'">Text</p>
```

---

## Unterschied zu [class.className]

Mit `[class.className]` setzt du nur eine einzelne Klasse:

```html
<p [class.active]="isActive">Text</p>
```

Mit `ngClass` kannst du mehrere Klassen gleichzeitig und dynamisch setzen.

---

## Zusammenfassung

- `ngClass` erlaubt das dynamische Setzen von CSS-Klassen im Template.
- Bedingungen und Variablen steuern, welche Klassen aktiv sind.
- Das `CommonModule` ist Voraussetzung für die Nutzung von `ngClass`.
- Drei Syntaxmöglichkeiten: Objekt, Array, String.
- Ideal für Statusfarben, Hervorhebungen und mehr.

---
