# 30. Angular BreakpointObserver

Der `BreakpointObserver` ist ein Service aus dem Angular CDK (Component Dev
Kit), der es ermöglicht, auf Änderungen von Media Queries (z.B.
Bildschirmgrößen) zu reagieren. Damit lassen sich responsive Layouts und
Komponenten einfach umsetzen.

## Inhaltsverzeichnis

- [Import & Setup](#import--setup)
- [Verwendung](#verwendung)
  - [Inject BreakpointObserver](#1-inject-breakpointobserver)
  - [Breakpoints beobachten](#2-breakpoints-beobachten)
- [Wichtige Breakpoints](#wichtige-breakpoints)
- [Beispiel: Unterschiedliche Layouts](#beispiel-unterschiedliche-layouts)
- [Was bedeuten `isHandset` und `resultmatches`](#was-bedeuten-ishandset-und-resultmatches)
- [Eigene Media Queries](#eigene-media-queries)
- [Hinweise](#hinweise)
- [Quellen](#quellen)

---

## Import & Setup

Um den `BreakpointObserver` zu verwenden, muss das Angular CDK installiert sein:

```bash
npm install @angular/cdk
```

Importiere den Service in deinem Component:

```typescript
import { BreakpointObserver, Breakpoints } from "@angular/cdk/layout";
```

---

## Verwendung

### 1. Inject BreakpointObserver

```typescript
breakpointObserver = inject(BreakpointObserver);
```

### 2. Breakpoints beobachten

```typescript
ngOnInit() {
  this.breakpointObserver.observe([Breakpoints.Handset])
    .subscribe(result => {
      if (result.matches) {
        // Handset-Layout aktiv
      }
    });
}
```

**Erklärung:**  
Die Methode `observe` gibt ein Observable zurück, das bei jeder Änderung der
Bildschirmgröße feuert.  
Das `result`-Objekt enthält die Eigenschaft `matches`, die `true` ist, wenn die
angegebene Media Query (hier: `Breakpoints.Handset`, also typischerweise ein
Smartphone) zutrifft.  
Mit `this.isHandset = result.matches;` wird also die Variable `isHandset` auf
`true` gesetzt, wenn das Gerät ein Handy ist, und auf `false`, wenn nicht.

---

## Wichtige Breakpoints

Angular CDK bietet vordefinierte Breakpoints:

- `Breakpoints.XSmall` (max-width: 599.99px)
- `Breakpoints.Small` (600px bis 959.99px)
- `Breakpoints.Medium` (960px bis 1279.99px)
- `Breakpoints.Large` (1280px bis 1919.99px)
- `Breakpoints.XLarge` (ab 1920px)
- `Breakpoints.Handset` (alle Smartphones)
- `Breakpoints.HandsetPortrait` (Smartphones im Hochformat)
- `Breakpoints.HandsetLandscape` (Smartphones im Querformat)
- `Breakpoints.Tablet` (alle Tablets)
- `Breakpoints.TabletPortrait` (Tablets im Hochformat)
- `Breakpoints.TabletLandscape` (Tablets im Querformat)
- `Breakpoints.Web` (alle Desktops)
- `Breakpoints.WebPortrait` (Desktops im Hochformat)
- `Breakpoints.WebLandscape` (Desktops im Querformat)

Du kannst auch mehrere Breakpoints gleichzeitig beobachten:

```typescript
this.breakpointObserver
  .observe([Breakpoints.Tablet, Breakpoints.Web])
  .subscribe((result) => {
    if (result.matches) {
      // Entweder Tablet oder Desktop
    }
  });
```

Oder gezielt auf Ausrichtung prüfen:

```typescript
this.breakpointObserver
  .observe([Breakpoints.HandsetPortrait])
  .subscribe((result) => {
    if (result.matches) {
      // Smartphone im Hochformat
    }
  });
```

**Tipp:**  
Du kannst auch eigene Media Queries als String angeben, z.B.
`'(min-width: 1200px)'`.

---

## Beispiel: Unterschiedliche Layouts

```typescript
import { Component, OnInit } from "@angular/core";
import { BreakpointObserver, Breakpoints } from "@angular/cdk/layout";

@Component({
  selector: "app-responsive",
  template: `
    <div *ngIf="isHandset">Handy-Layout</div>
    <div *ngIf="!isHandset">Desktop-Layout</div>
  `,
})
export class ResponsiveComponent implements OnInit {
  isHandset = false;

  breakpointObserver = inject(BreakpointObserver);

  ngOnInit() {
    this.breakpointObserver
      .observe([Breakpoints.Handset])
      .subscribe((result) => {
        // result.matches ist true, wenn das Gerät ein Handy ist
        this.isHandset = result.matches;
      });
  }
}
```

**Was bedeuten `isHandset` und `result.matches`?**

- **`isHandset`** ist eine selbst gewählte Variable im Component-Code. Sie
  speichert, ob das aktuelle Gerät ein "Handset" (also ein Smartphone) ist. Der
  Name kann beliebig gewählt werden, z.B. auch `isMobile`.
- **`result.matches`** ist eine Eigenschaft des Objekts, das vom
  BreakpointObserver zurückgegeben wird. Sie ist `true`, wenn die beobachtete
  Media Query (hier: `Breakpoints.Handset`) aktuell zutrifft, also wenn das
  Fenster die Größe eines Smartphones hat. Ist das Fenster größer, ist
  `result.matches` `false`.

**Zusammengefasst:**  
Immer wenn sich die Fenstergröße ändert, prüft der BreakpointObserver, ob die
Bedingung für ein Handy erfüllt ist. Das Ergebnis (`true` oder `false`) wird in
der Variable `isHandset` gespeichert. Die Angular-Template-Logik zeigt dann das
passende Layout an.

---

## Eigene Media Queries

Eigene Media Queries können als String übergeben werden:

```typescript
this.breakpointObserver.observe(["(max-width: 800px)"]).subscribe((result) => {
  if (result.matches) {
    // kleiner als 800px
  }
});
```

**Erklärung:**  
Hier wird geprüft, ob die Fensterbreite kleiner als 800px ist.  
`result.matches` ist `true`, wenn die Bedingung zutrifft.

---

## Hinweise

- Der `BreakpointObserver` gibt ein Observable zurück, das bei jeder Änderung
  der Media Query feuert.
- Mehrere Breakpoints können gleichzeitig beobachtet werden.
- Für komplexe Layouts empfiehlt sich die Kombination mit Angular Flex Layout.

---

## Quellen

- [Angular CDK Layout Guide](https://material.angular.io/cdk/layout/overview)
- [API Doku: BreakpointObserver](https://material.angular.io/cdk/layout/api)
