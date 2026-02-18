# TypeScript-Variablen im HTML anzeigen: Die `{{}}`-Notation

In Angular kannst du Variablen aus deiner TypeScript-Komponente direkt im
HTML-Template anzeigen. Dafür nutzt du die sogenannte **Interpolation** mit der
`{{}}`-Notation.

---

## Inhaltsverzeichnis

1. [Beispiel](#beispiel)
2. [Hinweise](#hinweise)
3. [Zusammenfassung](#zusammenfassung)

---

## Beispiel

Angenommen, du hast in deiner Komponente folgende Variable definiert:

```typescript
export class AppComponent {
  title = "Meine Angular App";
}
```

Im zugehörigen HTML-Template kannst du diese Variable so anzeigen:

```html
<h1>{{ title }}</h1>
```

Das Ergebnis ist, dass im Browser die Überschrift mit dem Wert von `title`
angezeigt wird.

## Hinweise

- Innerhalb der `{{}}`-Klammern kannst du auch einfache Ausdrücke verwenden,
  z.B. `{{ title.toUpperCase() }}`.
- Die Interpolation ist nur zum Anzeigen von Daten gedacht, nicht zum Ausführen
  von komplexen Logiken.

## Zusammenfassung

Die `{{}}`-Notation ist der Standardweg in Angular, um Werte aus der Komponente
im Template darzustellen.
