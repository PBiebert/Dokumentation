[Back to Table of Contents](../README.md)

## Inhaltsverzeichnis

1. [Angular ist eine Single Page Application (SPA)](#angular-ist-eine-single-page-application-spa)
2. [Wozu die .htaccess?](#-wozu-die-htaccess)
3. [Brauche ich die .htaccess immer?](#brauche-ich-die-htaccess-immer)
4. [Warum generiert Angular keine .htaccess?](#warum-generiert-angular-keine-htaccess)
5. [Merksatz](#merksatz)

---

## Angular ist eine Single Page Application (SPA)

- Es gibt nur eine einzige `index.html`.
- Angular regelt die Routen im Browser (z. B. `/dashboard`, `/login`,
  `/products/42`).

**Problem:**  
Wenn du im Browser direkt auf eine Route gehst (z. B. `example.com/login`),  
fragt dein Server nach einer Datei oder einem Ordner `login/`.  
Die gibt es nicht → **404 Not Found**.

---

## Wozu die .htaccess?

Die `.htaccess` sorgt dafür, dass alle Anfragen auf deine Angular-App immer zur
`index.html` umgeleitet werden.  
Dann übernimmt Angular das Routing.

**Typische .htaccess für Angular:**

```apache
# filepath: .htaccess
RewriteEngine On

RewriteCond %{REQUEST_FILENAME} -s [OR]
RewriteCond %{REQUEST_FILENAME} -l [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^.*$ - [NC,L]

RewriteRule ^(.*) /index.html [NC,L]
```

**Das bedeutet:**  
Wenn die angefragte Datei nicht existiert,  
→ liefere immer `index.html` aus.

---

## Brauche ich die .htaccess immer?

**Nur auf Apache-Servern, wenn du Angular mit Routing nutzt!**

Du brauchst `.htaccess`, wenn:

- dein Server Apache ist
- du Angular-Routing nutzt (`/dashboard`, `/users/:id`)
- du eine SPA bist

Du brauchst sie **nicht**, wenn:

- du keine Routen hast
- dein Server ein Node.js-Server ist (z. B. express)
- du auf Firebase Hosting, Netlify, Vercel, GitHub Pages usw. hostest (diese
  Anbieter haben eigene Routing-Regeln)

---

## Warum generiert Angular keine .htaccess?

- Angular ist framework-agnostisch und weiß nicht, auf welchem Server du
  hostest.
- `.htaccess` ist Apache-spezifisch, andere Server brauchen andere Regeln.
- Angular CLI generiert nur Framework-Dateien — keine Serverkonfigs.

---

## Merksatz

Angular löst das Routing im Browser.  
Dein Webserver weiß davon aber nichts —  
also musst du ihm sagen: „Gib bei jeder Route index.html aus.“
