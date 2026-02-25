[Zurück zur Übersicht](../README.md)

# Firebase Authentication: Registrieren, Anmelden, Profil aktualisieren und aktuellen Nutzer abrufen

## Inhaltsverzeichnis

- [Firebase Authentication: Registrieren, Anmelden, Profil aktualisieren und aktuellen Nutzer abrufen](#firebase-authentication-registrieren-anmelden-profil-aktualisieren-und-aktuellen-nutzer-abrufen)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Zweck](#zweck)
  - [Voraussetzungen](#voraussetzungen)
  - [Firebase Auth initialisieren](#firebase-auth-initialisieren)
  - [Neue Nutzer registrieren](#neue-nutzer-registrieren)
  - [Vorhandene Nutzer anmelden](#vorhandene-nutzer-anmelden)
  - [Derzeit angemeldeten Nutzer abrufen](#derzeit-angemeldeten-nutzer-abrufen)
    - [Direktzugriff über `auth.currentUser`](#direktzugriff-über-authcurrentuser)
    - [Empfohlen: Auth-Status beobachten mit `onAuthStateChanged`](#empfohlen-auth-status-beobachten-mit-onauthstatechanged)
  - [Profil eines Nutzers aktualisieren](#profil-eines-nutzers-aktualisieren)
  - [Erstanmeldung erkennen (First Sign-In)](#erstanmeldung-erkennen-first-sign-in)
  - [Nach der ersten Anmeldung: Next Steps (offizielle Empfehlung)](#nach-der-ersten-anmeldung-next-steps-offizielle-empfehlung)
  - [Typischer Ablauf im UI](#typischer-ablauf-im-ui)
  - [Fehlerquellen und Troubleshooting](#fehlerquellen-und-troubleshooting)
  - [Best Practices](#best-practices)
  - [Siehe auch](#siehe-auch)

## Zweck

Firebase Authentication ermöglicht die Anmeldung und Verwaltung von Nutzern.

Zusätzlich trennt Firebase klar zwischen:

- **Authentifizierung** (Wer ist der Nutzer?)
- **Autorisierung** über Security Rules (Was darf der Nutzer lesen/schreiben?)

Diese Doku zeigt den typischen Web-Flow mit E-Mail/Passwort auf Basis der
offiziellen Firebase-Dokumentation.

## Voraussetzungen

- Firebase-Projekt mit aktivierter **E-Mail/Passwort**-Anmeldung in der Firebase
  Console
- Firebase SDK im Projekt installiert
- Firebase App initialisiert

---

## Firebase Auth initialisieren

```typescript
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";

const firebaseConfig = {
  // ...deine Firebase Config...
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
```

---

## Neue Nutzer registrieren

Offizielle Methode: `createUserWithEmailAndPassword(auth, email, password)`

```typescript
import { createUserWithEmailAndPassword } from "firebase/auth";

async function signUp(email: string, password: string) {
  try {
    const userCredential = await createUserWithEmailAndPassword(
      auth,
      email,
      password,
    );
    const user = userCredential.user;
    console.log("Nutzer registriert:", user.uid);
  } catch (error) {
    console.error("Fehler bei Registrierung:", error);
  }
}
```

**Wichtig laut offizieller Doku:**

- Passwort muss die Firebase-Anforderungen erfüllen
- Bei Erfolg bekommst du ein `UserCredential` inkl. `user`

**Was dabei passiert:**  
Firebase erstellt ein neues Auth-Konto, meldet den Nutzer in der Regel direkt an
und setzt den Auth-Status der App auf „eingeloggt“.

---

## Vorhandene Nutzer anmelden

Offizielle Methode: `signInWithEmailAndPassword(auth, email, password)`

```typescript
import { signInWithEmailAndPassword } from "firebase/auth";

async function signIn(email: string, password: string) {
  try {
    const userCredential = await signInWithEmailAndPassword(
      auth,
      email,
      password,
    );
    console.log("Erfolgreich angemeldet:", userCredential.user.uid);
  } catch (error) {
    console.error("Fehler bei Anmeldung:", error);
  }
}
```

**Was dabei passiert:**  
Firebase prüft die Credentials serverseitig. Bei Erfolg erhältst du wieder ein
`UserCredential`, und der Nutzer ist im Client authentifiziert.

---

## Derzeit angemeldeten Nutzer abrufen

### Direktzugriff über `auth.currentUser`

```typescript
const user = auth.currentUser;
if (user) {
  console.log("Aktueller Nutzer:", user.uid, user.email);
} else {
  console.log("Kein Nutzer angemeldet");
}
```

### Empfohlen: Auth-Status beobachten mit `onAuthStateChanged`

```typescript
import { onAuthStateChanged } from "firebase/auth";

onAuthStateChanged(auth, (user) => {
  if (user) {
    console.log("Nutzer angemeldet:", user.uid);
  } else {
    console.log("Nutzer abgemeldet");
  }
});
```

**Hinweis:**  
`currentUser` kann beim initialen App-Start kurzfristig `null` sein, bis
Firebase den Zustand geladen hat.

**Empfehlung aus der Praxis:**  
UI-Logik (z. B. geschützte Bereiche) immer an `onAuthStateChanged` hängen, nicht
nur an einen einmaligen `currentUser`-Check.

---

## Profil eines Nutzers aktualisieren

Offizielle Methode: `updateProfile(user, { displayName, photoURL })`

```typescript
import { updateProfile } from "firebase/auth";

async function updateUserProfile(displayName: string, photoURL?: string) {
  if (!auth.currentUser) {
    console.log("Kein eingeloggter Nutzer vorhanden.");
    return;
  }

  try {
    await updateProfile(auth.currentUser, {
      displayName,
      photoURL: photoURL ?? null,
    });
    console.log("Profil erfolgreich aktualisiert");
  } catch (error) {
    console.error("Fehler bei Profil-Update:", error);
  }
}
```

**Was dabei passiert:**  
Es werden nur die angegebenen Profilfelder aktualisiert. Voraussetzung ist ein
authentifizierter Nutzer (`auth.currentUser`).

---

## Erstanmeldung erkennen (First Sign-In)

Für den ersten Login kann `isNewUser` aus den Zusatzinformationen verwendet
werden.

```typescript
import {
  signInWithEmailAndPassword,
  getAdditionalUserInfo,
} from "firebase/auth";

async function signInAndCheckFirstLogin(email: string, password: string) {
  try {
    const credential = await signInWithEmailAndPassword(auth, email, password);
    const additionalInfo = getAdditionalUserInfo(credential);
    const isFirstLogin = additionalInfo?.isNewUser === true;

    if (isFirstLogin) {
      console.log("Erstanmeldung erkannt: Onboarding starten.");
      // z.B. Willkommensdialog, Initialdaten anlegen, Profil vervollständigen
    } else {
      console.log("Wiederkehrender Nutzer.");
    }
  } catch (error) {
    console.error("Anmeldung fehlgeschlagen:", error);
  }
}
```

**Praxis-Tipp:**  
Beim ersten Login direkt Next Steps ausführen (z.B. Onboarding, Profilergänzung,
optional E-Mail-Verifizierung).

---

## Nach der ersten Anmeldung: Next Steps (offizielle Empfehlung)

Nach erfolgreichem Sign-up/Sign-in empfiehlt die Firebase-Doku typischerweise:

- **E-Mail-Adresse verifizieren** (z. B. mit `sendEmailVerification`)
- **Passwort-Reset anbieten** (z. B. mit `sendPasswordResetEmail`)
- **Auth-Status zentral beobachten** (`onAuthStateChanged`)
- **App-Routen und Datenzugriff absichern** (Security Rules + Guard-Logik)

Damit wird der Login-Flow robuster und nutzerfreundlicher, besonders beim ersten
Start eines neuen Kontos.

---

## Typischer Ablauf im UI

1. Nutzer registriert sich über Formular (`createUserWithEmailAndPassword`)
2. Nutzer meldet sich an (`signInWithEmailAndPassword`)
3. App hört auf Auth-Status (`onAuthStateChanged`)
4. Profil wird bei Bedarf aktualisiert (`updateProfile`)
5. Bei Erstanmeldung wird Onboarding gestartet (`isNewUser`)

---

## Fehlerquellen und Troubleshooting

- **`auth/invalid-email`**: E-Mail-Format ungültig
- **`auth/weak-password`**: Passwort zu schwach
- **`auth/email-already-in-use`**: Konto existiert bereits
- **`auth/invalid-credential` / `auth/user-not-found` / `auth/wrong-password`**:
  Login-Daten falsch
- **`currentUser === null` direkt nach Start**: auf `onAuthStateChanged` warten
- **`updateProfile` ohne Login**: nur möglich, wenn `auth.currentUser` gesetzt
  ist

---

## Best Practices

- Auth-Status zentral über `onAuthStateChanged` verwalten
- Fehlercodes auswerten und nutzerfreundliche Meldungen anzeigen
- Profil-Updates nur bei eingeloggtem Nutzer ausführen
- Erstanmeldung gezielt für Onboarding nutzen
- Sicherheitsregeln (Firestore/Storage Rules) immer an Auth koppeln

---

## Siehe auch

- [Firebase: Neue Nutzer registrieren (Web)](https://firebase.google.com/docs/auth/web/start?hl=de#sign_up_new_users)
- [Firebase: Vorhandene Nutzer anmelden (Web)](https://firebase.google.com/docs/auth/web/start?hl=de#sign_in_existing_users)
- [Firebase: Derzeit angemeldeten Nutzer abrufen](https://firebase.google.com/docs/auth/web/manage-users?hl=de#get_the_currently_signed-in_user)
- [Firebase: Profil eines Nutzers aktualisieren](https://firebase.google.com/docs/auth/web/manage-users?hl=de#update_a_users_profile)
- [Firebase: Password Auth – Next Steps](https://firebase.google.com/docs/auth/web/password-auth?hl=de#next_steps)
