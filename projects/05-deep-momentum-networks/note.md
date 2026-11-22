Etat de l'art sur les strats de trend following / Momentum

- Le momentum transversal (cross-sectional) compare les actifs entre eux. Tu classes toutes les actions selon leur rendement des 3 à 12 derniers mois, puis tu achètes les 10 % meilleures et tu vends les 10 % pires. Tu paries que les gagnants vont continuer à battre les perdants. C'est le résultat de Jegadeesh et Titman (1993), la référence [35].

- Le momentum temporel (time-series) compare chaque actif à son propre passé. S'il a monté sur un an, tu achètes ; s'il a baissé, tu vends. C'est Moskowitz et al. (2012), la référence [1] : 58 actifs sur 25 ans, avec la mise à l'échelle par la volatilité.


La strategie que l'on va mettre en place repose sur la seconde approche donc momentum temporel
On doit a chaque fois donc déterminer deux choses, c'est une structure qui vaux pour les strats de momentum / trend following en réalité :

-> Comment mesurer la tendance ?
-> Comment transformer cette mesure en taille de position ?



Ici on va utiliser une structure LSTM -> Est ce que un bon sujet de suite serait de transformer ce LSTM via les transformer qui est un peu la suite "logique" des LSTM
