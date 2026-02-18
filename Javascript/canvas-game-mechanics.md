[Zurück zum Inhaltsverzeichnis](../README.md)

# Canvas-Spielmechaniken

## Inhaltsverzeichnis

1. [Canvas in der Spielmechanik](#canvas-in-der-spielmechanik)
2. [Arbeiten mit Bild-Animationen und Bild-Cache](#arbeiten-mit-bild-animationen-und-bild-cache)
3. [Hitboxen setzen und zeichnen](#hitboxen-setzen-und-zeichnen)
4. [Kollisionsabfrage zwischen Objekten](#kollisionsabfrage-zwischen-objekten)
5. [Kollisionsprüfung in der World-Klasse](#kollisionsprüfung-in-der-world-klasse)
6. [Steuerung mit Keyboard-Klasse](#steuerung-mit-keyboard-klasse)
7. [Charakter-Steuerung und Bild-Spiegelung](#charakter-steuerung-und-bild-spiegelung)
8. [Level-Management und Auslagerung der Leveldetails](#level-management-und-auslagerung-der-leveldetails)
9. [Kameraverschiebung und Begrenzung des Charakters](#kameraverschiebung-und-begrenzung-des-charakters)
10. [Gravitation im Spiel](#gravitation-im-spiel)
11. [Cooldown-Logik für Aktionen](#cooldown-logik-für-aktionen)
12. [Kollision zwischen Gegner und Blase erkennen](#kollision-zwischen-gegner-und-blase-erkennen)
13. [Audio-Management im Spiel](#audio-management-im-spiel)

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

### Variante 1: instanceof (eine Möglichkeit, aber problematisch)

Eine Möglichkeit ist, in der Methode `drawFrame(ctx)` mit `instanceof` zu
prüfen, ob das aktuelle Objekt z.B. ein `Character`, `Fish` oder `JellyFish`
ist:

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

**Hinweis:**  
Mit `instanceof` wird geprüft, ob das aktuelle Objekt eine Instanz einer
bestimmten Klasse ist.  
Das Problem: Diese Lösung kann zu Import-Schleifen und Fehlern bei komplexeren
Vererbungsstrukturen führen.

---

### Variante 2: Steuerung über `hasHitbox` (empfohlen)

Um Import-Schleifen und Typ-Probleme zu vermeiden, kann stattdessen jede Klasse,
die eine Hitbox benötigt, eine Property `hasHitbox` setzen.

- In der Basisklasse `DrawableObject` ist `hasHitbox = false` gesetzt.
- Nur Klassen, die eine Hitbox benötigen (z.B. `Fish`, `Character`,
  `JellyFish`), setzen explizit `hasHitbox = true`.
- Die Methode `drawFrame(ctx)` prüft nur noch diese Property:

```javascript
class MovableObject {
  // ...existing code...
  hasHitbox = false;

  drawFrame(ctx) {
    if (this.hasHitbox) {
      ctx.beginPath();
      ctx.lineWidth = "2";
      ctx.strokeStyle = "red";
      ctx.rect(this.rX, this.rY, this.rWidth, this.rHeight);
      ctx.stroke();
    }
  }
}
```

**Wie funktioniert das?**

- Beim Zeichnen eines Objekts ruft die World-Klasse immer
  `object.drawFrame(ctx)` auf.
- Nur wenn `hasHitbox` auf `true` steht, wird die Hitbox gezeichnet.
- Alle anderen Objekte (z.B. Hintergrund, Coins, etc.) haben `hasHitbox = false`
  und bekommen keine Hitbox angezeigt.

**Warum ist das besser?**

- **Keine Import-Schleifen:** Es gibt keine Abhängigkeiten zwischen den Klassen
  durch `instanceof`, was Import-Probleme verhindert.
- **Einfach erweiterbar:** Neue Klassen können einfach durch Setzen von
  `hasHitbox = true` eine Hitbox bekommen, ohne die Basisklasse oder die
  Zeichenlogik anpassen zu müssen.
- **Klar und wartbar:** Die Entscheidung, ob ein Objekt eine Hitbox hat, ist
  direkt in der jeweiligen Klasse sichtbar und leicht zu steuern.
- **Performance:** Die Prüfung auf eine einfache Property ist schneller und
  weniger fehleranfällig als komplexe Typ-Prüfungen.

**Fazit:**  
Die Verwendung der Property `hasHitbox` ist eine flexible, robuste und saubere
Lösung, um gezielt für bestimmte Objekte Hitboxen zu zeichnen, ohne die
Nachteile von `instanceof` und Import-Abhängigkeiten.

---

## Kollisionsabfrage zwischen Objekten

Um zu erkennen, ob sich zwei Objekte im Spiel berühren (z.B. Charakter und
Gegner), wird eine Kollisionsabfrage implementiert. Diese prüft, ob sich die
Rechtecke (Hitboxen) der Objekte überschneiden.

### Kollisionsmethode in MovableObject

In der Klasse `MovableObject` gibt es die Methode `isColliding(object)`, die
prüft, ob das aktuelle Objekt mit einem anderen kollidiert. Es gibt zwei
Varianten, wie diese Methode umgesetzt werden kann:

#### Variante 1: Mit getRealFrame und gespeicherten Hitbox-Werten

```javascript
isColliding(object) {
  this.getRealFrame();
  object.getRealFrame();
  return (
    this.rX + this.rWidth > object.rX &&
    this.rY + this.rHeight > object.rY &&
    this.rX < object.rX + object.rWidth &&
    this.rY < object.rY + object.rHeight
  );
}
```

**Hinweis:**  
Diese Methode setzt voraus, dass für beide Objekte (`this` und `object`) vor der
Kollisionserkennung `getRealFrame()` aufgerufen wird, damit die Werte `rX`,
`rY`, `rWidth`, `rHeight` aktuell sind. Wird dies vergessen, kann die
Kollisionserkennung fehlschlagen oder falsche Ergebnisse liefern.

#### Problematische Aspekte dieser Variante:

- Es ist leicht, das Aktualisieren der Hitboxen zu vergessen.
- Bei schnellen Bewegungen oder Spiegelungen können die gespeicherten Werte
  veraltet sein.
- Die Methode ist fehleranfälliger und schwerer zu warten.

#### Variante 2: Direkte Berechnung (empfohlen)

Eine robustere Alternative ist, die Hitboxen direkt in der Methode zu berechnen,
ohne auf gespeicherte Werte angewiesen zu sein:

```javascript
isColliding(other) {
  return (
    this.x + this.width - this.offset.right > other.x + other.offset.left &&
    this.x + this.offset.left < other.x + other.width - other.offset.right &&
    this.y + this.height - this.offset.bottom > other.y + other.offset.top &&
    this.y + this.offset.top < other.y + other.height - other.offset.bottom
  );
}
```

**Vorteile dieser Variante:**

- Die Berechnung ist immer aktuell, unabhängig von vorherigen Methodenaufrufen.
- Keine Gefahr von veralteten Hitbox-Werten.
- Die Methode ist einfacher, robuster und weniger fehleranfällig.

**Fazit:**  
Für eine zuverlässige Kollisionsabfrage empfiehlt es sich, die Hitboxen direkt
in der Methode zu berechnen und nicht auf zwischengespeicherte Werte zu setzen.
Das macht die Kollisionserkennung robuster und wartungsärmer.

---

### Kollisionsprüfung in der World-Klasse

Die Kollisionsabfrage wird regelmäßig in der `World`-Klasse durchgeführt. Dazu
gibt es die Methode `checkCollisions()`, die im Konstruktor der World-Klasse
aufgerufen wird:

```javascript
class World {
  // ...existing code...
  checkCollisions() {
    setInterval(() => {
      this.level.enemies.forEach((enemy) => {
        if (
          this.character.isColliding(enemy) &&
          !this.character.cooldownActive
        ) {
          this.character.hit();
          this.character.cooldown();
          this.statusBar.setPercentage(
            this.character.energy,
            ImageAssets.LIFE_BAR,
          );
          this.currentCharacterDamage = true;
        } else {
          this.currentCharacterDamage = false;
        }
      });

      this.level.objects.forEach((object, index) => {
        if (this.character.isColliding(object)) {
          if (object instanceof Coin) {
            this.level.objects.splice(index, 1);
            this.counterBar.coins.count++;
          }
          if (object instanceof Bubble) {
            this.level.objects.splice(index, 1);
            this.counterBar.bubbles.count++;
          }
        }
      });

      this.throwableObject.forEach((bubble, bubbleIndex) => {
        this.level.enemies.forEach((enemy, enemyIndex) => {
          if (bubble.isColliding(enemy) && !enemy.cooldownActive) {
            this.throwableObject.splice(bubbleIndex, 1);
            enemy.energy -= bubble.damage;
            enemy.cooldown();
            if (enemy.isDead()) {
              setTimeout(() => {
                this.level.enemies.splice(enemyIndex, 1);
              }, 500);
            }
          }
        });
      });
    }, 1000 / 60);
  }
  // ...existing code...
}
```

**Erklärung:**

- Es wird regelmäßig (z.B. 60x pro Sekunde) geprüft, ob der Charakter mit
  Gegnern, Münzen oder Blasen kollidiert.
- Bei einer Kollision mit einem Gegner wird Energie abgezogen und ein Cooldown
  aktiviert.
- Bei einer Kollision mit Münzen oder Blasen werden diese entfernt und der
  Zähler erhöht.
- Bei einer Kollision von geworfenen Blasen mit Gegnern wird der Gegner verletzt
  und ggf. entfernt.

**Zusammengefasst:**

- Die Methode `isColliding()` prüft die Überschneidung zweier Objekte.
- Die Methode `checkCollisions()` in der World-Klasse sorgt dafür, dass alle
  relevanten Kollisionen im Spiel korrekt erkannt und verarbeitet werden.
- Die Kombination aus robuster Kollisionsmethode und zentraler
  Kollisionserkennung ist essenziell für die Spiellogik.

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

## Charakter-Steuerung und Bild-Spiegelung

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
      object.height,
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
  0, // levelLength initial 0
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
      new BackgroundObject(backgroundImages[7], insertPosition + 720),
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

## Cooldown-Logik für Aktionen

### Was ist ein Cooldown?

Ein **Cooldown** ist eine Wartezeit, die nach einer bestimmten Aktion abläuft,
bevor diese Aktion erneut ausgeführt werden kann. In Spielen wird dies häufig
verwendet, um z.B. das wiederholte Schießen, Angreifen oder andere Aktionen zu
begrenzen. Während des Cooldowns ist die Aktion gesperrt und kann erst nach
Ablauf der Zeit erneut ausgelöst werden.

### Beispiel: Cooldown beim Schießen einer Blase

Im Spiel kann der Charakter (z.B. Sharkie) Blasen verschießen. Damit nicht
unendlich viele Blasen pro Sekunde abgeschossen werden können, wird eine
Cooldown-Logik implementiert.

#### Umsetzung im Code

1. **Status-Variable:**  
   Im Character gibt es eine Property wie `shotKeyPressed` (boolean), die
   speichert, ob die Schusstaste gerade gedrückt wurde und ob der Cooldown aktiv
   ist.

2. **Aktion auslösen:**  
   Wenn die Schusstaste (z.B. `H`) gedrückt wird und der Cooldown nicht aktiv
   ist, wird die Aktion (Blase schießen) ausgeführt und der Cooldown aktiviert.

3. **Cooldown aktivieren:**  
   Nach dem Auslösen der Aktion wird `shotKeyPressed` auf `true` gesetzt. Erst
   wenn die Taste wieder losgelassen wird (`H` nicht mehr gedrückt), wird
   `shotKeyPressed` wieder auf `false` gesetzt und die Aktion kann erneut
   ausgelöst werden.

#### Beispielhafter Ablauf

```javascript
if (this.world.keyboard.H && !this.shotKeyPressed) {
  if (this.world.counterBar.bubbles.count > 0) {
    this.shotKeyPressed = true; // Cooldown aktivieren
    this.shot(); // Aktion ausführen
    this.world.counterBar.bubbles.count -= 1;
  }
}
if (!this.world.keyboard.H) {
  this.shotKeyPressed = false; // Cooldown zurücksetzen, wenn Taste losgelassen
}
```

**Erklärung:**

- **Taste gedrückt & kein Cooldown:**  
  Die Aktion (Blase schießen) wird nur ausgeführt, wenn die Taste gedrückt ist
  und `shotKeyPressed` noch `false` ist.
- **Cooldown aktiv:**  
  Nach dem Schuss wird `shotKeyPressed` auf `true` gesetzt. Solange die Taste
  gehalten wird, kann kein weiterer Schuss ausgelöst werden.
- **Cooldown zurücksetzen:**  
  Erst wenn die Taste losgelassen wird (`!this.world.keyboard.H`), wird
  `shotKeyPressed` wieder auf `false` gesetzt. Jetzt kann beim nächsten
  Tastendruck wieder geschossen werden.

#### Vorteile dieser Logik

- **Verhindert Dauerfeuer:**  
  Es kann immer nur eine Blase pro Tastendruck geschossen werden, nicht mehrere
  pro Sekunde durch Halten der Taste.
- **Einfache Steuerung:**  
  Die Logik ist leicht verständlich und benötigt nur eine zusätzliche Property.
- **Flexibel erweiterbar:**  
  Die Cooldown-Logik kann für beliebige andere Aktionen (z.B. Angriffe,
  Spezialfähigkeiten) übernommen werden.

#### Erweiterung: Zeitbasierter Cooldown

Alternativ kann ein zeitbasierter Cooldown verwendet werden, bei dem nach jeder
Aktion eine bestimmte Zeit abgewartet werden muss, bevor die Aktion erneut
möglich ist. Dies kann z.B. mit `setTimeout` umgesetzt werden:

```javascript
if (this.canShoot) {
  this.shot();
  this.canShoot = false;
  setTimeout(() => {
    this.canShoot = true;
  }, 500); // 500ms Cooldown
}
```

**Hier:**  
Nach jedem Schuss wird `canShoot` für 500 Millisekunden auf `false` gesetzt.
Erst danach kann wieder geschossen werden.

---

**Zusammengefasst:**  
Die Cooldown-Logik sorgt dafür, dass Aktionen wie das Schießen von Blasen nicht
beliebig oft pro Sekunde ausgeführt werden können. Sie wird über eine
Status-Variable gesteuert, die beim Auslösen der Aktion gesetzt und beim
Loslassen der Taste oder nach Ablauf einer Zeitspanne zurückgesetzt wird. So
bleibt das Spiel balanciert und die Steuerung nachvollziehbar.

---

## Kollision zwischen Gegner und Blase erkennen

Um zu erkennen, ob eine geworfene Blase einen Gegner trifft, wird eine
Kollisionserkennung zwischen den jeweiligen Objekten durchgeführt. Das Vorgehen
ist wie folgt:

### 1. Kollisionserkennungsmethode

Sowohl Gegner als auch Blasen sind Instanzen von Klassen, die von
`MovableObject` erben. In dieser Basisklasse gibt es die Methode
`isColliding(object)`, die prüft, ob sich die Rechtecke (Hitboxen) zweier
Objekte überschneiden:

```javascript
isColliding(object) {
  return (
    this.x + this.width > object.x &&
    this.y + this.height > object.y &&
    this.x < object.x + object.width &&
    this.y < object.y + object.height
  );
}
```

- Diese Methode wird sowohl für Gegner als auch für Blasen verwendet.
- Sie gibt `true` zurück, wenn sich die beiden Objekte überlappen.

### 2. Überprüfung in der Spielwelt

In der `World`-Klasse gibt es ein Array `throwableObject` (für Blasen) und ein
Array `enemies` (für Gegner). In der Methode `checkCollisions()` wird regelmäßig
geprüft, ob eine Blase einen Gegner trifft:

```javascript
this.throwableObject.forEach((bubble, bubbleIndex) => {
  this.level.enemies.forEach((enemy, enemyIndex) => {
    if (bubble.isColliding(enemy) && !enemy.cooldownActive) {
      // 1. Blase trifft Gegner
      this.throwableObject.splice(bubbleIndex, 1); // Blase entfernen
      enemy.energy -= bubble.damage; // Schaden zufügen
      enemy.cooldown(); // Gegner kurzzeitig unverwundbar machen
      if (enemy.isDead()) {
        setTimeout(() => {
          this.level.enemies.splice(enemyIndex, 1); // Gegner entfernen
        }, 500);
      }
    }
  });
});
```

**Ablauf im Detail:**

1. Jede Blase wird mit jedem Gegner auf Kollision geprüft
   (`bubble.isColliding(enemy)`).
2. Wird eine Kollision erkannt und der Gegner ist nicht im Cooldown, passiert
   Folgendes:
   - Die Blase wird aus dem Array entfernt (`splice`).
   - Der Gegner erhält Schaden (`enemy.energy -= bubble.damage`).
   - Der Gegner wird für kurze Zeit unverwundbar (`enemy.cooldown()`).
   - Falls der Gegner keine Energie mehr hat (`enemy.isDead()`), wird er nach
     kurzer Zeit entfernt.

### 3. Zusammenfassung

- Die Methode `isColliding()` prüft die Überschneidung der Hitboxen.
- In der Spielwelt werden alle Blasen mit allen Gegnern verglichen.
- Bei Kollision wird die Blase entfernt und der Gegner erhält Schaden.
- Gegner können durch mehrere Blasen besiegt werden.

**Vorteil:**  
Diese Logik ist flexibel und kann für beliebige andere Kollisionen (z.B.
Charakter und Gegner, Charakter und Münzen) wiederverwendet werden.

---

## Audio-Management im Spiel

### Zentrale Verwaltung mit AudioHub

Für die Steuerung und Verwaltung aller Audioeffekte im Spiel empfiehlt sich eine
zentrale Klasse wie `AudioHub`. Diese Klasse kapselt alle Audioelemente (z.B.
Hintergrundmusik, Soundeffekte) und stellt Methoden zum Abspielen und Stoppen
bereit.

**Beispiel: audio-hub.class.js**

```javascript
export class AudioHub {
  static backgroundMusic = new Audio("src/audio/background.mp3");
  static bubbleSound = new Audio("src/audio/bubble.mp3");
  static click = new Audio("src/audio/click.mp3");
  static hurt = new Audio("src/audio/hurt.mp3");
  static collect = new Audio("src/audio/collect.mp3");
  static blubbCollect = new Audio("src/audio/blubb_collect.mp3");
  static dangerous = new Audio("src/audio/dangerous.mp3");
  static playSounds = false;

  static backgroundSound(sound) {
    if (AudioHub.playSounds) {
      sound.volume = 0.25;
      sound.currentTime = 0;
      sound.play();
    }
  }

  static hoverSound(sound) {
    if (AudioHub.playSounds) {
      sound.volume = 0.5;
      sound.currentTime = 0.06;
      sound.play();
    }
  }

  static hurtSound(sound) {
    if (AudioHub.playSounds) {
      sound.volume = 0.4;
      sound.currentTime = 0.3;
      sound.play();
    }
  }

  static attackSound(sound) {
    if (AudioHub.playSounds) {
      sound.volume = 0.4;
      sound.currentTime = 0;
      sound.play();
    }
  }

  static collectSound(sound) {
    if (AudioHub.playSounds) {
      sound.volume = 0.2;
      sound.currentTime = 0;
      sound.play();
    }
  }

  static stop(sound) {
    sound.pause();
  }
}
```

**Vorteile:**

- Alle Audioelemente sind zentral verwaltet.
- Die Lautstärke und Startzeit können für jeden Sound individuell gesetzt
  werden.
- Über die Property `playSounds` kann Audio global aktiviert/deaktiviert werden.

---

### Einbindung und Steuerung von Audio im Spiel

**1. Audio abspielen bei bestimmten Aktionen**

Beim Eintreten bestimmter Spielereignisse (z.B. Kollision, Sammeln von Items,
Klicks) wird der passende Sound über die Methoden von `AudioHub` abgespielt:

```javascript
// Beispiel: Sound beim Einsammeln eines Items
AudioHub.collectSound(AudioHub.collect);

// Beispiel: Sound beim Schaden nehmen
AudioHub.hurtSound(AudioHub.hurt);

// Beispiel: Hintergrundmusik starten
AudioHub.backgroundSound(AudioHub.backgroundMusic);
```

**2. Audio global aktivieren/deaktivieren**

Die Property `AudioHub.playSounds` kann z.B. über einen Button oder eine
Einstellung im Spiel gesetzt werden, um alle Sounds stummzuschalten oder zu
aktivieren:

```javascript
// Sounds aktivieren
AudioHub.playSounds = true;

// Sounds deaktivieren
AudioHub.playSounds = false;
```

---

### Beispiel: AudioHub – Mehrere Sounds und Visualisierung

In anderen Projekten wie SoundHub werden mehrere Sounds parallel verwaltet und
mit Visualisierung kombiniert. Hier ein angepasstes Beispiel, das die Benennung
wie im Sharkie-Projekt verwendet:

```javascript
export class AudioHub {
  static backgroundMusic = new Audio("./assets/sounds/background.mp3");
  static hurt = new Audio("./assets/sounds/hurt.mp3");
  static collect = new Audio("./assets/sounds/collect.mp3");
  static bubble = new Audio("./assets/sounds/bubble.mp3");
  static click = new Audio("./assets/sounds/click.mp3");
  static dangerous = new Audio("./assets/sounds/dangerous.mp3");
  static allSounds = [
    AudioHub.backgroundMusic,
    AudioHub.hurt,
    AudioHub.collect,
    AudioHub.bubble,
    AudioHub.click,
    AudioHub.dangerous,
  ];

  static playOne(sound, soundId) {
    sound.volume = 0.2;
    sound.currentTime = 0;
    sound.play();
    // Optional: Visualisierung, falls ein Element mit soundId existiert
    const soundImg = document.getElementById(soundId);
    if (soundImg) soundImg.classList.add("active");
  }

  static stopAll() {
    AudioHub.allSounds.forEach((sound) => {
      sound.pause();
    });
    document.getElementById("volume").value = 0.2;
    // Optional: Visualisierung zurücksetzen
    const soundImages = document.querySelectorAll(".sound_img");
    soundImages.forEach((img) => img.classList.remove("active"));
  }

  static stopOne(sound, soundId) {
    sound.pause();
    // Optional: Visualisierung zurücksetzen
    const soundImg = document.getElementById(soundId);
    if (soundImg) soundImg.classList.remove("active");
  }

  static objSetVolume(volumeSliderID) {
    let currentVolumeValue = document.getElementById(volumeSliderID).value;
    AudioHub.allSounds.forEach((sound) => {
      sound.volume = currentVolumeValue;
    });
  }
}
```

---

### Lautstärkeregelung per Slider im Optionsmenü

In modernen Spielen wird die Lautstärke häufig über einen Slider im Optionsmenü
angepasst. Die zentrale AudioHub-Klasse kann dies unterstützen, indem sie eine
Methode bereitstellt, die die Lautstärke aller Sounds entsprechend dem Wert
eines Sliders setzt.

**Beispiel: Lautstärke-Slider im Optionsmenü**

```html
<!-- Beispiel für einen Lautstärke-Slider im Optionsmenü -->
<label for="volumeSlider">Lautstärke:</label>
<input
  type="range"
  id="volumeSlider"
  min="0"
  max="1"
  step="0.01"
  value="0.2"
  onchange="AudioHub.objSetVolume('volumeSlider')"
/>
```

Die Methode in der AudioHub-Klasse könnte so aussehen:

```javascript
// ...existing code...
```

**Typischer Ablauf im Spiel:**

1. Im Optionsmenü wird der Slider angezeigt.
2. Der Spieler verschiebt den Slider, wodurch das `onchange`-Event ausgelöst
   wird.
3. Die Methode `AudioHub.objSetVolume('volumeSlider')` wird aufgerufen und setzt
   die Lautstärke aller Sounds entsprechend dem aktuellen Wert des Sliders.

**Vorteile:**

- Die Lautstärke kann jederzeit im Spiel angepasst werden, ohne dass Sounds
  neugestartet werden müssen.
- Die Einstellung kann gespeichert und beim nächsten Spielstart
  wiederhergestellt werden.

**Hinweis:**  
Für ein vollständiges Optionsmenü empfiehlt es sich, die aktuelle Lautstärke
z.B. im `localStorage` zu speichern und beim Laden des Spiels wieder zu setzen.

**Beispiel für Speicherung:**

```javascript
// Beim Ändern des Sliders
function onVolumeChange() {
  let value = document.getElementById("volumeSlider").value;
  localStorage.setItem("gameVolume", value);
  AudioHub.objSetVolume("volumeSlider");
}

// Beim Laden des Spiels
window.onload = function () {
  let savedVolume = localStorage.getItem("gameVolume") || 0.2;
  document.getElementById("volumeSlider").value = savedVolume;
  AudioHub.objSetVolume("volumeSlider");
};
```

---

### Fehlerbehandlung beim Audio-Playback

Gerade beim schnellen Starten und Stoppen von Sounds kann es zu Fehlern kommen,
wenn ein Audio-Element noch nicht vollständig geladen ist. Daher empfiehlt sich
eine Prüfung des Ladezustands (`readyState`):

```javascript
class AudioError {
  static LONG = new Audio("./assets/sounds/binary.mp3");

  static playOne(sound) {
    setInterval(() => {
      if (sound.readyState == 4) {
        sound.volume = 0.5;
        sound.play();
      }
    }, 200);
  }

  static stopOne(sound) {
    sound.pause();
  }
}
```

#### Was bedeutet `readyState` bei Audio?

Das Property `readyState` eines Audio-Elements zeigt an, wie weit die Audiodatei
geladen ist.  
Je nach Wert kann das Audio unterschiedlich verwendet werden:

| Wert | Name              | Bedeutung                                                                                                  |
| ---- | ----------------- | ---------------------------------------------------------------------------------------------------------- |
| 0    | HAVE_NOTHING      | Es sind keine Informationen über das Audio geladen. Kein Audio verfügbar.                                  |
| 1    | HAVE_METADATA     | Nur die Metadaten (z.B. Dauer, Format) sind geladen. Kein Audio abspielbar.                                |
| 2    | HAVE_CURRENT_DATA | Es sind Daten für die aktuelle Wiedergabeposition verfügbar, aber nicht unbedingt für die nächste Sekunde. |
| 3    | HAVE_FUTURE_DATA  | Es sind Daten für die aktuelle und die nächste Wiedergabeposition verfügbar.                               |
| 4    | HAVE_ENOUGH_DATA  | Genug Daten sind geladen, um das Audio ohne Unterbrechung abzuspielen.                                     |

**Praxis:**

- Erst bei `readyState == 4` (`HAVE_ENOUGH_DATA`) kann das Audio garantiert ohne
  Fehler abgespielt werden.
- Die Methode im Beispiel prüft deshalb auf `readyState == 4`, bevor sie das
  Audio startet.

**Tipp:**  
Wenn du ein Audio sehr früh abspielen willst, kann es sinnvoll sein, auf einen
niedrigeren Wert zu prüfen (z.B. `>= 2`), aber dann kann es zu Unterbrechungen
kommen, falls noch nicht alles geladen ist.

**Beispiel für alle Zustände:**

```javascript
switch (sound.readyState) {
  case 0:
    console.log("Noch nichts geladen.");
    break;
  case 1:
    console.log("Metadaten geladen.");
    break;
  case 2:
    console.log("Aktuelle Daten verfügbar.");
    break;
  case 3:
    console.log("Daten für die nächste Wiedergabeposition verfügbar.");
    break;
  case 4:
    console.log("Genug Daten für unterbrechungsfreies Abspielen.");
    break;
}
```

**Zusammengefasst:**  
Die Prüfung von `readyState` hilft, Fehler beim Audio-Playback zu vermeiden und
sorgt für ein besseres Nutzererlebnis – besonders bei großen oder gestreamten
Audiodateien.
