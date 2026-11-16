# Synthèse Ruby on Rails

## 1. Ce qu'on a fait pendant le TP

L'objectif de la séance était d'ajouter des fonctionnalités l'application Splitly (les bon comptes).

En partant d'une base avec les utilisateurs et les portefeuilles, on a réalisé 5 missions :
- La création du modèle `Expense` et la migration en base de données.
- La mise en place des routes imbriquées (`/wallets/:id/expenses`) et du contrôleur `ExpensesController`.
- Les vues en HTML/ERB pour afficher la liste des dépenses et le formulaire pour en ajouter une.
- La possibilité d'associer un payeur parmi les membres du groupe.
- Le calcul du total des dépenses directement en base de données.

## 2. Ce que je retiens de Ruby on Rails

### Les points positifs
- **C'est rapide à mettre en place** : 
Peu de lignes de code, des commandes pour générer les modèles ou les contrôleurs font gagner du temps.
- **Le Convention over Configuration** : 
Rails devine beaucoup de choses tout seul (le nom des tables à partir des modèles, les routes REST, l'emplacement des vues). Cela simplifie beaucoup le code mais peut parfois poser problème.
- **Sécurité déjà intégrée** : 
La protection contre les failles courantes (injections SQL, attaques CSRF sur les formulaires) est activée par défaut.

### Les points négatifs
- **Le pilote auto** : 
Le framework fait beaucoup de choses automatiquement, c'est parfois difficile tout comprendre (ORM par exemple).
- **Moins rapide** : 
Ruby est un langage interprété, donc moins rapide que du C, du Go ou du Java pour des traitements back lourds.
- **Dure à optimiser** : 
Quand on ne fait pas attention, il est facile de faire des requêtes lentes.

## 3. Les difficultés et blocages rencontrés

Pendant le TP, j'ai rencontré quelques blocages :

- **Les clés étrangères dans la migration** :  
   En générant le modèle avec `creator:references` et `payer:references`, Rails a voulu créer des clés vers des tables `creators` et `payers` qui n'existent pas. La migration a planté. J'ai dû modifier la migration pour préciser que ces deux colonnes pointent vers la table `users`.

- **Le calcul du total (Ruby vs SQL)** :  
   Au début, j'ai fait la somme en Ruby avec `@expenses.sum(&:amount)`. Ça marche, mais en regardant les logs, Rails chargeait toutes les lignes de la base une par une. J'ai corrigé en faisant `@wallet.expenses.sum(:amount)` dans le contrôleur pour que PostgreSQL calcule la somme en une seule requête.

- **Le push Git bloqué en SSH** :  
   En voulant pousser mes commits sur le GitLab de l'université. J'ai réglé le problème avec un jeton d'accès GitLab (*Personal Access Token*). Après ce porblème est indépendant du framework.

## 4. Mon avis sur l'utilisation de l'IA (IA générative)

À mon avis, l'IAg risque de réduire l'intérêt d'un framework comme Ruby on Rails :
- **La DevEx devient moins déterminante** : 
La force historique de Rails reposait sur son objectif de Developer Happiness. Aujourd'hui, comme l'IA peut générer le code à la place du développeur le fait que la synthaxe soit lourde ou verbeuse n'est plus un problème.

- **Le désavantage de la performance** : 
Ruby reste un langage interprété, donc moins performant et plus gourmand qu'un langage compilé (comme Go, Rust ou Java). Si l'IA permet d'écrire tout aussi vite dans des technologies beaucoup plus rapides et économes, le compromis de Rails perd de sa valeur.

- **Le cadrage des conventions** : 
Le vrai point fort de Rails avec l'IA reste le Convention over Configuration. Cela force l'IA à produire un code standardisé et évite les possibles hallucinations ou perte de comprention du code source.


## 5. Conclusion

Ruby on Rails est fortement orienter devEX pour créer des prototypes ou des applications web rapidement. Une fois qu'on a compris le principe du MVC et des conventions, on développe beaucoup plus vite qu'avec d'autres technologies. Il faut faire attention à bien comprendre les conceptes derrièere l'abstraction.
