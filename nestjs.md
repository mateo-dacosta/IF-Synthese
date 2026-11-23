# Synthèse NestJS

## 1. Ce qu'on a fait pendant le TP

L'objectif de la séance était d'ajouter la gestion des dépenses dans l'application Les Bons Comptes en utilisant NestJS.

En partant d'une base avec l'authentification JWT, les utilisateurs et les porte-monnaies, on a réalisé 4 étapes :
- La mise en place de la route `GET /wallets/:walletId/expenses` avec le calcul de la somme totale en base et le formatage des données.
- La création de la route `POST /wallets/:walletId/expenses` avec un DTO validé via `class-validator`, la gestion d'un payeur alternatif membre et le rafraîchissement de la date de modification du porte-monnaie.
- La sécurisation de la suppression `DELETE /wallets/:walletId/expenses/:id` (réponse 204) avec un Guard personnalisé (`ExpenseOwnerGuard`), un décorateur de paramètre `@CurrentUser()` et la vérification que le porte-monnaie n'est pas clôturé.
- En bonus, le calcul de clôture et d'équilibre des comptes (`GET /wallets/:id/balance`) via une fonction pure pour répartir équitablement les centimes restants et générer la liste des virements d'équilibrage.

## 2. Ce que je retiens de NestJS

NestJS s'appuie sur TypeScript et un moteur d'exécution (Node.js). C'est du JavaScript qui s'exécute sur un environnement mono-thread basé sur une boucle d'événements (*event loop*).

### Les points positifs
- **Une architecture claire et standardisée** : 
La séparation nette entre Contrôleur (gestion HTTP), Service (logique métier) et Module (encapsulation et injection de dépendances) évite le code fouillis et structure très bien les projets d'équipe.
- **La robustesse du typage et de la validation** : 
Le duo TypeScript + DTO avec `class-validator` et `ValidationPipe` permet de bloquer automatiquement les données mal formées dès l'entrée HTTP avant même d'atteindre le code métier.
- **Le modèle asynchrone non-bloquant** : 
Grâce à l'event loop de Node.js et aux `async/await`, NestJS gère très bien un grand nombre de requêtes d'I/O simultanées sans bloquer le thread principal.

### Les points négatifs
- **Beaucoup plus verbeux que Rails** : 
Il y a beaucoup de fichiers et de décorateurs à écrire pour une simple route (DTO, décorateurs, contrôleur, service, déclaration dans le module). C'est moins direct pour prototyper rapidement.
- **L'injection de dépendances parfois piégeuse** : 
Si on oublie de lister un service dans les `providers` du module, l'application plante au runtime au démarrage avec une erreur parfois obscure.
- **Le piège du mono-thread** : 
Comme Node.js est mono-thread, si on effectue un traitement synchrone lourd (CPU-bound) dans un service au lieu d'opérations asynchrones, on risque de bloquer toute l'event loop et d'impacter toutes les autres requêtes utilisateurs.

## 3. Les difficultés et blocages rencontrés

Pendant le TP, j'ai rencontré quelques blocages :

- **L'oubli du provider dans le module** :  
   Après avoir créé `ExpensesController` et `ExpensesService`, l'application a planté avec l'erreur `Nest can't resolve dependencies of the ExpensesController (?)`. J'avais oublié d'ajouter `ExpensesService` dans le tableau `providers` de `ExpensesModule`.

- **Les contraintes sur le DTO de dépense** :  
   Au départ, j'avais utilisé `@IsPositive()` pour le montant, mais le test a refusé `12.5` car les montants doivent être des entiers stricts en centimes. J'ai dû combiner `@IsInt()` et `@Min(1)`. De plus, j'avais oublié `@IsOptional()` sur `paidById`, ce qui rejetait les requêtes valides où l'utilisateur connecté est le payeur par défaut.

- **Le code HTTP 204 sur la suppression** :  
   Par défaut, une route `@Delete()` renvoie un code `200 OK` dans NestJS si on renvoie du contenu ou du void sans précision. Le test attendait un `204 No Content`, ce qui a nécessité l'ajout de `@HttpCode(HttpStatus.NO_CONTENT)`.

- **L'extension `.js` obligatoire en ESM** :  
   Dans un projet Node en mode ES Modules (`"type": "module"`), chaque import local doit impérativement se terminer par `.js` même si les sources sont en `.ts` (ex: `import ... from './expenses.service.js'`), ce que l'auto-complétion de VS Code n'ajoute pas toujours par défaut.

## 4. Mon avis sur l'utilisation de l'IA (IA générative)

Contrairement à Ruby on Rails où l'IA compense surtout le manque d'expérience avec les conventions magiques, avec NestJS l'IA apporte un bénéfice différent :
- **Idéal pour absorber la verbosité (boilerplate)** : 
Comme NestJS demande beaucoup de code d'infrastructure (décorateurs Swagger, DTOs, classes de modules, tests unitaires avec mocks), l'IA excelle pour générer rapidement tout ce squelette répétitif.

- **Moins d'hallucinations grâce au typage explicite** : 
NestJS étant très explicite avec TypeScript, l'IA dispose de contrats clairs (types Prisma, interfaces, DTOs). Le compilateur TypeScript et le `ValidationPipe` détectent immédiatement si l'IA invente une méthode ou une propriété inexistante.

- **Un bon tuteur pour l'architecture** : 
L'IA est très utile pour expliquer comment brancher les Guards, les Interceptors ou les Pipes dans le cycle de vie de NestJS, qui sont des notions plus abstraites que le MVC classique.

## 5. Conclusion

NestJS offre un cadre d'entreprise robuste et moderne pour construire des APIs avec Node.js. C'est plus verbeux et moins axé sur la productivité immédiate que Ruby on Rails, mais en contrepartie, le typage strict, la modularité et l'inversion de contrôle rendent les projets beaucoup plus faciles à maintenir et à faire évoluer à grande échelle.
