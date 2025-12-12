[Back to Table of Contents](../README.md)

# Globale npm-Module verwalten

---

## Installierte globale Module anzeigen

Zeige alle global installierten npm-Module an:

```bash
npm list -g --depth=0
```

---

## Ein Modul global installieren

Installiere ein npm-Paket global (z.B. `typescript`):

```bash
npm install -g typescript
```

---

## Ein globales Modul deinstallieren

Deinstalliere ein global installiertes npm-Paket (z.B. `typescript`):

```bash
npm uninstall -g typescript
```

---

## Zusammenfassung

- Mit `npm list -g --depth=0` siehst du alle global installierten Module.
- Mit `npm install -g <paketname>` installierst du ein Modul global.
- Mit `npm uninstall -g <paketname>` entfernst du ein globales Modul.
