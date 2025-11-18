[Back to Table of Contents](../README.md)

# Classes in JavaScript

## What are Classes?

Classes are blueprints for objects. They define how an object should look and which properties and methods it has. Classes help to organize complex structures in a clear and reusable way.

---

## Structure of a Class

A class is defined using the `class` keyword. Properties are usually initialized in the constructor.

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

**Explanation:**

- `class Person { ... }`: Defines a new class called `Person`.
- `constructor(...)`: The function that is called when a new object is created.
- `this`: Refers to the current object.

---

## Inheritance

With `extends`, a class can inherit from another. The child class takes over all properties and methods of the parent class.

```javascript
class Friend extends Person {
  constructor(firstName, lastName) {
    super(firstName, lastName);
  }
}
```

**Explanation:**

- `extends Person`: Friend inherits from Person.
- `super(...)`: Calls the constructor of the parent class.

---

## Methods in Classes

Methods are functions that belong to a class and can access its properties.

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

**Explanation:**

- `printFullName()`: Outputs the full name.
- `call()`: Opens the phone app with the stored number.

---

## Creating Objects from Classes

With `new`, a new object is created from a class.

```javascript
let myContact = new Contact("Philipp", "Biebert", "017333333");
```

---

## Example: Contact Book

```javascript
let contacts = [];

function addContact(firstName, lastName, phone) {
  let myContact = new Contact(firstName, lastName, phone);
  contacts.push(myContact);
}

addContact("Philipp", "Biebert", "017333333");
```

---

## Summary

- Classes are templates for objects.
- Properties are set in the constructor.
- Methods are functions inside the class.
- Inheritance enables code reuse.
- Instances are created with `new`.

---

## Class Diagram

```
Person
  ├── firstName
  ├── lastName
  │
  ├── Contact (inherits from Person)
  │     ├── phone
  │     ├── printFullName()
  │     └── call()
  │
  └── Friend (inherits from Person)
```

---
