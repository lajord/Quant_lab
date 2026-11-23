Etat de l'art sur les strats de trend following / Momentum

- Le momentum transversal (cross-sectional) compare les actifs entre eux. Tu classes toutes les actions selon leur rendement des 3 à 12 derniers mois, puis tu achètes les 10 % meilleures et tu vends les 10 % pires. Tu paries que les gagnants vont continuer à battre les perdants. C'est le résultat de Jegadeesh et Titman (1993), la référence [35].

- Le momentum temporel (time-series) compare chaque actif à son propre passé. S'il a monté sur un an, tu achètes ; s'il a baissé, tu vends. C'est Moskowitz et al. (2012), la référence [1] : 58 actifs sur 25 ans, avec la mise à l'échelle par la volatilité.


La strategie que l'on va mettre en place repose sur la seconde approche donc momentum temporel
On doit a chaque fois donc déterminer deux choses, c'est une structure qui vaux pour les strats de momentum / trend following en réalité :

-> Comment mesurer la tendance ?
-> Comment transformer cette mesure en taille de position ?



Ici on va utiliser une structure LSTM -> Est ce que un bon sujet de suite serait de transformer ce LSTM via les transformer qui est un peu la suite "logique" des LSTM


## Partie data — premiers pas

Objectif : passer des fichiers Pinnacle bruts à une table propre de rendements journaliers, avec un actif par colonne. Tout se fait dans un notebook `01_data.ipynb`, une étape à la fois.

### Étape 0 — Ranger avant de toucher

- Créer `data/raw/` pour les fichiers Pinnacle tels quels, jamais modifiés, et `data/processed/` pour ce que je produis.
- Ajouter `data/` au `.gitignore` **avant** de copier les fichiers : les données sont sous licence.
- Dans DataMaker, générer les séries continues avec la méthode **Ratio Adjusted**. Noter ici les réglages utilisés.

### Étape 1 — Faire l'inventaire, sans aucun calcul

Pour chaque fichier, noter :

- le marché, en reliant le code Pinnacle à la liste de l'annexe A du paper ;
- les colonnes et le format des dates ;
- la première date, la dernière date et le nombre de lignes.

Résultat attendu : un tableau d'inventaire, qui répond à deux questions :

- Les 88 contrats du paper sont-ils tous là ? Lesquels manquent, lesquels sont en plus ?
- Quelles séries s'arrêtent avant 2026 ? Surveiller en particulier les « Day Session ».

### Étape 2 — Contrôler la qualité

- Dates en double, jours manquants, longs trous.
- Prix nuls ou négatifs.
- Prix figés : le même Settle plusieurs jours de suite.
- Les plus gros rendements journaliers : vrais événements (2008, mars 2020…) ou erreurs de données ?

### Étape 3 — Mes décisions, à écrire ici avant de coder la suite

- Prix utilisé : le Settle, seul prix de fin de journée fourni par Pinnacle.
- Univers : les 88 contrats du paper, ou sans les doublons de séance ?
- Rendements : simples ou logarithmiques ?
- Winsorisation à 5 écarts-types, demi-vie de 252 jours : sur les prix ou sur les rendements ?
- Jours manquants : que faire ?

### Étape 4 — Première sortie

- Une table des rendements journaliers, avec les dates en lignes et les actifs en colonnes, sauvegardée dans `data/processed/`.
- Trois graphiques de contrôle : quelques séries de prix en échelle log, l'histogramme des rendements d'un actif, et le nombre d'actifs disponibles chaque jour ($N_t$).

Ensuite seulement : la volatilité, puis la stratégie Long Only.
