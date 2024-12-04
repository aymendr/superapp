# superapp


### Contexte du projet

Vous travaillez sur un projet appelé **"SuperApp"** avec plusieurs collègues. Le projet a plusieurs fonctionnalités en développement simultanément, et vous devez gérer les branches, résoudre des conflits, maintenir un historique propre et automatiser certaines actions.

Le dépôt contient deux composants principaux :  
1. **Backend** - Un ensemble d'API écrites en Node.js.
2. **Frontend** - Un client web construit avec React.

En plus de ces deux composants, vous utilisez un sous-module Git pour inclure une bibliothèque de gestion des utilisateurs provenant d'un autre dépôt.

---

### Objectifs du lab

1. **Cloner le projet et initialiser l’environnement de travail**
2. **Gérer les branches et effectuer des fusions avancées**
3. **Utiliser `git cherry-pick` pour appliquer des commits spécifiques**
4. **Résoudre des conflits complexes**
5. **Manipuler l’historique avec `git rebase`, `git reset`, et `git reflog`**
6. **Utiliser des sous-modules pour gérer les dépendances**
7. **Configurer un hook Git pour automatiser des tests**
8. **Pousser et synchroniser les modifications avec un dépôt distant**

---

### Étape 1: Cloner le dépôt et initialiser l’environnement de travail

1. **Cloner le dépôt principal (superapp) depuis GitHub :**

   ```bash
   git clone https://github.com/votre-utilisateur/superapp.git
   cd superapp
   ```

2. **Vérifiez que vous êtes sur la branche `main` :**

   ```bash
   git checkout main
   ```

3. **Vérifiez l'état du dépôt :**

   ```bash
   git status
   ```

   Vous devriez voir un projet avec plusieurs fichiers et dossiers, dont `backend/`, `frontend/` et un sous-module.

---

### Étape 2: Gérer les branches et effectuer des fusions avancées

#### 2.1 Créer des branches pour les nouvelles fonctionnalités

1. **Créez une branche `feature/backend-auth` pour travailler sur l'authentification du backend :**

   ```bash
   git checkout -b feature/backend-auth
   ```

2. **Ajoutez une nouvelle fonctionnalité au backend (par exemple, un nouveau fichier `auth.js`) :**

   ```bash
   echo "module.exports = { authenticate: function() { return true; } }" > backend/auth.js
   git add backend/auth.js
   git commit -m "Ajout du module d'authentification au backend"
   ```

3. **Créez une branche `feature/frontend-login` pour travailler sur la page de connexion dans le frontend :**

   ```bash
   git checkout main
   git checkout -b feature/frontend-login
   ```

4. **Ajoutez une nouvelle page de connexion dans le frontend (par exemple, un fichier `LoginPage.jsx`) :**

   ```bash
   echo "import React from 'react'; export default function LoginPage() { return <div>Login</div>; }" > frontend/LoginPage.jsx
   git add frontend/LoginPage.jsx
   git commit -m "Ajout de la page de connexion dans le frontend"
   ```

#### 2.2 Fusionner les branches avec `git merge`

1. **Fusionnez `feature/frontend-login` dans `main` :**

   ```bash
   git checkout main
   git merge feature/frontend-login
   ```

   Résolvez les éventuels conflits et effectuez un commit de fusion si nécessaire.

2. **Fusionnez `feature/backend-auth` dans `main` :**

   ```bash
   git checkout main
   git merge feature/backend-auth
   ```

---

### Étape 3: Utiliser `git cherry-pick` pour appliquer des commits spécifiques

Supposons que vous ayez une branche `feature/payment` avec un commit que vous voulez appliquer sur `feature/backend-auth`.

1. **Créez une branche `feature/payment` :**

   ```bash
   git checkout -b feature/payment
   ```

2. **Ajoutez un commit sur `feature/payment` :**

   ```bash
   echo "module.exports = { processPayment: function() { return 'Payment processed'; } }" > backend/payment.js
   git add backend/payment.js
   git commit -m "Ajout du module de traitement des paiements"
   ```

3. **Vous voulez appliquer ce commit sur `feature/backend-auth`.**
   
   D'abord, identifiez le SHA du commit :

   ```bash
   git log
   ```

4. **Sur la branche `feature/backend-auth`, appliquez le commit avec `git cherry-pick` :**

   ```bash
   git checkout feature/backend-auth
   git cherry-pick <commit-sha>
   ```

5. **Résolvez les conflits (si nécessaire) et validez la fusion.**

---

### Étape 4: Résoudre des conflits complexes

1. **Simulez un conflit :**  
   Modifiez un même fichier dans deux branches différentes, par exemple, `frontend/LoginPage.jsx`.

   - Sur `feature/frontend-login`, ajoutez une ligne dans le fichier `LoginPage.jsx`.
   - Sur `feature/backend-auth`, modifiez également ce même fichier.

2. **Fusionnez les deux branches et résolvez le conflit :**

   ```bash
   git checkout main
   git merge feature/frontend-login
   git merge feature/backend-auth
   ```

   Git devrait vous signaler un conflit. Ouvrez le fichier en conflit, résolvez le conflit et effectuez un commit de résolution.

---

### Étape 5: Manipuler l’historique avec `git rebase`, `git reset`, et `git reflog`

#### 5.1 Rebaser une branche

1. **Supposons que `feature/payment` soit en retard par rapport à `main`. Utilisez `git rebase` pour appliquer les commits de `main` à `feature/payment` :**

   ```bash
   git checkout feature/payment
   git fetch origin
   git rebase origin/main
   ```

   Résolvez les conflits s'il y en a.

#### 5.2 Annuler un commit avec `git reset`

1. **Vous avez ajouté un commit indésirable, revenez à un état précédent sans perdre vos changements locaux :**

   ```bash
   git reset --soft HEAD~1
   ```

2. **Si vous souhaitez également supprimer les changements dans le répertoire de travail :**

   ```bash
   git reset --hard HEAD~1
   ```

#### 5.3 Utiliser `git reflog` pour récupérer des commits disparus

1. **Vous avez supprimé des commits par erreur. Utilisez `git reflog` pour les retrouver et revenir à un état antérieur :**

   ```bash
   git reflog
   git checkout <commit-sha>
   ```

---

### Étape 6: Utiliser des sous-modules pour gérer les dépendances

1. **Ajoutez un sous-module pour la gestion des utilisateurs (par exemple un dépôt appelé `user-management-lib`) :**

   ```bash
   git submodule add https://github.com/organisme/user-management-lib.git lib/user-management
   git submodule update --init --recursive
   ```

2. **Modifiez votre code pour utiliser la bibliothèque du sous-module (par exemple, intégrer la gestion des utilisateurs dans le backend).**

3. **Pour pousser vos modifications, n'oubliez pas de pousser le sous-module :**

   ```bash
   git add lib/user-management
   git commit -m "Ajout du sous-module pour la gestion des utilisateurs"
   git push origin main
   ```

---

### Étape 7: Configurer un hook Git pour automatiser des tests

1. **Créez un hook `pre-commit` pour exécuter les tests avant chaque commit :**

   Allez dans `.git/hooks/pre-commit` et créez un script comme suit :

   ```bash
   #!/bin/sh
   npm run test
   if [ $? -ne 0 ]; then
     echo "Tests échoués, commit annulé."
     exit 1
   fi
   ```

2. **Rendez ce script exécutable :**

   ```bash
   chmod +x .git/hooks/pre-commit
   ```

---

### Étape 8: Pousser et synchroniser les modifications avec un dépôt distant

1. **Poussez toutes vos modifications sur le dépôt distant :**

   ```bash
   git push origin main
   ```

2. **Si vous avez des branches distantes, assurez-vous qu'elles sont bien mises à jour :**

   ```bash
   git push origin feature/backend-auth
   git push origin feature/frontend-login
   ```

---

### Conclusion

Ce lab couvre une série de tâches avancées dans Git : gestion des branches, résolution de conflits, manipulation de l’historique, utilisation des sous-modules, automatisation des tests, et gestion des hooks. Ces compétences vous permettront de gérer des projets complexes avec plusieurs développeurs et de maintenir un historique propre tout en intégrant
