[Zurück zum Inhaltsverzeichnis](../README.md)

# Klassen in JavaScript

## Inhaltsverzeichnis

1. [Was sind Klassen?](#was-sind-klassen)
2. [Aufbau einer Klasse](#aufbau-einer-klasse)
3. [Vererbung](#vererbung)
4. [Methoden in Klassen](#methoden-in-klassen)
5. [Objekte aus Klassen erstellen](#objekte-aus-klassen-erstellen)
6. [Beispiel: Kontaktbuch](#beispiel-kontaktbuch)
7. [Zusammenfassung](#zusammenfassung)
8. [Klassendiagramm](#klassendiagramm)

## Was sind Klassen?

Klassen sind Baupläne für Objekte. Sie definieren, wie ein Objekt aussehen soll
und welche Eigenschaften und Methoden es besitzt. Klassen helfen dabei, komplexe
Strukturen übersichtlich und wiederverwendbar zu organisieren.

---

## Aufbau einer Klasse

Eine Klasse wird mit dem Schlüsselwort `class` definiert. Eigenschaften werden
meist im Konstruktor initialisiert.

```javascript
class Person {
  firstName;
  lastName;

  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}
```

**Erklärung:**

- `class Person { ... }`: Definiert eine neue Klasse namens `Person`.
- `constructor(...)`: Die Funktion, die beim Erstellen eines neuen Objekts
  aufgerufen wird.
- `this`: Bezieht sich auf das aktuelle Objekt.

---

## Vererbung

Mit `extends` kann eine Klasse von einer anderen erben. Die Kindklasse übernimmt
alle Eigenschaften und Methoden der Elternklasse.

```javascript
class Friend extends Person {
  constructor(firstName, lastName) {
    super(firstName, lastName);
  }
}
```

**Erklärung:**

- `extends Person`: Friend erbt von Person.
- `super(...)`: Ruft den Konstruktor der Elternklasse auf.

---

## Methoden in Klassen

Methoden sind Funktionen, die zu einer Klasse gehören und auf deren
Eigenschaften zugreifen können.

```javascript
class Contact extends Person {
  phone;

  constructor(firstName, lastName, phone) {
    super(firstName, lastName);
    this.phone = phone;
  }

  printFullName() {
    console.log(`${this.firstName}, ${this.lastName}`);
  }

  call() {
    window.location.href = "tel:" + this.phone;
  }
}
```

**Erklärung:**

- `printFullName()`: Gibt den vollständigen Namen aus.
- `call()`: Öffnet die Telefon-App mit der gespeicherten Nummer.

---

## Objekte aus Klassen erstellen

Mit `new` wird ein neues Objekt aus einer Klasse erstellt.

```javascript
let myContact = new Contact("Philipp", "Biebert", "017333333");
```

---

## Beispiel: Kontaktbuch

```javascript
let contacts = [];

function addContact(firstName, lastName, phone) {
  let myContact = new Contact(firstName, lastName, phone);
  contacts.push(myContact);
}

addContact("Philipp", "Biebert", "017333333");
```

---

## Zusammenfassung

- Klassen sind Vorlagen für Objekte.
- Eigenschaften werden im Konstruktor gesetzt.
- Methoden sind Funktionen innerhalb der Klasse.
- Vererbung ermöglicht Code-Wiederverwendung.
- Instanzen werden mit `new` erstellt.

---

## Klassendiagramm

```
Person
  ├── firstName
  ├── lastName
  │
  ├── Contact (erbt von Person)
  │     ├── phone
  │     ├── printFullName()
  │     └── call()
  │
  └── Friend (erbt von Person)
```

---
