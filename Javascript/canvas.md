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

---

## Canvas in der Spielmechanik

Canvas-Techniken werden häufig genutzt, um Spielwelten und deren Objekte
darzustellen und zu animieren.

### Initialisierung der Spielwelt

Die Spielwelt kann z.B. mit einer eigenen Klasse (`World`) verwaltet werden.
Beim Laden der Seite wird die Canvas initialisiert und die Zeichenlogik
gestartet.

```javascript
class World {
  constructor(canvas) {
    this.ctx = canvas.getContext("2d");
    this.canvas = canvas;
    this.draw();
  }

  draw() {
    this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height); // Canvas leeren
    this.addObjectsToMap(this.backgroundObjects);
    this.addObjectsToMap(this.lightBeams);
    this.addObjectsToMap(this.enemies);
    this.addToMap(this.character);

    let self = this;
    requestAnimationFrame(() => {
      self.draw();
    });
  }
}
```

> **Hinweis:** `this` wird oft in einer eigenen Variablen (`self`) gespeichert,
> da sich der Kontext von `this` in Callback-Funktionen wie bei
> `requestAnimationFrame` ändern kann. So bleibt der Bezug zur aktuellen Instanz
> erhalten.

### Anwendung der Canvas-Methoden im Spiel

- Hintergrund, Charakter, Gegner und Lichtstrahlen werden als eigene Klassen
  modelliert und in Arrays verwaltet.
- Die Methode `draw()` ruft die Zeichenfunktionen für alle Objekte auf und sorgt
  für die Animation.
- Die Animation erfolgt durch rekursiven Aufruf von `draw()` mit
  `requestAnimationFrame`.

---

## Arbeiten mit Bild-Animationen und Bild-Cache

### Bild-Cache mit `loadImages` füllen

Um Animationen flüssig darzustellen, werden alle benötigten Bilder vorab geladen
und im sogenannten Bild-Cache (`imageCache`) gespeichert. Dies geschieht in der
Klasse `MovableObject` mit der Methode `loadImages`:

```javascript
class MovableObject {
  imageCache = {};

  loadImages(array) {
    array.forEach((path) => {
      let img = new Image();
      img.src = path;
      this.imageCache[path] = img;
    });
  }
}
```

**Wichtig:**  
Damit die Animation funktioniert, muss `loadImages` im Konstruktor der
jeweiligen Klasse (z.B. `Character`) aufgerufen werden, und zwar mit dem Array
der Bildpfade:

```javascript
class Character extends MovableObject {
  IMAGES_SWIMMING = [
    /* ...Bildpfade... */
  ];

  constructor() {
    super().loadImage("src/img/1.Sharkie/1.IDLE/1.png");
    this.loadImages(this.IMAGES_SWIMMING); // Lädt alle Bilder in den Cache
    this.animate();
  }
  // ...existing code...
}
```

**Erklärung:**

- Zuerst wird das Startbild geladen.
- Danach werden alle Bilder für die Animation mit
  `this.loadImages(this.IMAGES_SWIMMING)` in den Cache geladen.
- Erst dann kann die Animation (`animate()`) korrekt auf die geladenen Bilder
  zugreifen.

---

## Hitboxen setzen und zeichnen

Um Kollisionen zu erkennen und die Begrenzungen von Objekten sichtbar zu machen,
kann für bestimmte Objekte eine Hitbox gezeichnet werden.  
Dazu wird in der Klasse `MovableObject` die Methode `drawFrame(ctx)` verwendet:

```javascript
class MovableObject {
  // ...existing code...
  drawFrame(ctx) {
    if (
      this instanceof Character ||
      this instanceof Fish ||
      this instanceof JellyFish
    ) {
      ctx.beginPath();
      ctx.lineWidth = "1";
      ctx.strokeStyle = "black";
      ctx.rect(this.x, this.y, this.width, this.height);
      ctx.stroke();
    }
  }
}
```

**Erklärung zu `instanceof`:**  
Mit `instanceof` wird geprüft, ob das aktuelle Objekt eine Instanz einer
bestimmten Klasse ist (z.B. `Character`, `Fish` oder `JellyFish`).  
Dadurch wird die Hitbox nur für diese Objekte gezeichnet und nicht für alle
anderen, wie z.B. Hintergrundobjekte.

Diese Methode wird nach dem Zeichnen des Bildes aufgerufen, z.B. in der
World-Klasse:

```javascript
class World {
  // ...existing code...
  addToMap(object) {
    object.draw(this.ctx); // Bild zeichnen
    object.drawFrame(this.ctx); // Hitbox zeichnen (nur für bestimmte Klassen)
    // ...existing code...
  }
}
```

**Hinweis:**  
Die Hitbox wird als schwarzer Rahmen um das Objekt angezeigt und hilft beim
Testen von Kollisionen sowie zur visuellen Kontrolle der Objektgrenzen.

---

## Kollisionsabfrage zwischen Objekten

Um zu erkennen, ob sich zwei Objekte im Spiel berühren (z.B. Charakter und
Gegner), wird eine Kollisionsabfrage implementiert. Diese prüft, ob sich die
Rechtecke (Hitboxen) der Objekte überschneiden.

### Kollisionsmethode in MovableObject

In der Klasse `MovableObject` gibt es die Methode `isColliding(object)`, die
prüft, ob das aktuelle Objekt mit einem anderen kollidiert:

```javascript
class MovableObject {
  // ...existing code...
  isColliding(object) {
    return (
      this.x + this.width > object.x &&
      this.y + this.height > object.y &&
      this.x < object.x &&
      this.y < object.y + object.height
    );
  }
}
```

**Erklärung:**  
Die Methode vergleicht die Positionen und Größen der beiden Objekte. Sie gibt
`true` zurück, wenn sich die Rechtecke überschneiden, andernfalls `false`.

### Kollisionsprüfung in der World-Klasse

Die Kollisionsabfrage wird regelmäßig in der `World`-Klasse durchgeführt. Dazu
gibt es die Methode `checkCollisions()`, die im Konstruktor der World-Klasse
aufgerufen wird:

```javascript
class World {
  constructor(canvas, keyboard) {
    // ...existing code...
    this.checkCollisions();
  }

  checkCollisions() {
    setInterval(() => {
      this.level.enemies.forEach((enemy) => {
        if (this.character.isColliding(enemy)) {
          this.character.hit(); // Schaden abziehen
        }
      });
    }, 200);
  }
}
```

**Erklärung:**  
Alle 200 Millisekunden wird geprüft, ob der Charakter mit einem der Gegner
kollidiert. Im Fall einer Kollision wird mit `this.character.hit();` die Energie
des Charakters reduziert.  
So kann einfach Schaden abgezogen werden, ohne zusätzliche Konsolenausgaben.

Die Methode kann später erweitert werden, um z.B. Animationen auszulösen oder
weitere Spiellogik zu ergänzen.

**Zusammengefasst:**

- Die Methode `isColliding()` prüft die Überschneidung zweier Objekte.
- In der World-Klasse wird regelmäßig für alle Gegner geprüft, ob eine Kollision
  mit dem Charakter vorliegt.
- Mit `this.character.hit();` kann direkt Schaden abgezogen werden.
- Die Kollisionsabfrage ist zentral für die Spielmechanik und kann beliebig
  erweitert werden.

---

## Steuerung mit Keyboard-Klasse

Um die Spielfigur per Tastatur zu steuern, wird eine eigene Klasse `Keyboard`
verwendet. Diese Klasse speichert den Zustand der relevanten Tasten als
Eigenschaften:

```javascript
class Keyboard {
  LEFT = false;
  RIGHT = false;
  UP = false;
  DOWN = false;
  SPACE = false;
}
```

Die Werte werden über Eventlistener gesetzt und zurückgesetzt:

```javascript
window.addEventListener("keydown", (event) => {
  switch (event.key) {
    case "w":
    case "ArrowUp":
      keyboard.UP = true;
      break;
    // ...existing code...
  }
});

window.addEventListener("keyup", (event) => {
  switch (event.key) {
    case "w":
    case "ArrowUp":
      keyboard.UP = false;
      break;
    // ...existing code...
  }
});
```

Das Keyboard-Objekt wird beim Initialisieren der Spielwelt erzeugt und an die
World-Klasse übergeben:

```javascript
let keyboard = new Keyboard();
let world = new World(canvas, keyboard);
```

**Wichtig:**  
Damit der Charakter auf das Keyboard zugreifen kann, muss im Konstruktor der
World-Klasse die Methode `setWorld()` aufgerufen werden:

```javascript
class World {
  constructor(canvas, keyboard) {
    this.ctx = canvas.getContext("2d");
    this.canvas = canvas;
    this.keyboard = keyboard;
    this.draw();
    this.setWorld(); // Muss im Konstruktor stehen!
  }

  setWorld() {
    this.character.world = this;
  }
}
```

Der Charakter kann dann über `this.world.keyboard` auf die Tastatur-Eingaben
zugreifen:

```javascript
class Character extends MovableObject {
  // ...existing code...
  move() {
    if (this.world.keyboard.RIGHT) {
      // Nach rechts bewegen
    }
    if (this.world.keyboard.LEFT) {
      // Nach links bewegen
    }
    // ...weitere Richtungen...
  }
}
```

**Zusammengefasst:**

- Die Klasse `Keyboard` speichert den Zustand der Tasten.
- Eventlistener setzen die Werte beim Drücken und Loslassen der Tasten.
- Die World-Klasse gibt die Referenz auf das Keyboard-Objekt an den Charakter
  weiter.
- Der Charakter kann so auf Tastatur-Eingaben reagieren und gesteuert werden.

---

## Charakter-Steuerung und Bild- Spiegelung

### Bewegung des Charakters

Die Bewegung des Charakters erfolgt in der Methode `animate()` der Klasse
`Character`. Hier wird geprüft, welche Taste gedrückt ist und entsprechend die
Position des Charakters angepasst:

```javascript
class Character extends MovableObject {
  otherDirection = false; // Standardmäßig false
  // ...existing code...
  animate() {
    setInterval(() => {
      if (
        this.world.keyboard.RIGHT &&
        this.x < this.world.level.levelLength - 720
      ) {
        this.x += this.speed;
        this.otherDirection = false;
      }
      if (this.world.keyboard.LEFT && this.x > 100) {
        this.x -= this.speed;
        this.otherDirection = true;
      }
      if (this.world.keyboard.UP && this.y > 0 - this.height / 2 + 10) {
        // canvas start - halbe characterHöhe + Transparenzen
        this.y -= this.speed;
      }
      if (this.world.keyboard.DOWN && this.y < 480 - this.height + 55) {
        // canvas end - characterHöhe + Transparenzen
        this.y += this.speed;
      }
      if (this.world.keyboard.SPACE) {
        this.speed = 4;
      } else {
        this.speed = 2;
      }
    }, 1000 / 60); // 60 fps
    // ...existing code...
  }
}
```

**Hinweis:**  
Die Eigenschaft `otherDirection` ist beim Character standardmäßig `false`. Sie
wird auf `true` gesetzt, wenn der Character nach links läuft, damit das Bild
beim Rendern gespiegelt wird.  
Zusätzlich sorgt die Bedingung `&& this.x > 100` dafür, dass der Charakter nicht
weiter nach links als bis zur x-Position 100 laufen kann. So bleibt er immer
sichtbar und verlässt das Spielfeld nicht nach links.  
Für die Begrenzung am rechten Rand des Levels wird geprüft, ob
`this.x < this.world.level.levelLength - 720` gilt. Die Zahl `720` entspricht
der Breite des Canvas. Dadurch kann der Charakter nicht weiter nach rechts
laufen, als das Level lang ist, und bleibt immer im sichtbaren Bereich des
Spielfelds.  
Die vertikalen Begrenzungen verhindern, dass der Charakter über den oberen oder
unteren Rand des Canvas hinaus bewegt werden kann. Für die obere Begrenzung wird
geprüft, ob `this.y > 0 - this.height / 2 + 10` gilt. Dadurch bleibt der
Charakter immer sichtbar und berücksichtigt auch eventuelle Transparenzen am
Bildrand. Für die untere Begrenzung wird geprüft, ob
`this.y < 480 - this.height + 55` gilt. Die Zahl `480` entspricht der Höhe des
Canvas, und der Wert `+ 55` sorgt dafür, dass der Charakter nicht zu weit nach
unten verschwindet und ebenfalls eventuelle Transparenzen berücksichtigt werden.

### Spiegeln des Charakters beim Rendern

Damit der Charakter beim Links-Laufen gespiegelt angezeigt wird, wird in der
Methode `addToMap()` der Klasse `World` das Canvas vorübergehend transformiert.
Dies passiert in mehreren Schritten:

1. **Zustand speichern:**  
   Mit `this.ctx.save()` wird der aktuelle Zustand des Canvas-Kontexts
   gespeichert. So können alle späteren Transformationen wieder rückgängig
   gemacht werden.

2. **Transformation anwenden:**  
   Mit `this.ctx.translate(object.width, 0)` wird der Ursprung des Canvas um die
   Breite des Objekts nach rechts verschoben.  
   Anschließend wird mit `this.ctx.scale(-1, 1)` die x-Achse gespiegelt. Dadurch
   werden alle nachfolgenden Zeichenoperationen horizontal gespiegelt
   dargestellt.

3. **Position invertieren:**  
   Die x-Position des Objekts wird mit `object.x = object.x * -1` invertiert,
   damit das Bild an der korrekten Stelle erscheint (da die Canvas nun
   gespiegelt ist).

4. **Bild zeichnen:**  
   Das Bild wird wie gewohnt mit `drawImage` gezeichnet, aber durch die
   Transformation erscheint es gespiegelt.

5. **Transformation rückgängig machen:**  
   Nach dem Zeichnen wird die x-Position wieder zurückgesetzt
   (`object.x = object.x * -1`) und mit `this.ctx.restore()` der ursprüngliche
   Canvas-Zustand wiederhergestellt. So sind weitere Zeichenoperationen nicht
   mehr gespiegelt.

Der relevante Code dazu:

```javascript
class World {
  // ...existing code...
  addToMap(object) {
    if (object.otherDirection) {
      this.ctx.save(); // 1. Zustand speichern
      this.ctx.translate(object.width, 0); // 2. Ursprung verschieben
      this.ctx.scale(-1, 1); // 2. Horizontal spiegeln
      object.x = object.x * -1; // 3. x-Position invertieren
    }
    this.ctx.drawImage(
      object.img,
      object.x,
      object.y,
      object.width,
      object.height
    ); // 4. Bild zeichnen
    if (object.otherDirection) {
      object.x = object.x * -1; // 5. x-Position zurücksetzen
      this.ctx.restore(); // 5. Transformation rückgängig machen
    }
  }
  // ...existing code...
}
```

**Zusammengefasst:**  
Durch das gezielte Anwenden und Rückgängigmachen von Canvas-Transformationen
wird das Bild des Charakters nur dann gespiegelt, wenn er nach links läuft. Alle
anderen Objekte werden wie gewohnt gezeichnet.

---

## Level-Management und Auslagerung der Leveldetails

Um die Verwaltung der Level zu vereinfachen und die Übersichtlichkeit zu
erhöhen, wurden alle Leveldetails aus der `World`-Klasse ausgelagert und in eine
eigene Klasse `Level` überführt. Die Leveldaten werden in separaten Dateien wie
`level1.js` definiert.

### Level-Klasse

Die Klasse `Level` kapselt alle relevanten Informationen eines Levels, wie
Gegner, Lichtstrahlen, Hintergrundobjekte und die Level-Länge. Die Variablen
werden oberhalb des Konstruktors deklariert:

```javascript
class Level {
  enemies; // Array mit Gegner-Objekten
  lightBeams; // Array mit Lichtstrahl-Objekten
  backgroundObjects; // Array mit Hintergrund-Objekten
  levelLength; // Länge des Levels in px

  constructor(enemies, lightBeams, backgroundObjects, levelLength) {
    this.enemies = enemies;
    this.lightBeams = lightBeams;
    this.backgroundObjects = backgroundObjects;
    this.levelLength = levelLength;
  }
}
```

### Definition eines Levels in level1.js

Die Leveldetails werden in einer eigenen Datei (z.B. `level1.js`) angelegt. Dort
werden die Gegner, Lichtstrahlen und Hintergrundobjekte für das jeweilige Level
erstellt und dem Level-Objekt übergeben. Die Level-Länge wird nach dem Rendern
des Hintergrunds gesetzt:

```javascript
let level1 = new Level(
  [new Fish(), new Fish(), new Fish()],
  [new LightBeam()],
  [],
  0 // levelLength initial 0
);

let backgroundImagesLevel = [
  "src/img/3. Background/Layers/5. Water/D1.png",
  "src/img/3. Background/Layers/4.Fondo 2/D1.png",
  "src/img/3. Background/Layers/3.Fondo 1/D1.png",
  "src/img/3. Background/Layers/2. Floor/D1.png",
  "src/img/3. Background/Layers/5. Water/D2.png",
  "src/img/3. Background/Layers/4.Fondo 2/D2.png",
  "src/img/3. Background/Layers/3.Fondo 1/D2.png",
  "src/img/3. Background/Layers/2. Floor/D2.png",
];

renderBackground(level1, 5, backgroundImagesLevel);

function renderBackground(level, backgroundRepeats, backgroundImages) {
  let repeats = backgroundRepeats;
  let insertPosition = 0;

  for (let i = 0; i < repeats; i++) {
    level.backgroundObjects.push(
      new BackgroundObject(backgroundImages[0], insertPosition + 0),
      new BackgroundObject(backgroundImages[1], insertPosition + 0),
      new BackgroundObject(backgroundImages[2], insertPosition + 0),
      new BackgroundObject(backgroundImages[3], insertPosition + 0),
      new BackgroundObject(backgroundImages[4], insertPosition + 720),
      new BackgroundObject(backgroundImages[5], insertPosition + 720),
      new BackgroundObject(backgroundImages[6], insertPosition + 720),
      new BackgroundObject(backgroundImages[7], insertPosition + 720)
    );
    insertPosition += 1440;
  }
  level.levelLength = insertPosition; // Level-Länge setzen
  console.log("Länge des Levels = " + insertPosition + "px");
}
```

Jetzt kann die Level-Länge überall im Code über `level1.levelLength` verwendet
werden.

### Verwendung in der World-Klasse

Die World-Klasse erhält nun ein Level-Objekt und verwendet dessen Daten für die
Darstellung und Logik der Spielwelt. Dadurch wird die World-Klasse deutlich
übersichtlicher und die Level können flexibel ausgetauscht oder erweitert
werden.

**Vorteile:**

- Trennung von Leveldaten und Spiellogik
- Einfaches Hinzufügen neuer Level durch separate Dateien
- Bessere Wartbarkeit und Übersichtlichkeit des Codes

### Verwendung eines Level-Objekts in der World-Klasse

Das Level-Objekt wird beim Erstellen der Spielwelt als Eigenschaft in der
World-Klasse gespeichert. Die World-Klasse nutzt dann die darin enthaltenen
Arrays für Gegner, Lichtstrahlen und Hintergrundobjekte über `this.level`:

```javascript
class World {
  // ...weitere Variablen...

  level = level1;

  // ...weitere Variablen...
}

 draw() {
  // ...existing code...
    this.addObjectsToMap(this.level.backgroundObjects);
    this.addObjectsToMap(this.level.lightBeams);
    this.addObjectsToMap(this.level.enemies);
    this.addToMap(this.character);
  // ...existing code...
  }

```

Die Hintergrundobjekte, Lichtstrahlen und Gegner werden also direkt über die
`level`-Eigenschaft angesprochen, z.B. `this.level.backgroundObjects`.  
Dadurch ist die World-Klasse flexibel und kann einfach mit verschiedenen
Level-Objekten arbeiten.

---

## Kameraverschiebung und Begrenzung des Charakters

### Wie verschiebe ich den sichtbaren Bereich des Canvas?

Um den sichtbaren Bereich des Canvas zu verschieben und so eine Kamerabewegung
zu simulieren, wird in der `World`-Klasse die Variable `camera_x` verwendet. In
der `draw()`-Methode wird der Canvas-Kontext vor dem Zeichnen aller Objekte mit
`ctx.translate(this.camera_x, 0)` verschoben. Der zweite Parameter (`0`) steht
für die Verschiebung auf der y-Achse – hier wird nur horizontal verschoben.

Nach dem Rendern aller Objekte muss der Viewport wieder zurückgesetzt werden,
damit die nächste Zeichenoperation wieder am ursprünglichen Ursprung startet.
Dies geschieht mit `this.ctx.translate(-this.camera_x, 0);` direkt nach dem
Zeichnen:

```javascript
class World {
  camera_x = 0;
  // ...existing code...

  draw() {
    this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
    this.ctx.translate(this.camera_x, 0);

    this.addObjectsToMap(this.level.backgroundObjects);
    this.addObjectsToMap(this.level.lightBeams);
    this.addObjectsToMap(this.level.enemies);
    this.addToMap(this.character);

    this.ctx.translate(-this.camera_x, 0);

    // ...existing code...
  }
}
```

### Wie steuere ich die Kameraverschiebung in der Character-Klasse?

Die Position der Kamera wird in der Bewegungslogik des Charakters angepasst. Die
Kamera folgt dem Charakter, solange er sich nicht ganz am linken oder rechten
Rand des Levels befindet. Die Logik dazu:

```javascript
class Character extends MovableObject {
  // ...existing code...
  animate() {
    setInterval(() => {
      // ...Bewegungslogik...
      if (this.x <= 100) {
        // Charakter ganz links, Kamera bleibt stehen
        this.world.camera_x = 0;
      } else if (this.x < this.world.level.levelLength - 720) {
        // Kamera folgt Charakter
        this.world.camera_x = -this.x + 100;
      } else {
        // Charakter ganz rechts, Kamera bleibt am rechten Rand stehen
        this.world.camera_x = -(this.world.level.levelLength - 720) + 100;
      }
    }, 1000 / 60);
    // ...existing code...
  }
}
```

**Erklärung der Logik und warum `this.x` negativ gesetzt wird:**

- **Links:** Wenn sich der Charakter ganz links befindet (`this.x <= 100`),
  bleibt die Kamera stehen. Das bedeutet, der sichtbare Bereich des Canvas wird
  nicht verschoben und der Charakter bleibt am linken Rand sichtbar.
- **Mitte:** Befindet sich der Charakter im mittleren Bereich des Levels, wird
  die Kamera so verschoben, dass der Charakter immer bei x=100 im Canvas bleibt.
  Das wird erreicht, indem `camera_x` auf `-this.x + 100` gesetzt wird. Das
  Minus sorgt dafür, dass die Welt nach links verschoben wird, wenn der
  Charakter nach rechts schwimmt. So bleibt der Charakter optisch an der
  gleichen Stelle im Canvas und die Umgebung bewegt sich.
- **Rechts:** Ist der Charakter ganz rechts, bleibt die Kamera am rechten Rand
  stehen. Der Wert `-(this.world.level.levelLength - 720) + 100` sorgt dafür,
  dass die Kamera nicht weiter nach rechts verschoben wird als das Level lang
  ist. Die Zahl `720` ist die Breite des Canvas.

**Warum negativ?**  
Das Canvas wird mit `ctx.translate(camera_x, 0)` verschoben. Ein negativer Wert
für `camera_x` verschiebt die gesamte Welt nach links, sodass der Charakter
optisch nach rechts schwimmt, aber im Canvas an der gleichen Stelle bleibt. Das
ist das Prinzip einer "Side-Scrolling"-Kamera: Der Charakter bleibt im Fokus,
während die Welt scrollt.

### Wie verhindere ich, dass der Charakter die Welt verlässt?

Die Bewegung des Charakters wird durch Bedingungen begrenzt, sodass er nicht aus
dem sichtbaren Bereich schwimmen kann:

```javascript
if (
  this.world.keyboard.RIGHT &&
  this.x < this.world.level.levelLength - 65 - this.width
) {
  this.x += this.speed;
  // ...existing code...
}
if (this.world.keyboard.LEFT && this.x > 0 - 40) {
  this.x -= this.speed;
  // ...existing code...
}
```

**Zusammengefasst:**

- Die Kamera wird mit `camera_x` verschoben, um den sichtbaren Bereich zu
  steuern.
- Die Begrenzungen sorgen dafür, dass der Charakter immer im sichtbaren Bereich
  bleibt und die Welt nicht verlassen kann.
- Die Logik für die Kameraverschiebung und Begrenzung befindet sich in der
  `draw()`-Methode der `World`-Klasse und in der Bewegungslogik der
  `Character`-Klasse.

  **Hinweis:**  
  Die Werte **65** und **40** berücksichtigen die transparenten Bereiche im Bild
  vom Charakter.

- **65** steht für den rechten transparenten Bereich des Bildes, damit Sharkie
  nicht mit unsichtbaren Bildteilen über den rechten Rand hinaus schwimmt
  (`this.world.level.levelLength - 65 - this.width`).
- **40** steht für den linken transparenten Bereich des Bildes, damit Sharkie
  nicht mit unsichtbaren Bildteilen über den linken Rand hinaus schwimmt
  (`0 - 40`).

Dadurch bleibt der sichtbare Teil des Charakters immer innerhalb des Spielfelds
und es entstehen keine unsichtbaren Überstände am Rand.

---

## Gravitation im Spiel

Mit der Einführung der Gravitation wird die Bewegung der Spielfigur
realistischer. Die Gravitation sorgt dafür, dass der Charakter nach oben und
unten nicht direkt per Tastendruck verschoben wird, sondern eine "Fallbewegung"
entsteht.

### Umsetzung der Gravitation

Die Gravitation wird in der Klasse `MovableObject` über die Methoden
`applyGravity()` und `isAboveGround()` gesteuert:

```javascript
applyGravity() {
  setInterval(() => {
    if (this.isAboveGround() || this.speedY > 0) {
      this.y -= this.speedY;
      this.speedY -= this.graphiteValue;
    }
  }, 1000 / this.graphiteSpeed);
  console.log(this.speedY);
}

isAboveGround() {
  return this.y < 480 - this.height + 55;
}
```

#### Erklärung der Funktionsweise

**applyGravity():**

- Diese Methode simuliert die Schwerkraft für das Objekt.
- Sie wird regelmäßig (60-mal pro Sekunde, abhängig von `graphiteSpeed`)
  aufgerufen.
- Solange das Objekt sich über dem Boden befindet (`isAboveGround()`) oder eine
  positive vertikale Geschwindigkeit (`speedY > 0`) hat, wird die y-Position des
  Objekts um `speedY` verringert (das Objekt bewegt sich nach oben).
- Anschließend wird `speedY` um den Wert von `graphiteValue` reduziert. Dadurch
  nimmt die Geschwindigkeit ab, bis das Objekt wieder nach unten fällt.
- Sobald das Objekt den Boden erreicht, stoppt die Bewegung nach unten.

**isAboveGround():**

- Diese Methode prüft, ob das Objekt sich noch über dem Boden befindet.
- Sie gibt `true` zurück, solange die y-Position kleiner als die berechnete
  Bodenposition ist (`480 - this.height + 55`).
- Erst wenn das Objekt den Boden erreicht hat, gibt die Methode `false` zurück
  und die Gravitation stoppt.

**Zusammengefasst:**  
Die Kombination aus beiden Methoden sorgt dafür, dass das Objekt nach einem
Sprung oder einer Bewegung nach oben wieder nach unten fällt und auf dem Boden
zum Stehen kommt. Die Werte für `speedY`, `graphiteValue` und `graphiteSpeed`
bestimmen dabei, wie schnell und wie stark die Gravitation wirkt.

#### Abhängigkeiten und deren Wirkung

- `speedY = 0;`  
  Startwert der vertikalen Geschwindigkeit. Wird durch Tastendruck (z.B. UP)
  erhöht.
- `graphiteValue = 0.15;`  
  Gibt an, wie stark die Geschwindigkeit pro Tick abnimmt ("Schwerkraft").
- `graphiteSpeed = 60;`  
  Bestimmt, wie oft pro Sekunde die Gravitation berechnet wird (Tickrate).

**Hinweis:**  
Je höher `graphiteValue`, desto schneller wird die Bewegung nach oben abgebremst
und der Charakter fällt wieder nach unten.  
`graphiteSpeed` bestimmt, wie "flüssig" die Gravitation wirkt – ein höherer Wert
sorgt für mehr Berechnungen pro Sekunde.

### Anpassung der Steuerung

Um die Gravitation zu nutzen, wurde die Steuerung für die UP- und DOWN-Taste in
der `Character`-Klasse angepasst:

- **UP:** Setzt `speedY` auf einen positiven Wert, sodass der Charakter nach
  oben "springt".
- **DOWN:** Setzt `speedY` auf 0 und verschiebt den Charakter direkt nach unten.

Dadurch entsteht eine natürliche Bewegung: Nach oben wird die Geschwindigkeit
durch die Gravitation abgebremst, nach unten fällt der Charakter, bis er den
Boden erreicht.

**Beispiel für die Steuerungslogik:**

```javascript
if (this.world.keyboard.UP) {
  this.speedY = this.speedDefault;
  if (this.y <= 0 - (this.height / 2 + 10)) {
    this.speedY = 0;
  }
}
if (this.world.keyboard.DOWN && this.y < 480 - this.height + 55) {
  this.y += this.speedX;
  this.speedY = 0;
}
```

**Zusammengefasst:**

- Die Klasse `Keyboard` speichert den Zustand der Tasten.
- Eventlistener setzen die Werte beim Drücken und Loslassen der Tasten.
- Die World-Klasse gibt die Referenz auf das Keyboard-Objekt an den Charakter
  weiter.
- Der Charakter kann so auf Tastatur-Eingaben reagieren und gesteuert werden.
- Die Gravitation sorgt für eine realistische Bewegung nach oben und unten.

---
