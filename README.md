# COPA CONTABLE 2026

Torneo educativo online de Sistema de Información Contable.
Escuela N.º 4-151 Dr. Benito Marianetti · 3.º Año.

## Archivos

- `index1.html` — aplicación completa (single file).
- `vercel.json` — configuración de despliegue en Vercel.

## Despliegue en Vercel

1. Subí este repositorio a GitHub (o usá Vercel CLI).
2. En Vercel: **New Project** → importá el repo.
3. Framework Preset: **Other** (sitio estático).
4. Build Command: (vacío). Output Directory: (vacío / raíz).
5. Deploy. La URL pública quedará disponible (ej. `https://tu-proyecto.vercel.app`).

## Base de datos (Firebase Firestore) — recomendada para multi-dispositivo

Vercel solo aloja archivos estáticos; para sincronizar varios dispositivos en tiempo real se necesita Firebase.

### Pasos

1. Crear proyecto en https://console.firebase.google.com/
2. **Firestore Database** → Crear base de datos (modo prueba o producción).
3. **Authentication** → habilitar método (Anónimo o Email/Contraseña).
4. Configuración del proyecto → "Tus apps" → Web → copiar `firebaseConfig`.
5. En `index1.html`, antes del `<script>` final (o editando la constante), definir:

```js
window.FIREBASE_CONFIG = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_PROYECTO.firebaseapp.com",
  projectId: "TU_PROYECTO",
  storageBucket: "TU_PROYECTO.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

6. Redesplegar en Vercel.

### Reglas de seguridad de Firestore (copiar en Firebase Console → Firestore → Reglas)

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /tournaments/{tid} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

> Ajustá las reglas según tus necesidades. Por defecto se deniega todo salvo torneos (lectura pública, escritura solo autenticados).

## Uso

- **Docente**: abrir la URL → "Panel docente" → contraseña por defecto `docente2026` (cambiar en el código para producción) → crear sala → abrir inscripción.
- **Equipos**: abrir la URL → "Ingresar" → código de sala → registrar nombre del equipo.

## Limitaciones conocidas

- Modo demo (sin Firebase): funciona en una sola pestaña/dispositivo.
- La validación de respuestas y puntuación en el cliente es para demo; para producción usar Cloud Functions.
- La contraseña docente es client-side.
- No se incluyen datos personales de alumnos.
```

