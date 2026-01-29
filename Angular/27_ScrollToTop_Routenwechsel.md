[Zurück zur Übersicht](../README.md)

# Scrollen nach oben bei Routenwechsel in Angular (ab v20)

---

## Was ist Scrollen nach oben bei Routenwechsel?

Beim Navigieren zwischen verschiedenen Seiten (Routen) in einer
Angular-Anwendung bleibt standardmäßig die aktuelle Scrollposition erhalten.  
Das kann zu einem unerwünschten Nutzererlebnis führen, wenn z.B. beim Wechsel
auf eine neue Seite nicht automatisch nach oben gescrollt wird.

Mit Angular v20 kannst du das Scrollverhalten ganz einfach zentral
konfigurieren, sodass bei jedem Routenwechsel automatisch zum Seitenanfang
gescrollt wird.

---

## Vorteile dieser Lösung

- **Zentrale Konfiguration:** Kein zusätzlicher Code in Komponenten nötig
- **Verbessertes Nutzererlebnis:** Immer Start am Seitenanfang nach Navigation
- **Einfach und wartbar:** Nutzung von Angular-Bordmitteln

---

## Umsetzung in Angular v20

Ab Angular v20 kannst du das Scrollverhalten direkt im Router-Provider
konfigurieren.  
Dazu nutzt du die Funktion `withInMemoryScrolling` mit der Option
`scrollPositionRestoration: 'top'`.

**Zusätzlich kannst du mit der Option `anchorScrolling` steuern, ob beim
Navigieren zu einem Fragment (z.B. `#section1`) automatisch zu diesem Element
gescrollt wird:**

```typescript
withInMemoryScrolling({
  scrollPositionRestoration: "top",
  anchorScrolling: "enabled", // oder "disabled"
}),
```

- `anchorScrolling: "enabled"`: Scrollt automatisch zu einem Fragment (z.B.
  `#zielId`).
- `anchorScrolling: "disabled"`: Kein automatisches Scrollen zu Fragmenten
  (Standard).

---

## Beispiel: app.config.ts

```typescript
// app.config.ts
import {
  ApplicationConfig,
  provideBrowserGlobalErrorListeners,
  provideZoneChangeDetection,
} from "@angular/core";
import { provideRouter, withInMemoryScrolling } from "@angular/router";

import { routes } from "./app.routes";
import { provideHttpClient } from "@angular/common/http";

export const appConfig: ApplicationConfig = {
  providers: [
    provideBrowserGlobalErrorListeners(),
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(
      routes,
      withInMemoryScrolling({
        scrollPositionRestoration: "top",
        anchorScrolling: "enabled",
      }),
    ),
    provideHttpClient(),
  ],
};
```

---

## Schritt-für-Schritt: Scrollen nach oben aktivieren

1. **Importiere** `withInMemoryScrolling` aus `@angular/router`.
2. **Füge**
   `withInMemoryScrolling({ scrollPositionRestoration: 'top', anchorScrolling: 'enabled' })`
   als Option beim Aufruf von `provideRouter` hinzu.
3. **Fertig!** Bei jedem Routenwechsel scrollt Angular automatisch zum
   Seitenanfang oder – falls ein Fragment angegeben ist – zur passenden ID.

---

## Zu bestimmten IDs scrollen: routerLink & fragment

Mit Angular kannst du gezielt zu bestimmten Elementen auf einer Seite scrollen,
indem du das `fragment`-Attribut von `routerLink` nutzt.  
Das funktioniert nur, wenn `anchorScrolling: "enabled"` gesetzt ist.

**Beispiel:**

```html
<a [routerLink]="['/zielseite']" fragment="zielId">Zum Abschnitt</a>
```

Das erzeugt einen Link wie `/zielseite#zielId`.  
Angular scrollt beim Navigieren automatisch zum Element mit `id="zielId"`.

**UrlTree generieren (z.B. im TypeScript-Code):**

```typescript
import { Router } from "@angular/router";

// ...existing code...
const urlTree = this.router.createUrlTree(["/zielseite"], {
  fragment: "zielId",
});
// Navigieren:
this.router.navigateByUrl(urlTree);
```

---

## Methodenübersicht

| Option                                  | Beschreibung                                    |
| --------------------------------------- | ----------------------------------------------- |
| `scrollPositionRestoration: 'top'`      | Scrollt bei Navigation immer zum Seitenanfang   |
| `scrollPositionRestoration: 'enabled'`  | Stellt die vorherige Scrollposition wieder her  |
| `scrollPositionRestoration: 'disabled'` | Kein automatisches Scrollen (Standardverhalten) |
| `anchorScrolling: 'enabled'`            | Scrollt zu Fragment-IDs (z.B. `#zielId`)        |
| `anchorScrolling: 'disabled'`           | Kein Scrollen zu Fragmenten (Standard)          |

---

## Hinweise

- **Kein zusätzlicher Code** in Komponenten oder Directives nötig.
- Funktioniert **out-of-the-box** mit Angular v20 und neuer.
- Die Einstellung gilt **global** für alle Routen.
- Für das Scrollen zu Fragmenten (`#id`) muss das Ziel-Element ein eindeutiges
  `id`-Attribut besitzen.

---

## Fazit

Mit der Option `scrollPositionRestoration: 'top'` und ggf.
`anchorScrolling: 'enabled'` im Router-Provider kannst du das Scrollverhalten
bei Routenwechseln in Angular ganz einfach steuern.  
Das sorgt für ein konsistentes und angenehmes Nutzererlebnis – ohne zusätzlichen
Aufwand!

---
