#  Exercice 01 — Mettre l'API inf83-api sous Git avec une v1.0.0 et un flux de branches

---

 **Projet fil rouge : INF83 – Déploiement continu DevOps - Projet pratique**

> C'est le **premier exercice** du projet fil rouge ! Vous allez poser les fondations du projet.
> Projet progressif construit au fil des exercices.

 **Dans cet exercice** : Vous recuperez l'API Node.js/Express + PostgreSQL fournie (routes /health, /ready, /items) avec ses tests Jest. Avant de parler culture DevOps, conteneurs ou CI/CD dans les chapitres suivants, on met le projet sous controle de version : depot GitHub, .gitignore propre, un flux feat/ -> main, un CHANGELOG et une premiere release taguee v1.0.0. C'est la base sur laquelle toute la chaine GitHub -> Actions -> Docker Hub -> VPS reposera.

---

##  Avant de commencer

L'**Exemple 01** (`exemples/exemple-01/exemple-01.md`) vous a montré, sur une mini lib neutre « calc », **comment** versionner un projet : git init, .gitignore, branche feature mergée, CHANGELOG.md et tags SemVer (v1.0.0 puis v1.0.1). Ici, vous appliquez exactement cette discipline au projet **INF83 – Déploiement continu DevOps - Projet pratique**. Assurez-vous d'avoir lu et suivi l'exemple avant de commencer.

---

##  Objectif

Transformer le dossier de l'API fourni en un depot Git versionne et publie sur GitHub, avec une premiere version officielle v1.0.0, un historique passant par une branche de fonctionnalite, un CHANGELOG, et une Issue decrivant un bug reproductible. (La protection de branche et la revue par PR viendront a l'Exercice 02 ; ici on pose les fondations.)

##  Prérequis

- Connaissances en développement d'applications (niveau CDA)
- Bases de la ligne de commande
- Notions de développement web et d'API
- Comptes GitHub, Docker Hub et accès à un VPS créés avant le J1

##  Scénario

Vous etes integre dans une equipe qui vous confie l'API inf83-api. Le code marche en local (`npm test` est vert) mais il n'est ni versionne ni publie. Votre mission : le mettre sous Git proprement, sortir une v1.0.0, et signaler un bug que vous avez reussi a reproduire pour que l'equipe puisse le traiter.

---

##  Étapes à réaliser

### Étape 1 — Recuperer l'API et verifier qu'elle tourne

Partir d'une base saine : le code fourni doit passer les tests avant d'etre versionne.

**Ce que vous devez faire :**

- Recuperer le dossier `inf83-api/` fourni (API Express + PostgreSQL, routes /health, /ready, /items).
- Installer les dependances avec `npm install`.
- Lancer `npm test` et verifier que les tests Jest sont verts.

 **Résultat attendu** : `npm test` se termine sans echec. Vous savez que la base est saine avant de la versionner.

 **Erreur fréquente** : Versionner un projet sans verifier les tests, puis taguer une v1.0.0 cassee. On ne tague que du code sain.

### Étape 2 — Creer le depot GitHub vide et le .gitignore

Initialiser le versionning localement en ignorant ce qui ne doit jamais entrer dans l'historique.

**Ce que vous devez faire :**

- Creer sur GitHub un depot **vide** nomme `inf83-api` (sans README, sans .gitignore, pour eviter un historique divergent au premier push).
- Dans le dossier local, initialiser git (`git init`) si ce n'est pas deja fait.
- Creer (ou completer) un `.gitignore` contenant au minimum `node_modules` et `.env`.

 **Résultat attendu** : Le depot distant existe et est vide ; le `.gitignore` local ignore node_modules et .env.

 **Erreur fréquente** : Cocher 'Add a README' a la creation du depot puis se retrouver avec un historique divergent au premier push. Creez le depot vide.

### Étape 3 — Premier commit et push sur main

Enregistrer l'etat initial du code et le publier sur la branche main.

**Ce que vous devez faire :**

- `git add .` puis verifier avec `git status` que node_modules et .env ne sont PAS dans les fichiers a commiter.
- Faire un premier commit clair (par exemple `chore: import initial de l'API inf83`).
- Relier le depot local au distant (`git remote add origin ...`) et pousser sur `main`.

 **Résultat attendu** : Le code de l'API est visible sur GitHub, branche main, avec au moins un commit. node_modules n'apparait pas.

 **Piste** : Si `git status` montre node_modules, c'est que le .gitignore n'est pas pris en compte (mauvais emplacement ou node_modules deja indexe : `git rm -r --cached node_modules`).

### Étape 4 — Creer une branche de fonctionnalite pour une petite evolution

Mettre en place le flux feat/ -> main avec un changement minuscule.

**Ce que vous devez faire :**

- Creer une branche `feat/` depuis main a jour (par exemple `feat/route-version`).
- Faire une petite evolution : par exemple exposer la version (ajouter une route GET /version qui renvoie la version de package.json) OU enrichir le README. Gardez ca minuscule.
- Commiter sur la branche.

 **Résultat attendu** : Une branche `feat/...` existe avec un commit contenant votre petite evolution.

 **Piste** : L'objectif ici est le **flux** (branche -> merge), pas la fonctionnalite. Un petit changement suffit largement.

### Étape 5 — Merger la branche dans main

Integrer la fonctionnalite dans main pour garder un historique lisible.

**Ce que vous devez faire :**

- Revenir sur `main` (`git checkout main`).
- Merger la branche (`git merge feat/route-version`).
- Supprimer la branche de feature une fois mergee (`git branch -d feat/route-version`) et pousser main.

 **Résultat attendu** : main contient votre evolution, l'historique montre l'integration de la branche, la branche de feature est supprimee.

 **Erreur fréquente** : Coder directement sur main « parce que c'est plus rapide ». On perd l'interet du flux par branche et la tracabilite du changement.

### Étape 6 — Ecrire le CHANGELOG.md

Documenter la premiere version de maniere lisible par un humain.

**Ce que vous devez faire :**

- Creer un `CHANGELOG.md` a la racine.
- Y ajouter une section `## [1.0.0]` avec les sous-sections **Added / Changed / Fixed**.
- Decrire ce que contient cette premiere version (routes /health, /ready, /items, et votre petite evolution).

 **Résultat attendu** : Un CHANGELOG.md present sur main, avec une entree 1.0.0 lisible.

 **Piste** : Inspirez-vous de Keep a Changelog. Meme si Changed/Fixed sont vides pour une v1.0.0, gardez les sous-titres pour le format.

### Étape 7 — Poser et pousser le tag v1.0.0

Materialiser la premiere release officielle avec un tag SemVer.

**Ce que vous devez faire :**

- Verifier que main est a jour et que `npm test` passe.
- Poser un tag annote : `git tag -a v1.0.0 -m "Premiere version stable de inf83-api"`.
- Pousser le tag : `git push origin v1.0.0`.

 **Résultat attendu** : Le tag v1.0.0 apparait sur GitHub (onglet Tags / Releases) et pointe sur le commit de release.

 **Erreur fréquente** : Croire que `git push` envoie aussi les tags. Il faut `git push origin v1.0.0` (ou `git push --tags`), sinon le tag reste local.

### Étape 8 — Ouvrir une Issue GitHub pour un bug reproductible

Pratiquer la gestion de probleme : signaler un bug de maniere exploitable par l'equipe.

**Ce que vous devez faire :**

- Reperer (ou imaginer) un bug **reproductible** : par exemple, appeler GET /ready quand la base PostgreSQL n'est pas joignable.
- Ouvrir une **Issue** sur GitHub avec : un titre clair, les etapes pour reproduire, le resultat observe vs attendu, et un debut de **diagnostic de 1er niveau** (logs consultes, route testee, hypothese).
- Indiquer si le probleme doit etre **escalade** (ex. bug bloquant -> assigner / labelliser).

 **Résultat attendu** : Une Issue ouverte, structuree, qu'un collegue peut reproduire sans vous poser de question.

 **Piste** : Un bon rapport de bug = quelqu'un d'autre reproduit le probleme en suivant vos etapes. Si ce n'est pas reproductible, ce n'est pas (encore) exploitable.

---

##  Vérification

Quand vous avez terminé, vérifiez que :

- [ ] Le depot GitHub `inf83-api` contient le code de l'API sur main.
- [ ] node_modules et .env ne sont PAS versionnes (.gitignore en place).
- [ ] L'historique montre une branche feat/ mergee dans main.
- [ ] Un `CHANGELOG.md` avec une entree 1.0.0 (Added/Changed/Fixed) est present.
- [ ] Le tag v1.0.0 est visible sur GitHub.
- [ ] Une Issue decrit un bug reproductible avec un diagnostic de 1er niveau.

---

## En pratique

> Sur un projet reel, le numero de version n'est pas decoratif : c'est lui qui dira plus tard quelle image Docker deployer et sur quoi faire un rollback. Prenez maintenant l'habitude de ne taguer que du code teste et merge sur main. Et soignez vos Issues : un bug bien decrit (reproductible, avec diagnostic) se corrige vite ; un « ca marche pas » fait perdre des heures a toute l'equipe.

---

##  Bonus (Facultatif mais Recommandé)

Creer une **Release GitHub** a partir du tag v1.0.0 (onglet Releases -> Draft a new release) en y collant les notes du CHANGELOG. Bonus supplementaire : adopter les Conventional Commits (feat:, fix:, chore:) pour pouvoir generer un changelog automatiquement plus tard.

---

##  Livrable attendu

URL du depot GitHub `inf83-api` montrant : le code de l'API sur main, le tag v1.0.0 dans les tags/releases, le CHANGELOG.md a la racine, l'historique avec la branche feat/ mergee, et le lien vers l'Issue du bug reproductible.

---

>  Une fois terminé, consultez la **correction** dans `corrections-exercices/correction-exercice-01/correction-exercice-01.md` pour comparer votre travail.
