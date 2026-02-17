[Zurück zur Übersicht](../README.md)

# ChangeDetectorRef in Angular

---

## Was ist der ChangeDetectorRef?

`ChangeDetectorRef` ist ein Service in Angular, mit dem du die Change Detection
(Änderungserkennung) gezielt steuern kannst.  
Er wird verwendet, um Angular mitzuteilen, dass sich Daten geändert haben und
das View aktualisiert werden soll – oder um die automatische Erkennung zeitweise
auszusetzen.

---

## Inhaltsverzeichnis

1. [Was ist der ChangeDetectorRef?](#was-ist-der-changedetectorref)
2. [Wann verwendet man ChangeDetectorRef?](#wann-verwendet-man-changedetectorref)
3. [Vorteile von ChangeDetectorRef](#vorteile-von-changedetectorref)
4. [Begriffe](#begriffe)
5. [Schritt-für-Schritt: ChangeDetectorRef verwenden](#schritt-für-schritt-changedetectorref-verwenden)
   - [ChangeDetectorRef injizieren](#1-changedetectorref-injizieren)
   - [Async Submit mit ChangeDetectorRef](#1a-beispiel-async-submit-mit-changedetectorref)
   - [Change Detection pausieren und wieder aufnehmen](#2-change-detection-pausieren-und-wieder-aufnehmen)
6. [Methoden von ChangeDetectorRef](#methoden-von-changedetectorref)
7. [Best Practices](#best-practices)
8. [Fazit](#fazit)

## Wann verwendet man ChangeDetectorRef?

Du verwendest `ChangeDetectorRef`, wenn...

- du nach asynchronen Operationen (z.B. `setTimeout`, Observables) das View
  manuell aktualisieren musst.
- du Performance optimieren willst, indem du die Change Detection gezielt
  kontrollierst.
- du in Lifecycle-Hooks wie `ngAfterViewInit` oder bei dynamischen
  DOM-Änderungen sicherstellen willst, dass das View korrekt aktualisiert wird.
- du die Change Detection für eine Komponente temporär deaktivieren möchtest.

---

## Vorteile von ChangeDetectorRef

- **Feingranulare Kontrolle:** Du entscheidest, wann Angular das View
  aktualisiert.
- **Performance-Optimierung:** Unnötige Change Detection-Zyklen können vermieden
  werden.
- **Flexibilität:** Besonders nützlich bei komplexen oder dynamischen
  UI-Updates.

---

## Begriffe

- **Change Detection:** Mechanismus von Angular, der Änderungen im Datenmodell
  erkennt und das View aktualisiert.
- **ChangeDetectorRef:** Service, um die Change Detection manuell zu steuern.

---

## Schritt-für-Schritt: ChangeDetectorRef verwenden

### 1. ChangeDetectorRef injizieren

```typescript
import { Component, ChangeDetectorRef } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<p>{{ value }}</p>`,
  standalone: true,
})
export class ExampleComponent {
  value = "Initial";

  constructor(private cdr: ChangeDetectorRef) {}

  updateValueAsync() {
    setTimeout(() => {
      this.value = "Aktualisiert!";
      this.cdr.detectChanges(); // Manuelles Triggern der Change Detection
    }, 1000);
  }
}
```

**Erklärung:**

- `ChangeDetectorRef` wird im Konstruktor injiziert.
- Nach einer asynchronen Änderung wird mit `detectChanges()` das View
  aktualisiert.

---

### 1a. Beispiel: Async Submit mit ChangeDetectorRef

```typescript
import { Component, ChangeDetectorRef } from "@angular/core";

@Component({
  selector: "app-submit-example",
  template: `
    <form (ngSubmit)="onSubmit()">
      <button type="submit">Absenden</button>
    </form>
    <p>{{ status }}</p>
  `,
  standalone: true,
})
export class SubmitExampleComponent {
  status = "Noch nicht gesendet";

  constructor(private cdr: ChangeDetectorRef) {}

  async onSubmit() {
    this.status = "Wird gesendet...";
    await this.fakeSubmit();
    this.status = "Erfolgreich gesendet!";
    this.cdr.detectChanges(); // Nach Abschluss der Async-Operation View aktualisieren
  }

  private async fakeSubmit(): Promise<void> {
    return new Promise((resolve) => setTimeout(resolve, 1500));
  }
}
```

**Erklärung:**

- Nach dem asynchronen Submit wird mit `detectChanges()` das View aktualisiert.
- Besonders nützlich, wenn Angular die Änderung nicht automatisch erkennt (z.B.
  bei externen Callbacks oder außerhalb von Angular-Zonen).

---

### 2. Change Detection pausieren und wieder aufnehmen

```typescript
import { Component, ChangeDetectorRef } from "@angular/core";

@Component({
  selector: "app-pause",
  template: `<p>{{ counter }}</p>`,
  standalone: true,
})
export class PauseComponent {
  counter = 0;

  constructor(private cdr: ChangeDetectorRef) {}

  stopDetection() {
    this.cdr.detach(); // Change Detection für diese Komponente pausieren
  }

  resumeDetection() {
    this.cdr.reattach(); // Change Detection wieder aktivieren
    this.cdr.detectChanges(); // Optional: sofortiges Update
  }
}
```

---

## Methoden von ChangeDetectorRef

| Methode           | Beschreibung                                                           |
| ----------------- | ---------------------------------------------------------------------- |
| `detectChanges()` | Startet die Change Detection für die Komponente und ihre Kinder.       |
| `markForCheck()`  | Markiert die Komponente für die nächste Change Detection (bei OnPush). |
| `detach()`        | Pausiert die automatische Change Detection für die Komponente.         |
| `reattach()`      | Aktiviert die Change Detection wieder.                                 |

---

## Best Practices

- **Nur bei Bedarf verwenden:** In den meisten Fällen reicht die automatische
  Change Detection von Angular aus.
- **Mit Lifecycle-Hooks kombinieren:** Häufig in `ngAfterViewInit` oder nach
  asynchronen Updates sinnvoll.
- **Mit OnPush-Strategie:** Besonders nützlich, wenn du die Change
  Detection-Strategie `OnPush` verwendest.

---

## Fazit

Mit `ChangeDetectorRef` hast du die volle Kontrolle über die Aktualisierung
deiner Views in Angular.  
Das ist besonders hilfreich bei asynchronen Prozessen, Performance-Optimierungen
oder komplexen UI-Interaktionen.

---
