# CLAUDE.md

LoLlevaTom : PWA perso de Thomas (sœur de Paletom / Comparatom). Sa femme, au Mexique, trie par
swipe les affaires restées en France ; Thomas les ramène en décembre.

- Tout est dans `index.html` (pas de build). Firebase Firestore via CDN jsdelivr `firebase@10.12.0`.
- Langues : UI d'elle en **espagnol (Mexique)**, « Mode Tom » (`#p-admin`) en **français**.
- Backend abstrait `B` (`onItems`, `addItem`, `update`, `remove`, `getPhoto`) : Firebase si la config
  est remplie, sinon `demoBackend()` en mémoire.
- Modèle : `items/{id}` (métadonnées + `thumb` ~360 px) et `photos/{id}` (photo pleine, lazy).
  Photos compressées en JPEG côté client (`compressImage`) — doc Firestore max 1 Mo.
- Catégories : `DEFAULT_CATS` en dur, liste réelle dans Firestore `config/categories` (`{ list: [{id,label,emoji,one}] }`), éditable en mode Tom (« ＋ Nouvelle » / « ✏️ Gérer »). Suppression bloquée si la catégorie contient des objets. Statuts : `''`, `'quiero'` (swipe droite), `'tarde'` (gauche), `'fuera'` = vender o tirar (haut) ; `valise` = coché dans la valise, `fait` = vendu/jeté. Mode Tom → Valise : bouton pour remettre tous les `tarde` à `''`.
- Swipe : `renderStack` réconcilie les cartes par id (ne casse pas un drag en cours), `S.leaving`
  masque une carte avant la confirmation Firestore, `S.history` + `undo()` pour « Deshacer ».
- Mode Tom : `?tom` dans l'URL ou 5 tapes sur le logo (`localStorage.llt_admin`).
- Déploiement : GitHub Pages depuis `main` (`/LoLlevaTom/`). Bumper `CACHE` dans `sw.js` si besoin.
- Test local : `python3 -m http.server 8791` → http://localhost:8791/index.html?tom
