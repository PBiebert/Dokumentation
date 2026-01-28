[Zurück zur Übersicht](../README.md)

# Formulare mit ngSubmit in Angular

---

## 1. Was ist ngSubmit?

`ngSubmit` ist ein Angular-Event für das sichere und komfortable Absenden von
Formularen.  
Es sorgt dafür, dass beim Absenden eines Formulars eine Methode in deiner
Komponente aufgerufen wird – egal ob per Button oder Enter-Taste.

---

## 2. Vorteile von ngSubmit

- Zentrale Formularverarbeitung in einer Methode
- Kein Seiten-Reload, sondern Angular-Logik
- Einfache Validierung und Verarbeitung der Formulardaten
- Sicher: Angular verhindert versehentliche Reloads

---

## 3. Voraussetzungen

- Das `FormsModule` muss importiert sein.
- Jedes Eingabefeld mit `ngModel` benötigt ein `name`-Attribut.

---

## 4. Schritt-für-Schritt-Anleitung

1. **FormsModule importieren**  
   (Im Modul oder in der Komponente)
2. **Formular im Template anlegen**
   ```html
   <form (ngSubmit)="onSubmit()">
     <input [(ngModel)]="username" name="username" required />
     <button type="submit">Absenden</button>
   </form>
   ```
3. **Eingabefelder mit ngModel binden**  
   (und `name`-Attribut vergeben)
4. **Methode in der Komponente implementieren**
   ```typescript
   // app.component.ts
   username = "";
   onSubmit() {
     alert(`Formular abgeschickt! Benutzername: ${this.username}`);
   }
   ```

---

## 5. Validierung vor dem Absenden

```typescript
// app.component.ts
email = "";
onSubmit() {
  if (this.email.includes("@")) {
    alert("E-Mail akzeptiert!");
  } else {
    alert("Bitte eine gültige E-Mail eingeben.");
  }
}
```

```html
<!-- app.component.html -->
<form (ngSubmit)="onSubmit()">
  <input [(ngModel)]="email" name="email" required />
  <button type="submit">Absenden</button>
</form>
```

---

## 6. Hinweise

- `ngSubmit` funktioniert nur auf `<form>`-Elementen.
- Angular verhindert das Standardverhalten (kein Seiten-Reload).
- Eigene Validierungen sind vor dem Absenden möglich.
- Für jedes Eingabefeld mit `ngModel` ist ein `name`-Attribut Pflicht.

---

## 7. Beispiel: Formular mit mehreren Feldern

```typescript
// app.component.ts
user = { name: "", email: "" };
onSubmit() {
  console.log("Formulardaten:", this.user);
}
```

```html
<!-- app.component.html -->
<form (ngSubmit)="onSubmit()">
  <input [(ngModel)]="user.name" name="name" placeholder="Name" required />
  <input [(ngModel)]="user.email" name="email" placeholder="E-Mail" required />
  <button type="submit">Absenden</button>
</form>
```

---

## 8. Methodenübersicht

| Syntax                        | Beschreibung                                 |
| ----------------------------- | -------------------------------------------- |
| `<form (ngSubmit)="fn()">`    | Methode beim Absenden des Formulars aufrufen |
| `<button type="submit">`      | Löst ngSubmit aus                            |
| `<input [(ngModel)]="val" />` | Bindet Eingabefeld an Property               |

---

## 9. Fazit

Mit `ngSubmit` verarbeitest du Formulare in Angular sicher, flexibel und ohne
Seiten-Reload.  
Validierungen und eigene Logik lassen sich zentral und übersichtlich umsetzen.

---
