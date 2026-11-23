# Synthèse NestJS

## 1. Ce qu'on a fait pendant le TP

L'objectif de la séance était de compléter le backend de l'application Les Bons Comptes en développant le module des dépenses avec NestJS.

En partant d'une base avec les utilisateurs, les porte-monnaies et Prisma, on a réalisé 4 missions :
- La route `GET /wallets/:walletId/expenses` pour lister les dépenses et calculer le total en base.
- La route `POST /wallets/:walletId/expenses` avec validation stricte par DTO (`class-validator`), payeur optionnel et rafraîchissement de `updatedAt`.
- La suppression sécurisée `DELETE /wallets/:walletId/expenses/:id` (code 204) via un Guard dédié (`ExpenseOwnerGuard`), un décorateur personnalisé `@CurrentUser()` et le blocage si clôturé.
- En bonus, le calcul de clôture (`GET /wallets/:id/balance`) avec une fonction pure pour répartir le reste des centimes par ordre d'`userId` et générer les virements.

## 2. Ce que je retiens de NestJS

NestJS apporte la rigueur de Spring Boot à l'écosystème Node.js. Il structure Express avec TypeScript sur un moteur d'exécution JavaScript mono-thread basé sur une boucle d'événements (*event loop*).

### Les points positifs
- **Une architecture familière et modulaire** : 
On retrouve les repères de Spring Boot (Module, Contrôleur, Service, Injection de dépendances), mais organisés par dossier fonctionnel (`src/expenses/`) plutôt que par couche technique.
- **Le cycle de traitement prévisible** : 
Le pipeline de requête est clair : Guard (401 - qui es-tu ?) ➔ DTO + ValidationPipe (400 - données valides ?) ➔ Contrôleur (délégation) ➔ Service (403/404/409 - règles métier) ➔ Prisma (SQL). Chaque étape filtre avant la suivante.
- **Performances sur les I/O** : 
Le modèle asynchrone non bloquant (`async/await`) de Node.js gère très efficacement les requêtes concurrentes vers la base de données (PostgreSQL/Neon).

### Les points négatifs
- **Lourdeur du boilerplate** : 
C'est beaucoup plus verbeux que Ruby on Rails. Il faut écrire un DTO, des décorateurs, une classe de service, un contrôleur et déclarer le tout dans le module pour chaque ressource.
- **L'injection de dépendances manuelle** : 
L'oubli d'un service dans le tableau `providers` fait planter l'application au démarrage avec une erreur parfois trompeuse.
- **Pièges du mono-thread et des flottants** : 
Comme l'event loop est mono-thread, un calcul lourd synchrone bloque tout le serveur. De plus, JS impose de manipuler les montants en centimes entiers pour éviter les pertes de précision.

## 3. Les difficultés et blocages rencontrés

Pendant le TP, j'ai rencontré quelques blocages :

- **L'oubli du provider dans le module** :  
   Après avoir codé `ExpensesController` et `ExpensesService`, NestJS a levé `Nest can't resolve dependencies of the ExpensesController (?)`. J'avais oublié d'ajouter le service dans `providers` de `ExpensesModule`.

- **La validation stricte du DTO** :  
   J'avais initialement utilisé `@IsPositive()` pour le montant, mais le test a refusé `12.5` car un montant en centimes doit être un entier strict (`@IsInt()` + `@Min(1)`). J'ai aussi dû ajouter `@IsOptional()` sur `paidById` pour autoriser l'absence de payeur explicite.

- **Le code HTTP 204 sur le DELETE** :  
   NestJS renvoie `200 OK` par défaut sur une route DELETE. Pour valider le test, il a fallu ajouter explicitement `@HttpCode(HttpStatus.NO_CONTENT)`.

- **Les contraintes d'import ESM** :  
   En mode ECMAScript Modules (`"type": "module"`), chaque import de fichier local doit comporter son extension `.js` (ex: `expenses.service.js`), ce qui n'est pas automatique dans VS Code.

## 4. Mon avis sur l'utilisation de l'IA (IA générative)

Pour NestJS, l'apport de l'IA est très complémentaire à celui observé sur Ruby on Rails :
- **Absorption du boilerplate** : 
L'IA est particulièrement efficace pour générer le code répétitif indispensable à NestJS (décorateurs Swagger, classes DTO, mocks de tests unitaires avec Vitest).
- **Moins d'hallucinations grâce au typage fort** : 
Contrairement à Rails où les conventions implicites peuvent tromper les modèles, NestJS repose sur des contrats explicites (types Prisma, interfaces, DTO). Le compilateur TypeScript et le `ValidationPipe` détectent immédiatement les erreurs de l'IA.
- **Assistance à l'architecture** : 
L'IA aide à choisir le bon outil au bon endroit (quand utiliser un Guard vs un Middleware vs un Pipe) et à respecter la séparation stricte des responsabilités.

## 5. Conclusion

NestJS est moins orienté sur la productivité instantanée que Ruby on Rails, mais il offre un cadre d'entreprise robuste, prévisible et maintenable pour les APIs d'envergure. En associant la puissance de TypeScript et le modèle asynchrone de Node.js, il constitue un standard solide pour des architectures modernes en conteneurs (Cloud Run).
