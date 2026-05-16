# 📖 Guide VideoVault v2 — Installation complète

> **Temps estimé : 25 à 35 minutes**
> Suivez les étapes dans l'ordre. Chaque étape est indépendante.

---

## Ce que vous allez faire

| Étape | Action | Durée |
|-------|--------|-------|
| 1 | Créer un projet Firebase | ~8 min |
| 2 | Activer l'authentification email/mot de passe | ~3 min |
| 3 | Créer la base de données Firestore | ~5 min |
| 4 | Configurer les règles de sécurité | ~3 min |
| 5 | Coller vos clés dans le fichier HTML | ~2 min |
| 6 | Publier sur GitHub Pages | ~8 min |

---

## ÉTAPE 1 — Créer le projet Firebase

1. Allez sur **https://console.firebase.google.com**
2. Connectez-vous avec votre compte Google (Gmail)
3. Cliquez sur **"Ajouter un projet"**
4. Nom du projet : `videovault` → **Continuer**
5. Désactivez Google Analytics (inutile ici) → **Créer le projet**
6. Patientez 10-15 secondes → **Continuer**

---

## ÉTAPE 2 — Activer l'authentification email/mot de passe

C'est ce qui permet à chaque utilisateur d'avoir son propre compte et sa propre bibliothèque.

1. Dans le menu gauche, cliquez sur **"Authentication"** (icône 👤)
2. Cliquez sur **"Commencer"**
3. Dans l'onglet **"Sign-in method"**, cliquez sur **"E-mail/Mot de passe"**
4. Activez le premier interrupteur **"Activer"** → **Enregistrer**

✅ L'authentification est activée.

---

## ÉTAPE 3 — Créer la base de données Firestore

1. Dans le menu gauche, cliquez sur **"Firestore Database"**
2. Cliquez sur **"Créer une base de données"**
3. Choisissez **"Commencer en mode production"** *(les règles de sécurité seront définies à l'étape 4)*
4. Choisissez un emplacement : **`europe-west1`** (France/Belgique) → **Activer**
5. Attendez que la base soit prête (10-20 secondes)

---

## ÉTAPE 4 — Configurer les règles de sécurité Firestore

Les règles déterminent qui peut lire ou écrire quoi. C'est la partie la plus importante pour la sécurité.

1. Dans Firestore, cliquez sur l'onglet **"Règles"**
2. Effacez tout le contenu actuel
3. Copiez-collez exactement ce bloc :

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Chaque utilisateur accède uniquement à ses propres données
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }

    // Les codes de partage : lisibles par tous,
    // créables/supprimables uniquement par leur propriétaire
    match /shareCodes/{code} {
      allow read: if true;
      allow create: if request.auth != null
                    && request.resource.data.ownerUid == request.auth.uid;
      allow delete: if request.auth != null
                    && resource.data.ownerUid == request.auth.uid;
    }
  }
}
```

4. Cliquez sur **"Publier"**

✅ Vos données sont sécurisées : chaque utilisateur ne voit que sa propre bibliothèque.

---

## ÉTAPE 5 — Récupérer vos clés et configurer le fichier HTML

### 5a. Ajouter une application Web dans Firebase

1. Dans le menu gauche, cliquez sur l'icône ⚙️ (engrenage) → **"Paramètres du projet"**
2. Faites défiler jusqu'à **"Vos applications"**
3. Cliquez sur l'icône **`</>`** (Web)
4. Nom de l'application : `videovault-web` → **"Enregistrer l'application"**
5. Vous voyez apparaître un bloc comme celui-ci :

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXX",
  authDomain: "videovault-12345.firebaseapp.com",
  projectId: "videovault-12345",
  storageBucket: "videovault-12345.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890"
};
```

**Copiez tout ce bloc** (de `const` jusqu'au `};`).

### 5b. Modifier le fichier HTML

1. Faites un **clic droit** sur `videovault-v2.html` → **Ouvrir avec** → **Bloc-notes** (Windows) ou **TextEdit** (Mac)
   - Sur Mac : menu Format → "Convertir en format ordinaire"

2. Appuyez sur **Ctrl+F** (Windows) ou **Cmd+F** (Mac), cherchez :
   ```
   VOTRE_API_KEY
   ```

3. Vous trouvez ce bloc à remplacer :
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

4. Sélectionnez **ces 8 lignes entières** et remplacez-les par le bloc copié à l'étape 5a.

5. Sauvegardez : **Ctrl+S** (Windows) ou **Cmd+S** (Mac).

### 5c. Tester en local

Double-cliquez sur le fichier HTML. Un écran de connexion doit apparaître.
Créez un compte avec votre email → vous accédez à votre bibliothèque.

---

## ÉTAPE 6 — Publier sur GitHub Pages

### 6a. Créer un compte et un dépôt GitHub

1. Allez sur **https://github.com** → **"Sign up"**
2. Entrez votre email, un mot de passe, un nom d'utilisateur → confirmez l'email
3. Une fois connecté, cliquez sur **"New"** (bouton vert) ou le `+` en haut
4. Remplissez :
   - **Repository name** : `videovault`
   - Cochez **"Public"** *(obligatoire pour Pages gratuit)*
   - Cochez **"Add a README file"**
5. Cliquez **"Create repository"**

### 6b. Uploader le fichier HTML

1. Dans votre dépôt, cliquez **"Add file"** → **"Upload files"**
2. Glissez votre fichier `videovault-v2.html` dans la zone
3. **Important** : avant de valider, le fichier doit s'appeler `index.html`.
   Si GitHub affiche `videovault-v2.html`, cliquez sur son nom dans la liste pour le renommer en `index.html`.
4. En bas, cliquez **"Commit changes"** (bouton vert)

### 6c. Activer GitHub Pages

1. Cliquez sur **"Settings"** (onglet en haut du dépôt)
2. Menu gauche → **"Pages"**
3. Sous "Source" : sélectionnez **"Deploy from a branch"**
4. Sous "Branch" : choisissez **"main"** et **"/ (root)"** → **Save**
5. Attendez 1 à 3 minutes, puis actualisez la page

Votre URL sera : `https://VOTRE_NOM.github.io/videovault/`

### 6d. Autoriser votre domaine GitHub dans Firebase

Firebase bloque par défaut les connexions depuis des domaines inconnus.

1. Retournez dans **Firebase Console** → **Authentication**
2. Onglet **"Settings"** → section **"Domaines autorisés"**
3. Cliquez **"Ajouter un domaine"**
4. Entrez : `VOTRE_NOM.github.io` *(remplacez VOTRE_NOM par votre vrai nom d'utilisateur GitHub)*
5. Cliquez **"Ajouter"**

✅ Votre site est maintenant en ligne et pleinement fonctionnel.

---

## Guide d'utilisation — Toutes les fonctionnalités

### 🔐 Compte utilisateur

- **Créer un compte** : cliquez "Créer un compte" sur l'écran de connexion
- **Se connecter** : entrez email + mot de passe
- Chaque compte a sa propre bibliothèque, totalement isolée des autres

---

### 📁 Dossiers (jusqu'à 3 niveaux)

**Créer un dossier**
→ Cliquez sur **"+ Dossier"** dans la barre latérale gauche
→ Donnez un nom et choisissez éventuellement un dossier parent

**Structure possible :**
```
📁 Tout
├── 📂 Musique
│   ├── 📂 Jazz
│   └── 📂 Rock
├── 📂 Tutoriels
│   └── 📂 Python
└── 📂 Sport
```

**Renommer un dossier**
→ Survolez le dossier dans la barre latérale → cliquez **"⋯"** → entrez le nouveau nom

**Naviguer**
→ Cliquez sur un dossier dans la barre latérale
→ Le fil d'Ariane (en haut du contenu) vous indique où vous êtes
→ Cliquez sur n'importe quel niveau du fil pour remonter

---

### 🎬 Vidéos

**Ajouter une vidéo**
→ Cliquez **"+ Vidéo"** → remplissez le formulaire :
- URL YouTube (formats supportés : vidéo normale, Short, lien court youtu.be)
- Titre
- Date de publication *(pour le tri)*
- Catégorie / tag
- Dossier de destination
- Notes personnelles *(optionnel)*

**Modifier une vidéo**
→ Survolez la carte → cliquez l'icône ✏️ → modifiez → "Mettre à jour"

**Déplacer une vidéo vers un autre dossier**
→ Survolez la carte → cliquez **"⋯"** → "Déplacer vers…" → choisissez le dossier → "Déplacer"

**Supprimer une vidéo**
→ Survolez la carte → cliquez **"⋯"** → "Supprimer"

**Ouvrir une vidéo**
→ Cliquez sur la miniature → YouTube s'ouvre dans un nouvel onglet

---

### 🔃 Tris

Utilisez le menu déroulant dans la barre d'outils :

| Option | Effet |
|--------|-------|
| Ajout ↓ | Les plus récemment ajoutées en premier |
| Ajout ↑ | Les plus anciennes d'abord |
| Publication ↓ | Les vidéos les plus récentes d'abord |
| Publication ↑ | Les vidéos les plus anciennes d'abord |
| Titre A→Z | Ordre alphabétique |
| Titre Z→A | Ordre alphabétique inverse |

> La date de publication doit être renseignée lors de l'ajout pour que les tris par publication fonctionnent.

---

### 📤 Export JSON

→ Cliquez sur l'icône ⬇️ (téléchargement) en haut à droite
→ Un fichier `videovault-export-AAAA-MM-JJ.json` est téléchargé
→ Il contient tous vos dossiers et vidéos, lisible par n'importe quel tableur ou éditeur de texte

Format du fichier exporté :
```json
{
  "exportedAt": "2026-01-15T10:30:00.000Z",
  "user": "vous@email.com",
  "folders": [ { "id": "...", "name": "Musique", "parentId": "" } ],
  "videos":  [ { "id": "...", "title": "...", "url": "...", "folderId": "..." } ]
}
```

---

### 🔗 Partage avec code à usage unique

Vous pouvez partager tout ou une partie de votre bibliothèque sans donner votre mot de passe.

**Créer un code de partage**
1. Cliquez sur l'icône de partage (🔗) en haut à droite
2. Choisissez la portée :
   - **Toute la bibliothèque** : partage toutes vos vidéos
   - **Un dossier précis** : sélectionnez le dossier (et ses sous-dossiers)
3. Ajoutez une note optionnelle (ex: "pour Marie")
4. Cliquez **"Générer un code"**
5. Un code de 8 caractères apparaît (ex: `AB12CD34`) → cliquez pour copier

**Partager le code**
→ Envoyez le code par message, email, etc.
→ La personne va sur votre URL GitHub Pages, clique sur "Code partagé", entre le code

**Ce que voit la personne avec le code**
→ Elle peut consulter et lire les vidéos partagées
→ Elle ne peut rien modifier, ajouter ou supprimer
→ Elle n'a pas besoin de créer un compte

**Révoquer un code**
1. Cliquez sur l'icône de partage 🔗
2. Dans la liste "Codes actifs", cliquez sur la ✕ à droite du code
3. Le code est immédiatement invalidé

---

## Problèmes fréquents

**❌ "Firebase non configuré" au lancement**
→ Vérifiez que vous avez remplacé les 8 lignes `firebaseConfig` avec vos vraies clés.
→ Assurez-vous qu'il ne reste plus de `"VOTRE_API_KEY"` dans le fichier.

**❌ "auth/unauthorized-domain" lors de la connexion**
→ Vous avez oublié l'étape 6d. Ajoutez `VOTRE_NOM.github.io` dans Firebase Authentication → Settings → Domaines autorisés.

**❌ "permission-denied" lors d'une opération**
→ Vérifiez les règles Firestore (étape 4). Copiez-collez exactement le bloc fourni.

**❌ Les codes de partage ne fonctionnent pas**
→ Vérifiez que les règles Firestore contiennent bien le bloc `shareCodes`.

**❌ Le site GitHub Pages affiche une erreur 404**
→ Attendez 5 minutes après activation.
→ Le fichier doit s'appeler exactement `index.html`.

**❌ "auth/invalid-credential" à la connexion**
→ Email ou mot de passe incorrect. Utilisez "Créer un compte" si c'est la première connexion.

---

## Mettre à jour l'application

Si vous recevez une nouvelle version du fichier HTML :
1. Ouvrez-le dans le Bloc-notes et copiez vos clés Firebase dedans (étape 5b)
2. Retournez sur GitHub → votre dépôt → cliquez sur `index.html`
3. Cliquez sur l'icône ✏️ (crayon) → effacez tout → collez le nouveau contenu
4. Cliquez **"Commit changes"**
5. Attendez 1-2 minutes → actualisez votre URL

---

## Coûts Firebase (plan Spark = gratuit)

| Ressource | Limite gratuite | Usage typique |
|-----------|----------------|---------------|
| Utilisateurs Auth | 10 000 | Usage personnel |
| Lectures Firestore | 50 000 / jour | ~50-200 par session |
| Écritures Firestore | 20 000 / jour | 1-5 par vidéo ajoutée |
| Stockage Firestore | 1 Go | Texte seulement → négligeable |
| **Coût total** | **0 €** | ✅ |

Pour un usage personnel ou familial, vous ne dépasserez jamais ces limites.
