# Angular NgForm – Template Formulare & Validierung

Angulars `NgForm`-Direktive macht das Arbeiten mit Formularen einfach und mächtig: Sie verwaltet den Status eines Formulars und ermöglicht direkte Bindung im Template sowie Zugriff auf Validierungen im TypeScript-Code.

---

## Inhaltsverzeichnis

- [Import & Setup](#import--setup)
- [Verwendung – NgForm im Template](#verwendung--ngform-im-template)
- [Eigenschaften der Validierung](#eigenschaften-der-validierung)
- [Validierung im Template](#validierung-im-template)
- [Validierung in TypeScript nutzen](#validierung-in-typescript-nutzen)
- [Beispiel: Ein einfaches Login-Formular](#beispiel-ein-einfaches-login-formular)
- [Weitere Hinweise](#weitere-hinweise)
- [Quellen](#quellen)

---

## Import & Setup

Um Template Formulare in Angular zu verwenden, muss das Modul importiert werden:

```typescript
import { FormsModule } from '@angular/forms';