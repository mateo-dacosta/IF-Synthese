# Rapport IF

**Auteur :** Matéo Da Costa

## 1. Introduction & Problématique

Dans le cadre du module **Infrastructures & Frameworks**, nous explorons cinq technologies backend emblématiques représentant des philosophies de conception, des langages et des moteurs d'exécution très différents :
- **Ruby on Rails** (Ruby) : le pionnier du *Convention over Configuration* et du monolithe MVC.
- **NestJS** (TypeScript / Node.js) : l'architecture d'entreprise modulaire et typée sur boucle d'événements.
- **Phoenix** (Elixir / BEAM) : la concurrence massive et la tolérance aux pannes du modèle d'acteurs. *(À venir)*
- **Gin** (Go) : le minimalisme compilé, la performance brute et l'empreinte mémoire minimale. *(À venir)*
- **Django** (Python) : le framework *batteries-included* pragmatique et adossé à l'écosystème data/IA. *(À venir)*

**Problématique :** Il n'existe pas un framework meilleur qu'un autre, donc comment les choix de conception déterminent-ils le compromis entre productivité, DevEx, maintenabilité et performance ?

## 2. Caractéristiques des frameworks

Pour évaluer chaque framework voici leur philosophie en quelques mots :

- **Ruby on Rails** (Ruby) : 
  - *Philosophie :* « Convention over Configuration » et « Developer Happiness ». 
  - *En résumé :* Tout est pensé pour prototyper et livrer très vite grâce à des conventions implicites et un monolithe MVC tout-en-un qui évite d'écrire du code d'infrastructure.

- **NestJS** (TypeScript / Node.js) : 
  - *Philosophie :* Rigueur d'entreprise, modularité et typage strict (inspiré de Spring Boot). 
  - *En résumé :* Aucune magie, architecture explicite découpée en Modules, Contrôleurs et Services avec injection de dépendances. Conçu pour des APIs structurées et maintenables en équipe sur un moteur asynchrone non-bloquant.

- **Phoenix** (Elixir) : 
  - *Philosophie :* Concurrence massive et tolérance aux pannes (modèle d'acteurs sur la machine virtuelle BEAM / Erlang). 
  - *En résumé :* Permet d'encaisser des millions de connexions simultanées en temps réel (WebSockets, LiveView) avec une isolation totale des processus (un crash n'impacte jamais le reste du système).

- **Gin** (Go) : 
  - *Philosophie :* Minimalisme, simplicité et performance brute. 
  - *En résumé :* Un micro-framework HTTP réduit à l'essentiel, compilé en binaire natif. Empreinte mémoire minimale, temps de réponse quasi-instantanés et concurrence légère grâce aux goroutines.

- **Django** (Python) : 
  - *Philosophie :* « Batteries-included » et pragmatisme. 
  - *En résumé :* Fournit clé en main tous les outils nécessaires au web (ORM puissant, back-office d'administration automatique, authentification, sécurité), avec une passerelle directe vers l'écosystème data et IA de Python.


## 3. Comparaison des conceptes

À ce stade du module, la comparaison repose sur Ruby et NestJS

### A. Convention vs Explicite
- **Ruby on Rails** mise tout sur l'abstraction et le bonheur du développeur. Très peu de lignes de code et des commandes de génération, noms de tables, les relations et les routes. Mais elle peut etre opaque pour un jeune dev.
- **NestJS** adopte la démarche inverse : aucune magie, tout est explicitement déclaré. Inspiré de Spring Boot, il impose un découpage strict en Modules, Contrôleurs et Services liés par injection de dépendances. C'est plus lourd et verbeux au démarrage, mais beaucoup plus lisible et compartimenté pour une grande équipe.

### B. Moteur d'exécution : Interprété vs Event Loop
- **Rails (Ruby)** : Langage interprété, historiquement plus gourmand en ressources CPU et mémoire, nécessitant souvent du scaling horizontal.
- **NestJS (Node.js)** : Basé sur un moteur mono-thread avec boucle d'événements (*Event Loop*). Extrêmement performant pour les opérations d'entrée/sortie mais vulnérable aux blocages sur de l'asynchrone.

### C. Contrats d'interfaces
- Rails intègre la validation directement au niveau du modèle ActiveRecord.
- NestJS sépare rigoureusement la validation d'entrée via des **DTOs** et `class-validator`.


## 4. Impact de l'IA Générative

L'essor des outils d'IA transforme en profondeur les critères de choix d'un framework :

1. **La fin des problèmes de verbosité :**  
   Historiquement, des frameworks comme Rails avaient un avantage décisif de réduire les lignes de code mais aujourd'hui, l'IA génère les DTOs, les décorateurs, les classes de modules et les tests NestJS en quelques secondes.

2. **Le typage strict comme garde-fou indispensable :**  
   Plus l'on confie la production de code à des modèles d'IA, plus le système de types devient critique. Sur des langages dynamiques, l'IA peut introduire des erreurs subtiles indétectables avant l'exécution.

3. **Le risque de l'illusion de sécurité :**  
   Comme observé lors du TP NestJS, un compilateur ne résout pas tout meme dans un langague typé comme TypeScript. L'IA peut oublier des annotations ou ne pas bien définir des types qui derrière seront prit comme source de vérité par les développeurs.

