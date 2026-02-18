[Zurück zur Übersicht](../README.md)

# Angular Material installieren – Modernes UI-Design für Angular-Projekte

## Inhaltsverzeichnis

1. [Was ist Angular Material?](#was-ist-angular-material)
2. [Vorteile von Angular Material](#vorteile-von-angular-material)
3. [Schritt-für-Schritt: Installation von Angular Material](#schritt-für-schritt-installation-von-angular-material)
   - [Paket installieren](#1-angular-material-paket-installieren)
   - [Konfiguration auswählen](#2-konfiguration-auswählen)
   - [Importe prüfen](#3-importe-prüfen)
   - [Material-Komponenten verwenden](#4-material-komponenten-verwenden)
4. [Beispiel: Material Button und Input](#beispiel-material-button-und-input)
5. [Hinweise](#hinweise)
6. [Fazit](#fazit)

---

## Was ist Angular Material?

Angular Material ist eine UI-Komponentenbibliothek, die das Material Design von
Google für Angular bereitstellt.  
Mit vorgefertigten Komponenten wie Buttons, Dialogen, Tabellen und mehr kannst
du moderne, responsive und barrierefreie Oberflächen schnell umsetzen.

---

## Vorteile von Angular Material

- **Schnelle Entwicklung** durch fertige Komponenten
- **Responsives Design** für alle Geräte
- **Barrierefreiheit** (Accessibility) inklusive
- **Konsistentes UI** nach Material Design Richtlinien

---

## Schritt-für-Schritt: Installation von Angular Material

### 1. Angular Material Paket installieren

Öffne das Terminal im Projektordner und führe aus:

```bash
ng add @angular/material
```

> **Hinweis:**  
> Der Befehl installiert alle nötigen Pakete und startet einen Assistenten für
> die Konfiguration.

### 2. Konfiguration auswählen

- Wähle ein Theme (z.B. Indigo/Pink, Deep Purple/Amber, etc.)
- Entscheide, ob Angular Material Typography und Animations aktiviert werden
  sollen

### 3. Importe prüfen

Der Assistent fügt automatisch die nötigen Importe in deiner `app.module.ts`
hinzu:

```typescript
// app.module.ts
import { MatButtonModule } from "@angular/material/button";
import { MatInputModule } from "@angular/material/input";
// ...weitere Material-Module...
```

### 4. Material-Komponenten verwenden

Jetzt kannst du Material-Komponenten im Template nutzen:

```html
<button mat-raised-button>Material Button</button>
<mat-form-field>
  <input matInput placeholder="Name" />
</mat-form-field>
```

---

## Beispiel: Material Button und Input

```typescript
// app.module.ts
import { MatButtonModule } from "@angular/material/button";
import { MatInputModule } from "@angular/material/input";
// ...existing code...
@NgModule({
  imports: [
    MatButtonModule,
    MatInputModule,
    // ...existing code...
  ],
  // ...existing code...
})
export class AppModule {}
```

```html
<!-- app.component.html -->
<button mat-raised-button color="primary">Klick mich!</button>
<mat-form-field>
  <input matInput placeholder="Dein Name" />
</mat-form-field>
```

---

## Hinweise

- **Theme:** Du kannst das Theme jederzeit in der `styles.css` ändern.
- **Module:** Importiere nur die Material-Module, die du wirklich brauchst.
- **Dokumentation:** Alle Komponenten findest du unter
  [material.angular.io](https://material.angular.io/components/categories).

---

## Fazit

Mit Angular Material erhältst du moderne UI-Komponenten, die sich einfach in
dein Angular-Projekt integrieren lassen.  
Die Installation ist schnell erledigt und du kannst sofort mit dem Design
starten.
