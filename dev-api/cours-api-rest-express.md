---
tags:
  - cours
  - dev-api
  - nodejs
  - express
  - rest
  - http
date: 2026-09-21
---

# Dev API — Architecture REST, Node.js & Express

## 1. Fondamentaux du Web & de l'Environnement

* **Node.js :** Environnement d'exécution (*runtime*) JavaScript asynchrone côté serveur fondé sur le moteur V8 de Google Chrome. Permet d'exécuter du code JavaScript en dehors d'un navigateur web.
* **npm (*Node Package Manager*) :** Gestionnaire de paquets officiel de Node.js, utilisé pour installer des dépendances, des bibliothèques et orchestrer les scripts d'exécution du projet[cite: 1, 2].
* **Express :** Framework minimaliste et flexible pour Node.js simplifiant le routage, la création d'API REST et la gestion du cycle de vie des requêtes HTTP via des middlewares[cite: 3].
* **Protocole HTTP :** Modèle **Client-Serveur**[cite: 1] :
  * **Client (actif) :** envoie une requête (*Request*) vers un serveur pour demander une ressource ou exécuter une action[cite: 1].
  * **Serveur (passif / réactif) :** écoute sur un port donné, traite la demande reçue et renvoie une réponse (*Response*)[cite: 1].
  * **Stateless (sans état) :** chaque cycle requête/réponse est autonome. Le serveur ne conserve aucun état transactionnel d'une requête à l'autre sans mécanisme tiers (sessions, cookies, tokens JWT).

---

## 2. Anatomie d'une Requête HTTP

Une requête HTTP REST (*Representational State Transfer*) se compose de 4 piliers[cite: 1] :
* **URL / Endpoint :** adresse de la ressource ciblée (ex. `http://localhost:3000/user`).
* **Méthode HTTP (Verbe) :** nature de l'action à réaliser sur la ressource[cite: 1].
* **Headers (En-têtes) :** métadonnées d'échange (`Content-Type: application/json`, `Authorization: Bearer <token>`, etc.).
* **Body (Corps) :** données transmises au format JSON lors d'opérations d'écriture (`POST`, `PUT`, `PATCH`).

---

## 3. Méthodes HTTP & Opérations CRUD

| Méthode | Action CRUD | Description | Idempotente ? |
| :--- | :--- | :--- | :---: |
| **`GET`**[cite: 1] | Read | Récupération de données sans altération d'état côté serveur[cite: 1]. | **Oui** |
| **`POST`**[cite: 1] | Create | Création d'une nouvelle ressource[cite: 1]. | **Non** |
| **`PUT`**[cite: 1] | Update (complet) | Remplacement complet de la ressource ciblée par les données fournies[cite: 1]. | **Oui** |
| **`PATCH`**[cite: 1] | Update (partiel) | Modification ciblée de certains champs uniquement[cite: 1]. | **Non** |
| **`DELETE`**[cite: 1] | Delete | Suppression définitive de la ressource[cite: 1]. | **Oui** |

> [!NOTE] Différence clé : PUT vs PATCH
> * `PUT /user/1` remplace l'intégralité du document (les champs omis dans le body sont écrasés ou supprimés).
> * `PATCH /user/1` met à jour seulement les clés fournies sans toucher au reste de l'objet.

---

## 4. Codes de Statut HTTP (*HTTP Status Codes*)

Le serveur transmet ces codes numériques dans l'en-tête de réponse via `res.status(code)`[cite: 13] :

### Familles de codes
* **`1xx` (Informationnel) :** requête en cours de traitement.
* **`2xx` (Succès) :** requête reçue, comprise et acceptée.
* **`3xx` (Redirection) :** action supplémentaire requise.
* **`4xx` (Erreur Client) :** anomalie côté émetteur (syntaxe, authentification, ressource introuvable).
* **`5xx` (Erreur Serveur) :** défaillance interne du serveur.

### Tableau récapitulatif

| Code | Libellé standard | Contexte d'usage en API REST |
| :--- | :--- | :--- |
| **`200`** | `OK` | Succès standard (`GET`, `PUT`, `DELETE` avec message de confirmation)[cite: 13]. |
| **`201`** | `Created` | Création confirmée d'une nouvelle ressource (`POST`)[cite: 13]. |
| **`204`** | `No Content` | Succès sans données renvoyées (`DELETE` silencieux)[cite: 13, 21]. |
| **`400`** | `Bad Request` | Requête invalide (JSON mal formé, paramètre obligatoire absent). |
| **`401`** | `Unauthorized` | Authentification requise ou jeton expiré. |
| **`403`** | `Forbidden` | Authentifié mais droits d'accès insuffisants sur la ressource. |
| **`404`** | `Not Found` | Route ou ressource inexistante. |
| **`500`** | `Internal Server Error` | Crash serveur ou exception non interceptée. |

> [!WARNING] Règle du code 204
> La spécification HTTP interdit d'inclure un corps (*body*) avec un code `204 No Content`[cite: 13]. Même si le code de cours fait `res.status(204).json(...)`[cite: 21], la norme stipule que si l'on souhaite renvoyer un message texte ou JSON au client, il faut employer le code `200 OK`[cite: 13].

---

## 5. Initialisation & Configuration Node.js

```bash
# Initialisation avec validation automatique des options par défaut
npm init -y

# Installation du framework Express
npm install express
# Raccourci équivalent : npm i express
```

* **`package.json` :** Manifeste du projet (métadonnées, scripts d'exécution et liste des dépendances)[cite: 2].
  * Ex. `"express": "^5.2.1"`[cite: 4] : le symbole caret (`^`)[cite: 4] autorise automatiquement les montées de versions mineures et de correctifs (SemVer : `Majeure.Mineure.Patch`)[cite: 4].
* **`node_modules/` :** Dossier contenant le code source compilé d'Express et de ses sous-dépendances[cite: 3].
* **`package-lock.json` :** Fige l'arbre exact des versions installées pour garantir la reproductibilité sur n'importe quel environnement.

> [!IMPORTANT] Règle d'or Git
> Ne jamais versionner le dossier `node_modules/`. Crée un fichier `.gitignore` à la racine :
> ```gitignore
> node_modules/
> .env
> ```

---

## 6. Architecture Modulaire du Projet

Organisation exacte du dépôt (`2esgi-api`)[cite: 21] :

```text
2esgi-api/
├── .vscode/
│   └── launch.json
├── node_modules/
├── route/
│   ├── product.js
│   └── user.js
├── .gitignore
├── app.js
├── package.json
├── package-lock.json
└── server.js
```

---

## 7. Fichiers Sources

### `.vscode/launch.json` (Configuration de Débogage VS Code)
Permet de poser des points d'arrêt (*breakpoints*) et d'exécuter `server.js` avec `F5`[cite: 21] :
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "skipFiles": [
        "<node_internals>/**"
      ],
      "program": "${workspaceFolder}\\server.js"
    }
  ]
}
```

### `server.js` (Couche Réseau & Démarrage)
```javascript
const http = require('http');
const app = require('./app.js');

// Création du serveur HTTP qui s'appuie sur la configuration Express
const server = http.createServer(app);

// Écoute de l'événement système émis à l'ouverture effective du port
server.on('listening', () => {
  console.log('Serveur en écoute sur le port : ' + 3000);
});

server.listen(3000);
```

### `app.js` (Configuration Centrale & Middlewares)
```javascript
const express = require('express');
const app = express();

const userRouter = require('./route/user.js');
const productRouter = require('./route/product.js');

// Middleware global indispensable pour parser le JSON entrant dans req.body
app.use(express.json());

// Déclaration et préfixage des routeurs modulaires
app.use('/user', userRouter);
app.use('/product', productRouter);

// Exportation CommonJS de l'application
module.exports = app;
```

> [!CAUTION] Exportation CommonJS
> En Node.js, `return app;` à la racine d'un fichier déclenche une erreur de syntaxe (`SyntaxError: Illegal return statement`)[cite: 6]. Utilise impérativement `module.exports = app;`[cite: 8].

### `route/user.js` (Endpoints Utilisateurs & Paramètres)
```javascript
const express = require('express');
const router = express.Router();

// GET /user/ : Récupération de la liste complète
router.get('/', (req, res, next) => {
  res.status(200).json('Récupération liste utilisateur');
  console.log('Récupération liste utilisateur');
});

// GET /user/:id : Récupération par paramètre dynamique d'URL (req.params.id)
router.get('/:id', (req, res, next) => {
  res.status(200).json('Récupération un utilisateur');
  console.log('Récupération un utilisateur');
});

// POST /user/ : Création via le corps JSON (req.body)
router.post('/', (req, res, next) => {
  res.status(201).json('Création utilisateur : ' + req.body.firstname + ' ' + req.body.lastname);
  console.log('Création utilisateur');
});

// PUT /user/:id : Remplacement complet par ID
router.put('/:id', (req, res, next) => {
  res.status(201).json('Modification utilisateur : ' + req.body.firstname + ' ' + req.body.lastname + ' id : ' + req.params.id);
  console.log('Modification utilisateur');
});

// DELETE /user/:id : Suppression par ID
router.delete('/:id', (req, res, next) => {
  res.status(204).json('Suppression utilisateur : ' + req.params.id);
  console.log('Suppression utilisateur');
});

// GET /user/filter/:firstname/:limit : Paramètres multiples d'URL
router.get('/filter/:firstname/:limit', (req, res, next) => {
  res.status(200).json('filtre des utilisateurs ' + req.params.firstname + ' ' + req.params.limit);
});

module.exports = router;
```

### `route/product.js` (Endpoints Produits)
```javascript
const express = require('express');
const router = express.Router();

// GET /product/
router.get('/', (req, res, next) => {
  res.status(200).json('Récupération produit');
  console.log('Récupération produit');
});

// POST /product/
router.post('/', (req, res, next) => {
  res.status(201).json('Création produit');
  console.log('Création produit');
});

// PUT /product/
router.put('/', (req, res, next) => {
  res.status(201).json('Modification produit');
  console.log('Modification produit');
});

// DELETE /product/
router.delete('/', (req, res, next) => {
  res.status(204).json('Suppression produit');
  console.log('Suppression produit');
});

module.exports = router;
```

---

## 8. Le Mécanisme des Middlewares (`req`, `res`, `next`)

Un **middleware** est une fonction intermédiaire intervenant lors du traitement d'une requête HTTP avant l'envoi de la réponse finale[cite: 7].

```javascript
(req, res, next) => {
  // Traitement, validation ou transformation...
  next(); // Passe la main au middleware suivant, ou res.send() / res.json() pour répondre
};
```

* **`req` (*Request*) :** contient toutes les données entrantes[cite: 7] :
  * `req.params` : variables dynamiques passées dans l'URL (`:id`)[cite: 17].
  * `req.body` : données envoyées dans le corps de la requête (nécessite `app.use(express.json())`)[cite: 16, 17].
* **`res` (*Response*) :** fournit les méthodes d'émission de réponse[cite: 7] :
  * `res.status(code)` : positionne le code HTTP[cite: 13].
  * `res.json(data)` : sérialise et renvoie des données au format JSON[cite: 13].
* **`next` :** fonction permettant de passer la main au middleware suivant dans la pile[cite: 7].

> [!DANGER] Blocage de requête (Timeout)
> Si un middleware n'appelle ni `res.send()` / `res.json()`, ni `next()`, la connexion reste ouverte et le client se retrouve en attente indéfinie (*timeout*)[cite: 7].

---

## 9. Exécution & Outillage de Développement

### Commandes usuelles
```bash
# Lancer le serveur
node server

# Stopper le serveur
Ctrl + C
```

### Amélioration : Redémarrage automatique avec `nodemon`
Pour éviter de couper et relancer manuellement le terminal à chaque sauvegarde :

1. Installer `nodemon` en dépendance de développement :
   ```bash
   npm install -D nodemon
   ```
2. Ajouter le script dans `package.json` :
   ```json
   "scripts": {
     "start": "node server.js",
     "dev": "nodemon server.js"
   }
   ```
3. Lancer en mode écoute continue :
   ```bash
   npm run dev
   ```
