[Back to Table of Contents](../README.md)

# Canvas

## Allgemeines zu Canvas

Das `<canvas>`-Element in HTML ermöglicht das dynamische Zeichnen von Grafiken,
Animationen und Spielen direkt im Browser. Es stellt eine Zeichenfläche bereit,
die mit JavaScript bearbeitet werden kann.

### Grundstruktur

```html
<canvas id="canvas" width="720" height="480"></canvas>
```

### Kontext holen

Um auf die Zeichenfläche zuzugreifen, benötigt man den Zeichenkontext:

```javascript
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
```

### Wichtige Methoden

- `clearRect(x, y, width, height)`: Löscht einen Bereich der Zeichenfläche.
- `drawImage(img, x, y, width, height)`: Zeichnet ein Bild auf die
  Zeichenfläche.
- `fillRect(x, y, width, height)`: Zeichnet ein gefülltes Rechteck.
- `strokeRect(x, y, width, height)`: Zeichnet ein Rechteck mit Rahmen.
- `beginPath()`, `moveTo(x, y)`, `lineTo(x, y)`, `stroke()`: Zeichnet Linien und
  Pfade.

### Bilder zeichnen

```javascript
const img = new Image();
img.src = "bild.png";
img.onload = () => {
  ctx.drawImage(img, 0, 0, 100, 100);
};
```

### Animation mit `requestAnimationFrame`

Mit `requestAnimationFrame` kann man Animationen flüssig darstellen, indem die
Zeichenfunktion immer wieder aufgerufen wird. Die Bildrate (Frames pro Sekunde)
ist dabei nicht fest, sondern hängt von der Leistung der Grafikkarte und des
Systems ab. Moderne Browser versuchen, möglichst synchron zur Bildwiederholrate
des Monitors zu rendern (meist 60 FPS), können aber bei schwächerer Hardware
weniger Frames liefern.

```javascript
function draw() {
  // ...zeichnen...
  requestAnimationFrame(draw);
}
draw();
```

> **Hinweis:** Die tatsächliche Anzahl der Frames pro Sekunde wird durch die
> Hardware und die Auslastung des Systems bestimmt. Je leistungsfähiger die
> Grafikkarte, desto mehr Bilder können pro Sekunde dargestellt werden.

### Objektverwaltung

Mehrere Objekte können in Arrays gespeichert und in jedem Frame gezeichnet
werden. Damit Bewegungen sichtbar werden und keine "Schlieren" entstehen, muss
das Canvas vor jedem neuen Frame geleert werden:

```javascript
function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height); // Canvas leeren
  addObjectsToMap(objects); // Alle Objekte neu zeichnen
  requestAnimationFrame(draw); // Nächsten Frame anfordern
}

function addObjectsToMap(objects) {
  objects.forEach((object) => {
    ctx.drawImage(object.img, object.x, object.y, object.width, object.height);
  });
}
draw();
```

> **Hinweis:** Das Leeren des Canvas mit `clearRect` ist notwendig, damit sich
> bewegende Objekte nicht mehrfach angezeigt werden und die Animation sauber
> bleibt.
