[Zurück zur Übersicht](../README.md)

# ngx-translate – Mehrsprachigkeit in Angular (ab v20)

---

## Was ist ngx-translate?

[ngx-translate](https://github.com/ngx-translate/core) ist eine beliebte
Bibliothek, um Angular-Anwendungen mehrsprachig zu machen.  
Sie ermöglicht das einfache Laden, Verwalten und Umschalten von Übersetzungen
zur Laufzeit.

---

## Vorteile von ngx-translate

- **Dynamisches Umschalten** der Sprache zur Laufzeit
- **Einfache JSON-Dateien** für Übersetzungen
- **Asynchrone Ladeoptionen** (z.B. HTTP)
- **Integration mit Pipes und Direktiven** im Template

---

## Installation

```bash
npm install @ngx-translate/core @ngx-translate/http-loader
```

---

## Grundkonfiguration (Angular v20)

1. **Importe im App-Modul:**

```typescript
// app.config.ts (Standalone API)
import { ApplicationConfig } from "@angular/core";
import { provideHttpClient, withInterceptors } from "@angular/common/http";
import { provideTranslate } from "@ngx-translate/core";

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(withInterceptors([])),
    provideTranslate({
      defaultLanguage: "de",
      useDefaultLang: true,
      loader: {
        provide: "TRANSLATE_LOADER",
        useFactory: (http: HttpClient) =>
          new TranslateHttpLoader(http, "./assets/i18n/", ".json"),
        deps: [HttpClient],
      },
    }),
  ],
};
```

2. **JSON-Übersetzungsdateien anlegen:**

Lege z.B. `assets/i18n/de.json` und `assets/i18n/en.json` an:

```json
// de.json
{
  "greeting": "Hallo Welt"
}
```

```json
// en.json
{
  "greeting": "Hello World"
}
```

---

## Übersetzungen im Template verwenden

Mit der Pipe:

```html
<p>{{ 'greeting' | translate }}</p>
```

Oder als Attribut:

```html
<button [attr.aria-label]="'greeting' | translate"></button>
```

---

## Übersetzungen im TypeScript-Code verwenden

```typescript
import { TranslateService } from '@ngx-translate/core';

constructor(private translate: TranslateService) {}

ngOnInit() {
  this.translate.get('greeting').subscribe((res: string) => {
    console.log(res); // Gibt die Übersetzung aus
  });
}
```

---

## Sprache zur Laufzeit wechseln

```typescript
this.translate.use("en"); // Wechselt auf Englisch
```

---

## Platzhalter und Parameter

```json
// de.json
{
  "welcome": "Willkommen, {{name}}!"
}
```

```html
<p>{{ 'welcome' | translate:{ name: userName } }}</p>
```

---

## Fallback-Sprache

```typescript
this.translate.setDefaultLang("de");
this.translate.use("en");
```

Wenn ein Key in `en` fehlt, wird automatisch `de` verwendet.

---

## Weitere Tipps

- **Strukturierte Keys**: Nutze verschachtelte Objekte für größere Projekte.
- **Lazy Loading**: Lade Übersetzungen nur bei Bedarf.
- **Direktiven**: Es gibt auch die `TranslateDirective` für Attribute.

---

## Offizielle Dokumentation

Weitere Details und fortgeschrittene Features findest du in der
[offiziellen ngx-translate-Dokumentation](https://www.codeandweb.com/babeledit/tutorials/how-to-translate-your-angular-app-with-ngx-translate).

---

## Fazit

Mit ngx-translate kannst du Angular-Apps einfach und flexibel mehrsprachig
machen.  
Die Integration ist unkompliziert und unterstützt sowohl Templates als auch
TypeScript-Code.

---
