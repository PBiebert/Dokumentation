[Back to Table of Contents](../README.md)

# http-server mit npm installieren und für SPAs nutzen

---

## 1. Installation

Installiere `http-server` global mit npm:

```bash
npm install -g http-server
```

---

## 2. Anwendung starten

Wechsle in den Ordner mit deinem Build (z.B. `dist/`):

```bash
cd dist/
http-server
```

Standardmäßig ist die Anwendung dann unter
[http://localhost:8080](http://localhost:8080) erreichbar.

---

## 3. Problem: SPA Routing und „Not Found“

Single Page Applications (SPA) verwenden clientseitiges Routing.  
Wenn du eine Route direkt aufrufst (z.B. `/imprint`), sucht der Server nach
einer echten Datei und gibt sonst einen 404-Fehler zurück.

---

## 4. Lösung: SPA-Modus aktivieren

Starte `http-server` mit dem `--spa`-Flag, damit alle Anfragen auf `index.html`
umgeleitet werden:

```bash
http-server --spa
```

**Wichtig:**  
Dadurch funktioniert das Routing auch beim direkten Aufruf einer Route.

---

## 5. Weitere nützliche Optionen

- **Anderen Port wählen:**
  ```bash
  http-server --spa -p 4200
  ```
- **Öffne Browser automatisch:**
  ```bash
  http-server --spa -o
  ```

---

## 6. Server beenden

Um den laufenden `http-server` zu beenden, drücke im Terminal:

**Strg + C**

Dadurch wird der Server gestoppt und der Port wieder freigegeben.

---

## Zusammenfassung

- Installiere `http-server` global mit `npm install -g http-server`.
- Starte den Server im Build-Ordner mit `http-server --spa`.
- Das `--spa`-Flag ist nötig, damit SPA-Routing funktioniert.
