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
      }),
    ),
    provideHttpClient(),
  ],
};
```

---

## Schritt-für-Schritt: Scrollen nach oben aktivieren

1. **Importiere** `withInMemoryScrolling` aus `@angular/router`.
2. **Füge** `withInMemoryScrolling({ scrollPositionRestoration: 'top' })` als
   Option beim Aufruf von `provideRouter` hinzu.
3. **Fertig!** Bei jedem Routenwechsel scrollt Angular automatisch zum
   Seitenanfang.

---

## Methodenübersicht

| Option                                  | Beschreibung                                    |
| --------------------------------------- | ----------------------------------------------- |
| `scrollPositionRestoration: 'top'`      | Scrollt bei Navigation immer zum Seitenanfang   |
| `scrollPositionRestoration: 'enabled'`  | Stellt die vorherige Scrollposition wieder her  |
| `scrollPositionRestoration: 'disabled'` | Kein automatisches Scrollen (Standardverhalten) |

---

## Hinweise

- **Kein zusätzlicher Code** in Komponenten oder Directives nötig.
- Funktioniert **out-of-the-box** mit Angular v20 und neuer.
- Die Einstellung gilt **global** für alle Routen.

---

## Fazit

Mit der Option `scrollPositionRestoration: 'top'` im Router-Provider kannst du
das Scrollverhalten bei Routenwechseln in Angular ganz einfach steuern.  
Das sorgt für ein konsistentes und angenehmes Nutzererlebnis – ohne zusätzlichen
Aufwand!

---
