# 📖 Guide pas à pas — VideoVault avec Firebase & GitHub Pages

> **Temps estimé : 20 à 30 minutes**
> Aucune compétence technique requise. Suivez les étapes dans l'ordre.

---

## Ce que vous allez faire

| Étape | Action | Durée |
|-------|--------|-------|
| 1 | Créer un compte GitHub et un dépôt | ~5 min |
| 2 | Créer un projet Firebase (base de données gratuite) | ~8 min |
| 3 | Modifier le fichier HTML avec vos clés Firebase | ~3 min |
| 4 | Publier l'application sur GitHub Pages (URL gratuite) | ~5 min |

---

## ÉTAPE 1 — Créer un compte GitHub et un dépôt

GitHub est un service gratuit pour héberger des fichiers et des sites web.

### 1a. Créer un compte GitHub
1. Allez sur **https://github.com**
2. Cliquez sur **"Sign up"** (en haut à droite)
3. Entrez votre e-mail, choisissez un mot de passe et un nom d'utilisateur
4. Confirmez votre e-mail

### 1b. Créer un dépôt (= un dossier en ligne)
1. Une fois connecté, cliquez sur le bouton vert **"New"** (ou l'icône `+` en haut)
2. Remplissez le formulaire :
   - **Repository name** : `videovault` (ou n'importe quel nom sans espace)
   - **Description** : Ma bibliothèque YouTube *(optionnel)*
   - Cochez **"Public"** *(nécessaire pour GitHub Pages gratuit)*
   - Cochez **"Add a README file"**
3. Cliquez sur **"Create repository"** (bouton vert)

✅ Vous avez maintenant un dépôt GitHub. Notez son adresse, par exemple :
`https://github.com/VOTRE_NOM/videovault`

---

## ÉTAPE 2 — Créer un projet Firebase

Firebase est un service Google, entièrement gratuit pour un usage personnel.
Vous aurez besoin d'un compte Google (Gmail).

### 2a. Créer le projet
1. Allez sur **https://console.firebase.google.com**
2. Connectez-vous avec votre compte Google
3. Cliquez sur **"Ajouter un projet"** (ou "Create a project")
4. **Nom du projet** : tapez `videovault` → cliquez **Continuer**
5. Désactivez Google Analytics (pas utile ici) → cliquez **Créer le projet**
6. Attendez quelques secondes → cliquez **Continuer**

### 2b. Créer la base de données Firestore
1. Dans le menu gauche, cliquez sur **"Firestore Database"**
2. Cliquez sur **"Créer une base de données"**
3. Choisissez **"Commencer en mode test"** → cliquez **Suivant**
   > ⚠️ Le mode test permet un accès libre pendant 30 jours.
   > Après l'étape 2c, vous sécuriserez l'accès.
4. Choisissez un emplacement (par ex. `europe-west1` pour la France) → **Activer**

### 2c. Sécuriser la base de données (optionnel mais recommandé)
1. Dans Firestore, cliquez sur l'onglet **"Règles"**
2. Remplacez le contenu par :
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```
3. Cliquez **Publier**

> Note : Ces règles permettent à n'importe qui d'accéder à votre DB si quelqu'un connaît vos clés. Pour un usage strictement personnel sans connexion, c'est suffisant. Vous pouvez ajouter une authentification plus tard.

### 2d. Récupérer vos clés Firebase
1. Dans le menu gauche, cliquez sur l'icône ⚙️ (engrenage) → **"Paramètres du projet"**
2. Faites défiler jusqu'à la section **"Vos applications"**
3. Cliquez sur l'icône `</>` (Web) pour ajouter une application web
4. **Nom de l'application** : `videovault-web` → cliquez **"Enregistrer l'application"**
5. Vous verrez un bloc de code qui ressemble à :

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "videovault-xxxxx.firebaseapp.com",
  projectId: "videovault-xxxxx",
  storageBucket: "videovault-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

**Copiez tout ce bloc** — vous en aurez besoin à l'étape suivante.

---

## ÉTAPE 3 — Modifier le fichier HTML

### 3a. Ouvrir le fichier HTML
1. Faites un **clic droit** sur le fichier `videovault-firebase.html`
2. Choisissez **"Ouvrir avec"** → **Bloc-notes** (Windows) ou **TextEdit** (Mac)
   > Sur Mac, dans TextEdit : menu Format → "Convertir en format ordinaire"

### 3b. Trouver le bloc à modifier
Utilisez la recherche : appuyez sur **Ctrl+F** (Windows) ou **Cmd+F** (Mac)
Cherchez : `VOTRE_API_KEY`

Vous trouverez ce bloc :
```javascript
const firebaseConfig = {
  apiKey:            "VOTRE_API_KEY",
  authDomain:        "VOTRE_PROJECT_ID.firebaseapp.com",
  projectId:         "VOTRE_PROJECT_ID",
  storageBucket:     "VOTRE_PROJECT_ID.appspot.com",
  messagingSenderId: "VOTRE_SENDER_ID",
  appId:             "VOTRE_APP_ID"
};
```

### 3c. Remplacer par vos vraies clés
Sélectionnez **les 8 lignes** de `const firebaseConfig = {` jusqu'à `};` (inclus)
et remplacez-les par le bloc copié à l'étape 2d.

### 3d. Sauvegarder
Appuyez sur **Ctrl+S** (Windows) ou **Cmd+S** (Mac).

✅ **Test rapide** : double-cliquez sur le fichier HTML pour l'ouvrir dans votre navigateur.
Le bandeau rouge "Configuration requise" ne doit plus apparaître, et un point vert
"Firebase connecté" doit s'afficher en haut à droite.

---

## ÉTAPE 4 — Publier sur GitHub Pages

GitHub Pages transforme votre dépôt en site web accessible à tous (ou juste à vous).

### 4a. Uploader le fichier HTML sur GitHub
1. Allez sur votre dépôt GitHub (ex: `github.com/VOTRE_NOM/videovault`)
2. Cliquez sur **"Add file"** → **"Upload files"**
3. Glissez-déposez votre fichier `videovault-firebase.html` dans la zone
4. Renommez-le **`index.html`** (cliquez sur le nom du fichier dans la liste pour le renommer)
   > Pourquoi index.html ? C'est le nom que GitHub Pages cherche par défaut.
5. En bas, dans "Commit changes", laissez le message par défaut
6. Cliquez sur **"Commit changes"** (bouton vert)

### 4b. Activer GitHub Pages
1. Dans votre dépôt, cliquez sur **"Settings"** (onglet en haut)
2. Dans le menu gauche, cliquez sur **"Pages"**
3. Sous "Source", sélectionnez **"Deploy from a branch"**
4. Sous "Branch", choisissez **"main"** et laissez `/ (root)` → cliquez **Save**
5. Attendez 1 à 2 minutes

### 4c. Accéder à votre site
Actualisez la page Settings → Pages.
Vous verrez une bannière verte avec votre URL :
`https://VOTRE_NOM.github.io/videovault/`

🎉 **Votre bibliothèque est en ligne !** Bookmark cette URL.

---

## Utilisation quotidienne

### Ajouter une vidéo
1. Copiez l'URL d'une vidéo YouTube (ex: `https://youtube.com/watch?v=dQw4w9WgXcQ`)
2. Dans VideoVault, cliquez sur **"+ Ajouter"**
3. Collez l'URL, entrez un titre et une catégorie (ex: Musique, Tuto, Sport…)
4. Cliquez **"Enregistrer"** — la vidéo est sauvegardée dans Firebase

### Regarder une vidéo
Cliquez sur la carte → YouTube s'ouvre dans un nouvel onglet

### Supprimer une vidéo
Survolez la carte → un bouton 🗑️ apparaît en bas à droite → cliquez dessus

### Rechercher
Tapez dans la barre de recherche en haut — la liste se filtre en temps réel.
Utilisez le menu déroulant pour filtrer par catégorie.

---

## Coûts et limites (plan gratuit Firebase)

| Ressource | Limite gratuite | Votre usage estimé |
|-----------|----------------|-------------------|
| Lectures Firestore | 50 000 / jour | ~10-50 par visite |
| Écritures Firestore | 20 000 / jour | 1 par vidéo ajoutée |
| Stockage données | 1 Go | Négligeable (texte seul) |
| **Total** | **100% gratuit** | ✅ |

Pour un usage personnel, vous ne dépasserez jamais ces limites.

---

## Problèmes fréquents

**❌ "Erreur Firebase" au lancement**
→ Vérifiez que vous avez bien remplacé toutes les valeurs dans `firebaseConfig`.
→ Assurez-vous qu'il n'y a plus de guillemets vides comme `"VOTRE_API_KEY"`.

**❌ Impossible d'ajouter des vidéos**
→ Vérifiez les règles Firestore (Étape 2c). Elles doivent autoriser `write: if true`.

**❌ Le site GitHub Pages affiche une erreur 404**
→ Attendez 5 minutes après avoir activé Pages.
→ Vérifiez que le fichier s'appelle bien `index.html` (pas `videovault-firebase.html`).

**❌ Les miniatures YouTube ne s'affichent pas**
→ Normal sur certains réseaux. Les vidéos fonctionnent quand même.

---

## Pour aller plus loin (optionnel)

- **Protéger vos données** : ajoutez une authentification Firebase (Google Sign-In)
  pour que seuls vous puissiez modifier la bibliothèque.
- **Partager** : donnez l'URL GitHub Pages à des amis — ils pourront voir vos vidéos
  (mais pas les modifier, sauf si vous le paramétrez).
- **Nom de domaine personnalisé** : dans GitHub Pages Settings, vous pouvez
  associer un domaine comme `mavideotheque.fr` (le domaine est payant, ~10€/an).
