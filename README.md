# LoLlevaTom ✈️🇲🇽

PWA pour trier à distance les affaires restées en France : Tom prend tout en photo, elle swipe
par catégorie (**→ ¡Lo quiero!** / **← Más tarde**), et Tom coche la valise avant le voyage de décembre.

- Interface d'elle : espagnol. Mode Tom (ajout de photos, objets, valise) : français.
- **Mode Tom** : ouvrir l'app avec `?tom` à la fin de l'URL, ou taper 5 fois sur le logo.

## Brancher Firebase (5 min)

1. https://console.firebase.google.com → **Ajouter un projet** → nom `lollevatom` (Analytics : inutile).
2. **Créer → Firestore Database** → *Créer une base de données* → région `eur3` (ou proche) → mode **production**.
3. Onglet **Règles** de Firestore, coller puis **Publier** :
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} { allow read, write: if true; }
     }
   }
   ```
4. ⚙️ **Paramètres du projet** → *Vos applications* → icône **Web `</>`** → nom `LoLlevaTom` → copier l'objet `firebaseConfig`.
5. Coller les 6 valeurs dans `index.html`, bloc `=== CONFIG FIREBASE ===`.

Tant que la config n'est pas remplie, l'app tourne en **mode démo** (données en mémoire, rien n'est sauvegardé).

## Données (Firestore)

- `items/{id}` : `{ cat, nom, thumb, statut: '' | 'quiero' | 'tarde', valise, createdAt, decidedAt }`
- `photos/{id}` : `{ data }` — photo pleine taille (JPEG ~1200 px), chargée à la demande.

Les photos sont compressées dans le navigateur et stockées dans Firestore (pas besoin de Firebase Storage / forfait Blaze).
