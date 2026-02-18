[Zurück zur Übersicht](../README.md)

# Lifecycle-Hooks in Angular

---

## Was sind Lifecycle-Hooks?

Lifecycle-Hooks sind spezielle Methoden, die Angular zu bestimmten Zeitpunkten
im Lebenszyklus einer Komponente oder Directive automatisch aufruft.  
Sie ermöglichen dir, auf Ereignisse wie Initialisierung, Änderungen, Rendern
oder Zerstörung zu reagieren und gezielt Code auszuführen.

### Der Lebenszyklus einer Komponente

Jede Angular-Komponente durchläuft einen klar definierten Lebenszyklus:

```
Erstellt (new) → Inputs gesetzt → Initialisiert → Änderungen erkannt →
View aufgebaut → Änderungen im View erkannt → Zerstört
```

Angular verwaltet diesen Zyklus vollständig selbst. Du als Entwickler kannst
über Lifecycle-Hook-Methoden in diese Phasen eingreifen.

**Wie lange lebt eine Komponente?**

- Eine Komponente **entsteht**, wenn Angular sie beim Rendern erstellt (z.B.
  wenn sie im Template auftaucht oder eine Route aktiviert wird).
- Eine Komponente **lebt**, solange sie im DOM sichtbar/aktiv ist.
- Eine Komponente **wird zerstört**, wenn sie aus dem DOM entfernt wird (z.B.
  bei `*ngIf = false`, Routenwechsel oder manueller Zerstörung).

> Beim Zerstören der Komponente ruft Angular `ngOnDestroy()` auf – hier solltest
> du Subscriptions beenden, Timer löschen und Ressourcen freigeben.

---

## Inhaltsverzeichnis

1. [Was sind Lifecycle-Hooks?](#was-sind-lifecycle-hooks)
2. [Übersicht Lifecycle-Hooks](#übersicht-lifecycle-hooks)
3. [Reihenfolge der Hooks](#reihenfolge-der-hooks)
4. [OnInit – Initialisierung von Komponenten und Directives](#oninit--initialisierung-von-komponenten-und-directives)
5. [OnChanges – Auf Eingabeänderungen reagieren](#onchanges--auf-eingabeänderungen-reagieren)
6. [DoCheck – Eigene Änderungserkennung](#docheck--eigene-änderungserkennung)
7. [AfterContentInit – Nach dem Projizieren von Inhalten](#aftercontentinit--nach-dem-projizieren-von-inhalten)
8. [AfterContentChecked – Nach jeder Content-Prüfung](#aftercontentchecked--nach-jeder-content-prüfung)
9. [AfterViewInit – Zugriff auf das View-Template nach der Initialisierung](#afterviewinit--zugriff-auf-das-view-template-nach-der-initialisierung)
10. [AfterViewChecked – Nach jeder View-Prüfung](#afterviewchecked--nach-jeder-view-prüfung)
11. [OnDestroy – Aufräumen beim Zerstören](#ondestroy--aufräumen-beim-zerstören)

---

## Übersicht Lifecycle-Hooks

| Hook                    | Wann wird er aufgerufen?                                       | Häufigkeit | Typische Anwendungsfälle             |
| ----------------------- | -------------------------------------------------------------- | ---------- | ------------------------------------ |
| `ngOnChanges`           | Wenn sich ein `@Input`-Wert ändert (auch vor `ngOnInit`)       | Mehrfach   | Auf Input-Änderungen reagieren       |
| `ngOnInit`              | Nach der Konstruktion und dem Setzen aller Inputs (einmalig)   | Einmalig   | Initialisierung, Daten laden         |
| `ngDoCheck`             | Bei jeder Änderungserkennung durch Angular                     | Sehr oft   | Eigene Änderungserkennung            |
| `ngAfterContentInit`    | Nachdem projizierter Inhalt (`ng-content`) initialisiert wurde | Einmalig   | Zugriff auf `@ContentChild`          |
| `ngAfterContentChecked` | Nach jeder Prüfung des projizierten Inhalts                    | Sehr oft   | Reaktion auf Content-Änderungen      |
| `ngAfterViewInit`       | Nachdem das View-Template und alle Child-Views aufgebaut sind  | Einmalig   | DOM-Zugriff, Animationen, Plugins    |
| `ngAfterViewChecked`    | Nach jeder Prüfung des View-Templates                          | Sehr oft   | Reaktion auf View-Änderungen         |
| `ngOnDestroy`           | Kurz bevor die Komponente/Directive zerstört wird              | Einmalig   | Subscriptions beenden, Timer löschen |

---

## Reihenfolge der Hooks

Angular ruft die Hooks in dieser festen Reihenfolge auf:

```
1. ngOnChanges           ← (nur wenn @Input vorhanden, vor ngOnInit)
2. ngOnInit              ← einmalig
3. ngDoCheck             ← bei jeder Change Detection
4. ngAfterContentInit    ← einmalig
5. ngAfterContentChecked ← bei jeder Change Detection
6. ngAfterViewInit       ← einmalig
7. ngAfterViewChecked    ← bei jeder Change Detection
8. ngOnDestroy           ← beim Zerstören
```

> `ngOnChanges` wird als einziger Hook **vor** `ngOnInit` aufgerufen – und dann
> jedes Mal erneut, wenn sich ein `@Input`-Wert ändert.

---

## Die zwei Phasen des Lifecycles

Der gesamte Lifecycle lässt sich in **zwei Phasen** unterteilen:

### Phase 1 – „First-Time Hooks" (einmalig beim Erstellen)

Diese Hooks werden **genau einmal** ausgeführt, wenn die Komponente zum ersten
Mal erstellt wird:

```
1. ngOnChanges           ← (nur wenn @Input vorhanden)
2. ngOnInit
3. ngDoCheck
4. ngAfterContentInit
5. ngAfterContentChecked
6. ngAfterViewInit
7. ngAfterViewChecked
```

### Phase 2 – „Change Detection Hooks" (bei jeder Änderung)

Diese Hooks werden **bei jeder Change-Detection-Runde** erneut ausgeführt – also
z.B. bei jedem Klick, Timer-Tick oder HTTP-Response:

```
1. ngOnChanges           ← (nur wenn sich ein @Input geändert hat)
2. ngDoCheck
3. ngAfterContentChecked
4. ngAfterViewChecked
```

> **Merkhilfe:** Die „einmaligen" Hooks (`ngOnInit`, `ngAfterContentInit`,
> `ngAfterViewInit`) laufen nur in Phase 1. Alles andere wiederholt sich mit
> jeder Change-Detection-Runde.

---

# OnInit – Initialisierung von Komponenten und Directives

---

## Was ist OnInit?

`OnInit` ist ein Lifecycle-Interface in Angular.  
Es ermöglicht dir, Code auszuführen, sobald eine Komponente oder Directive
fertig initialisiert ist – also nachdem alle Eingabewerte (`@Input`) gesetzt
wurden.

---

## Wann verwendet man OnInit?

Du verwendest `OnInit`, wenn du Initialisierungscode ausführen möchtest, der auf
bereits gesetzte Eingabewerte (`@Input`) oder injizierte Abhängigkeiten
zugreifen muss. Typische Anwendungsfälle sind:

- Wenn du Daten laden oder asynchrone Operationen beim Start der Komponente
  ausführen willst.
- Wenn du Werte berechnen oder initialisieren möchtest, die von Inputs abhängen.
- Wenn du DOM-Manipulationen oder Initialisierungen in einer Directive
  durchführen willst.
- Immer dann, wenn der Konstruktor nicht ausreicht, weil dort noch keine Inputs
  gesetzt sind.

---

## Vorteile von OnInit

- **Klarer Startpunkt:** Initialisierungslogik ist sauber getrennt vom
  Konstruktor.
- **Sicherer Zugriff:** Alle Inputs und Abhängigkeiten sind gesetzt.
- **Strukturierter Code:** Bessere Übersicht und Wartbarkeit.

---

## Schritt-für-Schritt: OnInit verwenden

### 1. Interface implementieren

```typescript
import { Component, OnInit } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<p>Hallo Welt!</p>`,
  standalone: true,
})
export class ExampleComponent implements OnInit {
  ngOnInit() {
    // Initialisierungscode hier
    console.log("Komponente wurde initialisiert!");
  }
}
```

**Erklärung:**

- Mit `implements OnInit` verpflichtest du dich, die Methode `ngOnInit()` zu
  schreiben.
- Angular ruft `ngOnInit()` automatisch nach der Konstruktion und dem Setzen
  aller Inputs auf.

---

### 2. In einer Directive verwenden

```typescript
import { Directive, ElementRef, OnInit, inject } from "@angular/core";

@Directive({
  selector: "[appHighlight]",
  standalone: true,
})
export class HighlightDirective implements OnInit {
  private el = inject(ElementRef);

  ngOnInit() {
    this.el.nativeElement.style.backgroundColor = "yellow";
  }
}
```

---

## Best Practices

- **Konstruktor nur für DI:** Im Konstruktor nur Abhängigkeiten injizieren,
  keine Logik ausführen.
- **Initialisierung in ngOnInit:** Lade Daten, setze Werte oder führe Logik in
  `ngOnInit()` aus.
- **Kein DOM-Zugriff:** Das View-Template ist in `ngOnInit` noch nicht gerendert
  – nutze dafür `ngAfterViewInit`.

---

# OnChanges – Auf Eingabeänderungen reagieren

---

## Was ist OnChanges?

`OnChanges` ist ein Lifecycle-Interface in Angular.  
Es ermöglicht dir, auf Änderungen von `@Input`-Properties zu reagieren – sowohl
beim ersten Setzen als auch bei jeder späteren Änderung durch die
Parent-Komponente.

> **Wichtig:** `ngOnChanges` reagiert nur auf Änderungen der **Referenz** eines
> Inputs. Mutationen innerhalb eines Objekts oder Arrays (z.B. `array.push()`)
> werden **nicht** erkannt.

---

## Wann verwendet man OnChanges?

- Wenn du auf Änderungen von `@Input`-Werten reagieren möchtest.
- Wenn du berechnete Werte aktualisieren willst, sobald sich ein Input ändert.
- Wenn du den alten und neuen Wert eines Inputs vergleichen möchtest.

---

## Schritt-für-Schritt: OnChanges verwenden

```typescript
import { Component, Input, OnChanges, SimpleChanges } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<p>{{ title }}</p>`,
  standalone: true,
})
export class ExampleComponent implements OnChanges {
  @Input() title: string = "";

  ngOnChanges(changes: SimpleChanges) {
    if (changes["title"]) {
      const previousValue = changes["title"].previousValue;
      const currentValue = changes["title"].currentValue;
      const isFirst = changes["title"].firstChange;

      console.log(`title geändert: ${previousValue} → ${currentValue}`);
      console.log("Erste Änderung?", isFirst);
    }
  }
}
```

**Erklärung:**

- `SimpleChanges` ist ein Objekt, das für jeden geänderten Input ein
  `SimpleChange`-Objekt enthält.
- `previousValue` – der alte Wert
- `currentValue` – der neue Wert
- `firstChange` – `true`, wenn es die erste Änderung (Initialisierung) ist

---

## Best Practices

- **Vermeide schwere Logik:** `ngOnChanges` kann oft aufgerufen werden – halte
  ihn performant.
- **Nutze `firstChange`:** Unterscheide zwischen erster Initialisierung und
  späteren Änderungen.
- **Referenz statt Mutation:** Übergib neue Objekt-/Array-Referenzen aus der
  Parent-Komponente, damit `ngOnChanges` zuverlässig feuert.

---

# DoCheck – Eigene Änderungserkennung

---

## Was ist DoCheck?

`DoCheck` ermöglicht dir, eigene Änderungserkennung zu implementieren.  
Angular ruft `ngDoCheck()` bei **jeder Change-Detection-Runde** auf – das kann
sehr häufig sein (z.B. bei jedem Klick, jedem Timer oder jeder HTTP-Antwort).

`ngDoCheck` wird **nach** `ngOnChanges` und `ngOnInit` aufgerufen und ist
besonders nützlich, um Änderungen zu erkennen, die Angular mit seiner
Standard-Erkennung nicht erfasst – etwa direkte Mutationen von Objekten oder
Arrays.

---

## Wann verwendet man DoCheck?

- Wenn du Mutationen in Arrays oder Objekten erkennen willst, ohne neue
  Referenzen zu erzeugen.
- Bei der `OnPush`-Change-Detection-Strategie, um manuelle Prüfungen
  durchzuführen.
- Als letzte Option, wenn `ngOnChanges` nicht ausreicht.

> ⚠️ **Achtung:** `ngDoCheck` wird extrem häufig aufgerufen. Nutze ihn nur, wenn
> du weißt, was du tust, und vermeide darin aufwändige Operationen.

---

## Schritt-für-Schritt: DoCheck verwenden

```typescript
import { Component, DoCheck, Input } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<p>{{ items.length }} Einträge</p>`,
  standalone: true,
})
export class ExampleComponent implements DoCheck {
  @Input() items: string[] = [];
  private lastLength = 0;

  ngDoCheck() {
    // Erkennt Mutationen im Array, die ngOnChanges nicht sieht
    if (this.items.length !== this.lastLength) {
      console.log("Array-Länge hat sich geändert!");
      this.lastLength = this.items.length;
    }
  }
}
```

---

## Best Practices

- **So selten wie möglich verwenden:** Bevorzuge `ngOnChanges` oder neue
  Referenzen.
- **Nur leichte Operationen:** Vergleiche, keine HTTP-Calls oder DOM-Zugriffe.
- **Kombination vermeiden:** Implementiere nicht gleichzeitig `DoCheck` und
  `OnChanges` – das führt zu unerwartetem Verhalten.

---

# AfterContentInit – Nach dem Projizieren von Inhalten

---

## Was ist AfterContentInit?

`AfterContentInit` wird aufgerufen, nachdem Angular den **projizierten Inhalt**
(`<ng-content>`) in die Komponente eingefügt hat.  
Erst hier sind `@ContentChild`- und `@ContentChildren`-Referenzen sicher
verfügbar.

Content Projection bedeutet: Eine Parent-Komponente übergibt DOM-Inhalt an eine
Child-Komponente, die diesen Inhalt mit `<ng-content>` einbettet.

---

## Wann verwendet man AfterContentInit?

- Wenn du auf projizierte Inhalte (Content Projection via `<ng-content>`)
  zugreifen willst.
- Wenn du `@ContentChild` oder `@ContentChildren` verwendest.

---

## Schritt-für-Schritt: AfterContentInit verwenden

```typescript
import {
  Component,
  AfterContentInit,
  ContentChild,
  ElementRef,
} from "@angular/core";

@Component({
  selector: "app-wrapper",
  template: `<ng-content></ng-content>`,
  standalone: true,
})
export class WrapperComponent implements AfterContentInit {
  @ContentChild("inner") inner!: ElementRef;

  ngAfterContentInit() {
    // Projizierter Inhalt ist jetzt verfügbar
    console.log("Projizierter Inhalt:", this.inner.nativeElement);
  }
}
```

**Verwendung im Template:**

```html
<app-wrapper>
  <p #inner>Ich bin projizierter Inhalt!</p>
</app-wrapper>
```

---

## Best Practices

- **Nur einmalig:** `ngAfterContentInit` wird genau einmal aufgerufen.
- **Kein View-Rendering nötig:** Läuft vor `ngAfterViewInit`, da Content
  Projection vor dem View-Rendering abgeschlossen wird.
- **Kein DOM-Zugriff auf eigene View-Elemente:** Nutze dafür `ngAfterViewInit`.

---

# AfterContentChecked – Nach jeder Content-Prüfung

---

## Was ist AfterContentChecked?

`AfterContentChecked` wird nach **jeder Änderungserkennung** des projizierten
Inhalts aufgerufen – also bei jeder Change-Detection-Runde, sehr häufig.  
Er ist das Pendant zu `ngDoCheck`, aber speziell für projizierten Inhalt.

---

## Wann verwendet man AfterContentChecked?

- Wenn du auf jede Änderung im projizierten Inhalt reagieren möchtest.
- Wenn du Werte synchronisieren willst, die vom projizierten Inhalt abhängen.

> ⚠️ **Achtung:** Wird sehr häufig aufgerufen – vermeide aufwändige Logik und
> verändere hier keine Daten, die eine erneute Prüfung auslösen würden.

---

```typescript
import { Component, AfterContentChecked } from "@angular/core";

@Component({
  selector: "app-wrapper",
  template: `<ng-content></ng-content>`,
  standalone: true,
})
export class WrapperComponent implements AfterContentChecked {
  ngAfterContentChecked() {
    // Wird nach jeder Content-Check-Runde aufgerufen
  }
}
```

---

# AfterViewInit – Zugriff auf das View-Template nach der Initialisierung

---

## Was ist AfterViewInit?

`AfterViewInit` ist ein Lifecycle-Interface in Angular.  
Es ermöglicht dir, Code auszuführen, sobald das View-Template einer Komponente
und alle Child-Komponenten/Directives vollständig initialisiert und im DOM
verfügbar sind.

---

## Wann verwendet man AfterViewInit?

Du verwendest `AfterViewInit`, wenn du auf Elemente im Template zugreifen oder
DOM-Manipulationen durchführen möchtest, die erst nach dem Rendern des Views
möglich sind. Typische Anwendungsfälle sind:

- Zugriff auf Template-Referenzen (`@ViewChild`, `@ViewChildren`)
- Initialisierung von Drittanbieter-Bibliotheken, die DOM benötigen
- Animationen oder Messungen nach dem Rendern
- DOM-Manipulationen, die erst nach dem Aufbau des Views möglich sind

---

## Vorteile von AfterViewInit

- **Sicherer DOM-Zugriff:** Alle View-Elemente sind garantiert verfügbar.
- **Integration externer Bibliotheken:** Initialisierung von Plugins, die DOM
  benötigen.
- **Strukturierter Code:** Trennung von Initialisierungslogik und DOM-bezogener
  Logik.

---

## Schritt-für-Schritt: AfterViewInit verwenden

### 1. Interface implementieren

```typescript
import { Component, AfterViewInit, ViewChild, ElementRef } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<div #box>Box</div>`,
  standalone: true,
})
export class ExampleComponent implements AfterViewInit {
  @ViewChild("box") box!: ElementRef;

  ngAfterViewInit() {
    // Zugriff auf das DOM-Element nach dem Rendern
    this.box.nativeElement.style.backgroundColor = "lightblue";
    console.log("View wurde initialisiert!");
  }
}
```

**Erklärung:**

- Mit `implements AfterViewInit` verpflichtest du dich, die Methode
  `ngAfterViewInit()` zu schreiben.
- Angular ruft `ngAfterViewInit()` automatisch auf, nachdem das View-Template
  und alle Child-Views aufgebaut wurden.
- Erst hier sind `@ViewChild`-Elemente sicher verfügbar.

---

### 2. In einer Directive verwenden

```typescript
import { Directive, ElementRef, AfterViewInit, inject } from "@angular/core";

@Directive({
  selector: "[appFocus]",
  standalone: true,
})
export class FocusDirective implements AfterViewInit {
  private el = inject(ElementRef);

  ngAfterViewInit() {
    this.el.nativeElement.focus();
  }
}
```

---

## Best Practices

- **Kein DOM-Zugriff im Konstruktor oder ngOnInit:** Erst in `ngAfterViewInit`
  ist das View-Template verfügbar.
- **Initialisierung von Plugins/Animationen:** Starte externe Bibliotheken oder
  Animationen in `ngAfterViewInit`.
- **Keine Datenbindungs-Änderungen:** Verändere hier keine Werte, die das
  Template erneut rendern – nutze stattdessen `setTimeout()` oder
  `ngAfterViewChecked` mit Vorsicht.

---

# AfterViewChecked – Nach jeder View-Prüfung

---

## Was ist AfterViewChecked?

`AfterViewChecked` wird nach **jeder Änderungserkennung** des View-Templates und
aller Child-Views aufgerufen – also bei jeder Change-Detection-Runde.  
Er ist das Pendant zu `ngAfterContentChecked`, aber für das eigene
View-Template.

---

## Wann verwendet man AfterViewChecked?

- Wenn du nach jedem Render-Zyklus Messungen oder Anpassungen am DOM vornehmen
  möchtest.
- Wenn du auf Änderungen in Child-Komponenten reagieren musst, die nach dem
  Rendern auftreten.

> ⚠️ **Achtung:** Wird sehr häufig aufgerufen. Verändere hier **keine** Daten,
> die das Template erneut rendern würden – das führt zu einer Endlosschleife und
> dem `ExpressionChangedAfterItHasBeenChecked`-Fehler.

---

```typescript
import { Component, AfterViewChecked } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<p>Beispiel</p>`,
  standalone: true,
})
export class ExampleComponent implements AfterViewChecked {
  ngAfterViewChecked() {
    // Wird nach jeder View-Check-Runde aufgerufen
    // Hier nur leichte, nicht datenmutierenden Operationen
  }
}
```

---

# OnDestroy – Aufräumen beim Zerstören

---

## Was ist OnDestroy?

`OnDestroy` ist einer der wichtigsten Lifecycle-Hooks.  
Angular ruft `ngOnDestroy()` auf, kurz **bevor** eine Komponente oder Directive
aus dem DOM entfernt und der Speicher freigegeben wird.  
Hier solltest du alle Ressourcen freigeben, die sonst Memory Leaks verursachen
würden.

---

## Wann wird eine Komponente zerstört?

Eine Komponente wird zerstört, wenn:

- `*ngIf` auf `false` wechselt und die Komponente aus dem DOM entfernt wird.
- Der Router zu einer anderen Route wechselt und die aktuelle Komponente nicht
  mehr benötigt wird.
- Eine Komponente manuell über `ViewContainerRef.remove()` entfernt wird.
- Die übergeordnete (Parent-)Komponente selbst zerstört wird.

---

## Was sollte man in OnDestroy aufräumen?

| Ressource                        | Wie aufräumen?                          |
| -------------------------------- | --------------------------------------- |
| RxJS Subscriptions               | `.unsubscribe()` aufrufen               |
| `setInterval` / `setTimeout`     | `clearInterval()` / `clearTimeout()`    |
| Event Listener (manuell gesetzt) | `removeEventListener()` aufrufen        |
| Drittanbieter-Bibliotheken       | `.destroy()` oder `.dispose()` aufrufen |

---

## Schritt-für-Schritt: OnDestroy verwenden

### 1. Subscription aufräumen

```typescript
import { Component, OnInit, OnDestroy } from "@angular/core";
import { Subscription, interval } from "rxjs";

@Component({
  selector: "app-example",
  template: `<p>Timer läuft...</p>`,
  standalone: true,
})
export class ExampleComponent implements OnInit, OnDestroy {
  private subscription!: Subscription;

  ngOnInit() {
    this.subscription = interval(1000).subscribe((count) => {
      console.log("Tick:", count);
    });
  }

  ngOnDestroy() {
    // Subscription beenden, um Memory Leaks zu vermeiden!
    this.subscription.unsubscribe();
    console.log("Komponente zerstört, Subscription beendet.");
  }
}
```

---

### 2. Mit takeUntilDestroyed (modern, Angular 16+)

Ab Angular 16 gibt es eine elegantere Alternative mit `takeUntilDestroyed`:

```typescript
import { Component, OnInit, inject, DestroyRef } from "@angular/core";
import { takeUntilDestroyed } from "@angular/core/rxjs-interop";
import { interval } from "rxjs";

@Component({
  selector: "app-example",
  template: `<p>Timer läuft...</p>`,
  standalone: true,
})
export class ExampleComponent implements OnInit {
  private destroyRef = inject(DestroyRef);

  ngOnInit() {
    interval(1000)
      .pipe(takeUntilDestroyed(this.destroyRef))
      .subscribe((count) => {
        console.log("Tick:", count);
      });
    // Kein manuelles unsubscribe nötig!
  }
}
```

---

### 3. Timer aufräumen

```typescript
import { Component, OnInit, OnDestroy } from "@angular/core";

@Component({
  selector: "app-timer",
  template: `<p>{{ elapsed }}s vergangen</p>`,
  standalone: true,
})
export class TimerComponent implements OnInit, OnDestroy {
  elapsed = 0;
  private intervalId!: ReturnType<typeof setInterval>;

  ngOnInit() {
    this.intervalId = setInterval(() => {
      this.elapsed++;
    }, 1000);
  }

  ngOnDestroy() {
    clearInterval(this.intervalId); // Timer stoppen!
  }
}
```

---

## Best Practices

- **Immer unsubscribe:** Beende jede manuelle Subscription in `ngOnDestroy`.
- **Moderne Alternative nutzen:** `takeUntilDestroyed` oder `AsyncPipe`
  verwalten Subscriptions automatisch.
- **Drittanbieter aufräumen:** Rufe `.destroy()` für Libraries wie Charts, Maps
  oder Editoren auf.
- **Nur aufräumen:** In `ngOnDestroy` keine neue Logik starten – nur Ressourcen
  freigeben.

---

## Fazit

`OnDestroy` ist essenziell für saubere Angular-Anwendungen.  
Ohne korrektes Aufräumen entstehen **Memory Leaks** – die App verbraucht immer
mehr Speicher und wird langsamer.  
Nutze `ngOnDestroy` konsequent, um Subscriptions, Timer und externe Ressourcen
zu beenden.
