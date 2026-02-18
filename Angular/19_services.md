[Zurück zur Übersicht](../README.md)

# Services – Gemeinsame Logik und Daten teilen

---

## Inhaltsverzeichnis

1. [Was sind Services?](#was-sind-services)
2. [Vorteile von Services](#vorteile-von-services)
3. [Begriffe und Datenfluss](#begriffe-und-datenfluss)
4. [Schritt-für-Schritt: Service nutzen](#schritt-für-schritt-service-nutzen)
   - [Service erstellen](#1-service-erstellen)
   - [Service in einer Komponente verwenden](#2-service-in-einer-komponente-verwenden)
5. [Fazit](#fazit)

---

## Was sind Services?

Mit Services kannst du in Angular gemeinsam genutzte Logik, Daten oder
Funktionen auslagern.  
Sie ermöglichen die Trennung von Geschäftslogik und Komponenten und fördern
Wiederverwendbarkeit und Testbarkeit.  
Services werden meist für Datenhaltung, API-Kommunikation oder Hilfsfunktionen
verwendet.

---

## Vorteile von Services

- **Wiederverwendbarkeit:** Ein Service kann von mehreren Komponenten genutzt
  werden.
- **Trennung von Anliegen:** Komponenten bleiben schlank, Logik wird
  ausgelagert.
- **Zentrale Datenhaltung:** Gemeinsame Daten werden zentral verwaltet.
- **Einfaches Testen:** Services lassen sich unabhängig testen.

---

## Begriffe und Datenfluss

- **Service:** Eine Klasse, die Logik oder Daten kapselt und bereitstellt.
- **Dependency Injection:** Services werden Komponenten oder anderen Services
  automatisch bereitgestellt.
- **Provider:** Definiert, wo und wie ein Service verfügbar ist (z.B. global mit
  `providedIn: 'root'`).

---

## Schritt-für-Schritt: Service nutzen

### 1. Service erstellen

Ein Service ist eine Klasse mit dem `@Injectable()`-Decorator.  
Hier ein Beispiel mit echten Fruchtdaten:

```typescript
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root", // Service ist global als Singleton verfügbar
})
export class FruitlistdataService {
  fruitlist = [
    {
      name: "Apfel",
      img: "apple.png",
      description:
        "Äpfel sind aufgrund ihres hohen Wassergehalts kalorienarm und enthalten nur Spuren von Fett und Eiweiß, dafür aber rund zwei Prozent Ballaststoffe und etwa elf Prozent Kohlenhydrate. Äpfel enthalten auch viele Vitamine und Mineralstoffe und sind daher eine wichtige Quelle für uns - zum Beispiel für Vitamin C.",
      genus: "Kernobstgewächsen innerhalb der Familie der Rosengewächse",
      stars: 2.3,
      reviews: [
        { name: "Kevin W.", text: "ist lecker" },
        { name: "Arne P.", text: "nicht so meins" },
      ],
    },
    // ...weitere Früchte wie in deinem Projekt...
  ];

  addCommentToFruit(comment: string, index: number) {
    this.fruitlist[index].reviews.push({
      name: "Philipp B.",
      text: comment,
    });
  }
}
```

**Erklärung:**

- Die Daten (z.B. `fruitlist`) werden zentral im Service gehalten.
- Methoden wie `addCommentToFruit` kapseln die Logik für alle Komponenten.

---

### 2. Service in einer Komponente verwenden

Der Service wird per Dependency Injection eingebunden und genutzt:

```typescript
import { Component, inject } from "@angular/core";
import { FruitlistdataService } from "../fruitlistdataService";

@Component({
  selector: "app-fruitlist",
  // ...existing code...
})
export class Fruitlist {
  fruitlistdata = inject(FruitlistdataService);

  addComment(comment: string, index: number) {
    this.fruitlistdata.addCommentToFruit(comment, index);
  }
}
```

**Erklärung:**

- Mit `inject(FruitlistdataService)` erhält die Komponente Zugriff auf die
  zentralen Daten und Methoden.
- Änderungen (z.B. Kommentare) werden direkt im Service gespeichert und stehen
  allen Komponenten zur Verfügung.

---

## Fazit

Services sind ein zentrales Konzept in Angular, um Logik und Daten zu kapseln
und zwischen Komponenten zu teilen.  
Sie sorgen für sauberen, wartbaren und testbaren Code.  
Durch die zentrale Datenhaltung und die klare Trennung von Logik und Darstellung
werden Angular-Anwendungen übersichtlicher und flexibler.
