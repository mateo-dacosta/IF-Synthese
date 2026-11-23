# Rapport Général - Étude Comparative des Frameworks Backend (Brouillon)

**Module :** Infrastructures & Frameworks (IF) - Master 2 MIAGE  
**Auteur :** Matéo Da Costa  
**Statut :** Document de travail / Brouillon évolutif  

---

## 1. Introduction & Problématique

Dans le cadre du module **Infrastructures & Frameworks**, nous explorons cinq technologies backend emblématiques représentant des philosophies de conception, des langages et des moteurs d'exécution très différents :
- **Ruby on Rails** (Ruby) : le pionnier du *Convention over Configuration* et du monolithe MVC.
- **NestJS** (TypeScript / Node.js) : l'architecture d'entreprise modulaire et typée sur boucle d'événements.
- **Phoenix** (Elixir / BEAM) : la concurrence massive et la tolérance aux pannes du modèle d'acteurs. *(À venir)*
- **Gin** (Go) : le minimalisme compilé, la performance brute et l'empreinte mémoire minimale. *(À venir)*
- **Django** (Python) : le framework *batteries-included* pragmatique et adossé à l'écosystème data/IA. *(À venir)*

**Question centrale :** Il n'existe pas de « meilleur » framework dans l'absolu. Comment les choix de conception (modèle d'exécution, niveau d'abstraction, système de typage) déterminent-ils le compromis entre **productivité de développement (DevEx)**, **maintenabilité à grande échelle** et **performance opérationnelle** ?

---

## 2. Grille d'Analyse Comparative

Pour évaluer chaque technologie de manière objective, nous retenons 5 critères fondamentaux :

| Critère | Question clé | Ce qu'on observe |
| :--- | :--- | :--- |
| **Philosophie & Abstraction** | Le framework est-il magique ou explicite ? | Conventions implicites (Rails), architecture en couches/IoC (NestJS), ou minimalisme (Gin). |
| **Moteur d'exécution & Concurrence** | Comment le runtime traite-t-il les requêtes ? | Multi-process/thread (Ruby/Python), Event Loop mono-thread (Node.js), goroutines (Go), processus BEAM légers (Elixir). |
| **Typage & Sécurité à la compilation** | Les erreurs sont-elles détectées avant l'exécution ? | Dynamique (Ruby, Python), transpilé/statique partiel (TypeScript), ou statique strict compilé (Go). |
| **Vélocité vs Dette technique** | À quelle vitesse livre-t-on et comment le code vieillit-il ? | Prototypage instantané vs robustesse et modularité sur des projets d'équipe à long terme. |
| **Synergie avec l'IA Générative** | L'IA renforce-t-elle ou fragilise-t-elle le framework ? | Capacité de l'IA à absorber la verbosité, risques d'hallucinations et présence de garde-fous (compilateur, validation). |

---

## 3. Premiers Enseignements : Ruby on Rails vs NestJS

À ce stade du module, la comparaison entre nos deux premiers ateliers met en lumière deux visions opposées du développement backend :

### A. Convention vs Explicite (Magie vs Rigueur)
- **Ruby on Rails** mise tout sur la « magie » et le bonheur du développeur (*Developer Happiness*). Avec très peu de lignes de code et des commandes de génération puissantes, Rails déduit les noms de tables, les relations et les routes. Revers de la médaille : l'abstraction est parfois opaque (ex: gestion des clés étrangères dans les migrations ou requêtes N+1 si l'on ne surveille pas l'ORM).
- **NestJS** adopte la démarche inverse : aucune magie, tout est explicitement déclaré. Inspiré de Spring Boot, il impose un découpage strict en Modules, Contrôleurs et Services liés par injection de dépendances. C'est plus lourd et verbeux au démarrage, mais beaucoup plus lisible et compartimenté pour une grande équipe.

### B. Moteur d'exécution : Interprété vs Event Loop
- **Rails (Ruby)** : Langage interprété, historiquement plus gourmand en ressources CPU et mémoire, nécessitant souvent du scaling horizontal (plusieurs workers/instances).
- **NestJS (Node.js)** : Basé sur le moteur V8 mono-thread avec boucle d'événements (*Event Loop*). Extrêmement performant pour les opérations d'entrée/sortie (I/O) non bloquantes (appels base de données, microservices), mais vulnérable aux blocages si un traitement CPU lourd est exécuté de manière synchrone.

### C. Validation & Contrats de données
- Rails intègre la validation directement au niveau du modèle ActiveRecord.
- NestJS sépare rigoureusement la validation d'entrée via des **DTOs** et `class-validator` (filtrage HTTP avant d'atteindre le métier) de la persistance en base (gérée par un ORM moderne typé comme Prisma).

---

## 4. Tableau Synthétique des Frameworks (En cours de complétion)

| Framework | Langage | Typage | Modèle de Concurrence | Philosophie dominante | Cas d'usage idéal |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Ruby on Rails** | Ruby | Dynamique | Multi-thread / Multi-process | *Convention over Configuration* | MVP rapide, SaaS monolithique, petite équipe agile |
| **NestJS** | TypeScript | Statique (transpilé) | Mono-thread (Event Loop asynchrone) | *Clean Architecture*, modulaire, IoC | APIs d'entreprise, microservices, écosystème fullstack JS/TS |
| **Phoenix** | Elixir | Dynamique / Typespecs | Modèle d'Acteurs (processus légers BEAM) | Tolérance aux pannes, réactivité temps réel | Systèmes distribués, messageries, fort trafic temps réel |
| **Gin** | Go | Statique (compilé) | Goroutines (CSP) | Minimalisme, performance brute | Microservices critiques, passerelles d'API, haute scalabilité |
| **Django** | Python | Dynamique (type hints) | Multi-thread / Asynchrone (ASGI) | *Batteries included*, sécurité par défaut | Applications data/IA, portails de gestion, CMS/Backoffice |

---

## 5. Réflexion Transversale : L'Impact de l'IA Générative

L'essor des outils d'IA (copilotes de code, agents) transforme en profondeur les critères de choix d'un framework :

1. **La fin de la pénalité de la verbosité :**  
   Historiquement, des frameworks comme Rails avaient un avantage décisif car ils épargnaient l'écriture de centaines de lignes de code d'infrastructure. Aujourd'hui, l'IA génère les DTOs, les décorateurs, les classes de modules et les tests NestJS en quelques secondes. La verbosité n'est plus un frein majeur.

2. **Le typage strict comme garde-fou indispensable :**  
   Plus l'on confie la production de code à des modèles d'IA, plus le système de types devient critique. Sur des langages dynamiques, l'IA peut introduire des erreurs subtiles indétectables avant l'exécution. Avec TypeScript ou Go, le compilateur agit comme un premier filtre automatique contre les hallucinations d'attributs ou de méthodes.

3. **Le risque de l'illusion de sécurité :**  
   Comme observé lors du TP NestJS, le compilateur ne résout pas tout : un code parfaitement typé en TypeScript peut être une passoire à l'exécution si l'IA oublie les annotations de validation runtime (`class-validator`) ou les règles de concurrence de base.

---

## 6. Prochaines Étapes du Rapport

- [ ] Atelier 3 : Analyse de **Phoenix (Elixir)** et de la machine virtuelle BEAM.
- [ ] Atelier 4 : Évaluation de **Gin (Go)** et benchmarks de performance brute / mémoire.
- [ ] Atelier 5 : Retour d'expérience sur **Django (Python)** et son écosystème unifié.
- [ ] Synthèse finale : Matrice d'aide à la décision pour architecte logiciel en 2026+.
