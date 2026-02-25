# Flexbox-Layout: Scrollbare Container am Beispiel `.add-Task-Container`

## Ziel

Ein flexibles Layout, bei dem ein Container (`.add-Task-Container`) immer den
verfügbaren Platz einnimmt und bei Überfüllung eine Scrollbar erhält, ohne dass
das Eltern-Element (`.wrapper`) überläuft.

---

## Grundprinzip

Flexbox ermöglicht es, Container und deren Kinder flexibel zu gestalten. Ein
typisches Szenario:

- Ein Eltern-Element (`.wrapper`) nimmt den gesamten verfügbaren Platz ein
  (`height: 100vh` oder `max-height: 100vh`).
- Ein Kind-Element (`.add-Task-Container`) soll den verbleibenden Platz
  ausfüllen und bei zu viel Inhalt scrollen.

---

## Beispielstruktur

```html
<div class="wrapper">
  <div class="head-line">...</div>
  <div class="add-Task-Container">
    <!-- Viel Inhalt -->
  </div>
  <div class="foot-line">...</div>
</div>
```

---

## Wichtige CSS-Eigenschaften

### 1. Eltern-Element (`.wrapper`)

```scss
.wrapper {
  display: flex;
  flex-direction: column;
  height: 100vh; // Begrenzung auf Viewport-Höhe
  max-height: 100vh;
  overflow: hidden; // verhindert Überlaufen
}
```

### 2. Flex-Kinder

```scss
.head-line,
.foot-line {
  flex-shrink: 0; // Bleiben immer sichtbar, schrumpfen nicht
}

.add-Task-Container {
  flex: 1 1 0; // Nimmt den verfügbaren Platz ein
  min-height: 0; // Darf schrumpfen, auch wenn Inhalt größer ist
  overflow-y: auto; // Scrollbar erscheint bei Überfüllung
  display: flex; // Optional, falls weitere Flex-Kinder enthalten sind
}
```

### 3. Verschachtelte Flex-Kinder

Wenn `.add-Task-Container` weitere Flex-Kinder hat, sollten diese ebenfalls
`min-height: 0` und ggf. `flex-basis: 0` bekommen, damit sie korrekt schrumpfen.

---

## Warum ist das wichtig?

- **flex: 1 1 0** sorgt dafür, dass das Element den verfügbaren Platz einnimmt.
- **min-height: 0** ist entscheidend, damit das Element auch wirklich schrumpfen
  kann, selbst wenn der Inhalt größer ist.
- **overflow-y: auto** sorgt dafür, dass eine Scrollbar erscheint, sobald der
  Inhalt den Container überfüllt.

---

## Typische Fehlerquellen

- **Fehlendes `min-height: 0`:**  
  Das Flex-Kind kann nicht schrumpfen und sprengt den Eltern-Container.
- **Zu viel Padding/Margin:**  
  Der Inhalt wird größer als der Container, die Scrollbar erscheint nicht wie
  gewünscht.
- **Falsche Flexbox-Eigenschaften:**  
  Ohne `flex: 1` nimmt das Kind nicht den verfügbaren Platz ein.

---

## Zusammenfassung

Mit Flexbox kannst du Layouts bauen, bei denen ein Container immer den
verfügbaren Platz einnimmt und bei Überfüllung scrollt.  
Wichtige Eigenschaften sind `flex: 1 1 0`, `min-height: 0` und
`overflow-y: auto`.  
Das Eltern-Element sollte auf die Viewport-Höhe begrenzt werden und kein
Overflow zulassen.

---

## Beispiel aus deinem Projekt

```scss
.wrapper {
  display: flex;
  flex-direction: column;
  height: 100vh;
  max-height: 100vh;
  overflow: hidden;
}

.add-Task-Container {
  flex: 1 1 0;
  min-height: 0;
  overflow-y: auto;
}
```

---

## Weiterführende Links

- [MDN Flexbox Guide](https://developer.mozilla.org/de/docs/Web/CSS/CSS_Flexible_Box_Layout)
- [CSS Tricks Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
