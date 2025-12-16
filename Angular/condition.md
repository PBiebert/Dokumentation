[Zurück zur Übersicht](../README.md)

# Bedingungen und Kontrollstrukturen im Angular Template

---

## Was sind Bedingungen?

Bedingungen steuern, ob ein bestimmter Block im Template angezeigt wird. Dafür
nutzt Angular Vergleichs- und logische Operatoren sowie spezielle
Template-Syntax.

---

## Logische Operatoren

### NICHT-Operator (!)

Negiert den Wahrheitswert.

```html
@if (!isActive) {
<!-- Wird angezeigt, wenn isActive false ist -->
}
```

### ODER-Operator (||)

Mindestens eine Bedingung muss wahr sein.

```html
@if (isAdmin || isOwner) {
<!-- Wird angezeigt, wenn isAdmin oder isOwner true ist -->
}
```

### UND-Operator (&&)

Beide Bedingungen müssen wahr sein.

```html
@if (isLoggedIn && hasProfile) {
<!-- Wird angezeigt, wenn beide Bedingungen true sind -->
}
```

---

## Vergleichsoperatoren

```html
@if (count > 5) { ... } @if (user.name === "Anna") { ... } @if (score != 0) {
... }
```

---

## Ternary Operator (Kurz-If)

Auch im Template nutzbar:

```html
{{ lang === 'de' ? 'Webseite' : 'Website' }}
```

---

## if-else Bedingungen

Mehrere Fälle mit `@if`, `@else if`, `@else` prüfen:

```html
@if (item.stars >= count) {
<img src="./assets/img/stars/star.svg" alt="Voller Stern" />
} @else if (item.stars >= count - 0.5) {
<img src="./assets/img/stars/star-half.svg" alt="Halber Stern" />
} @else {
<img src="./assets/img/stars/star-empty.svg" alt="Leerer Stern" />
}
```

---

## switch/case

Mehrere Fälle übersichtlich prüfen – ähnlich wie in JavaScript:

```html
@switch (true) { @case(item.stars >= count) {
<img src="./assets/img/stars/star.svg" alt="Voller Stern" />
} @case(item.stars >= count - 0.5) {
<img src="./assets/img/stars/star-half.svg" alt="Halber Stern" />
} @default {
<img src="./assets/img/stars/star-empty.svg" alt="Leerer Stern" />
} }
```

---

## Zusammenfassung

- Bedingungen steuern die Anzeige im Template.
- Logische (&&, ||, !) und Vergleichsoperatoren (==, ===, >, <, ...) sind die
  Basis.
- Mit `@if`, `@else if`, `@else` und `@switch`/`@case`/`@default` prüfst du
  verschiedene Fälle.
- Der Ternary Operator ist eine Kurzform für einfache Bedingungen.

---
