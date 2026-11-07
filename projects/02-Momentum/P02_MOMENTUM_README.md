# P02 — Is Momentum Real?

**Quant Lab · Projet 02 · Data & Scientific Foundations**

> **Statut** — À commencer  
> **Durée** — Libre. La roadmap donne un repère de 3 à 4 séances.  
> **Prérequis** — Savoir récupérer les clôtures ajustées et calculer un rendement simple, comme dans P01.

## 1. Mission

**Après un mois de hausse de SPY, le mois suivant est-il positif plus souvent qu'en général ?**

Tu vas tester cette hypothèse sur l'historique de SPY. À la fin de chaque mois, tu connais déjà le rendement du mois qui se termine. Ce rendement est ton **signal**. Le rendement du mois suivant est le **résultat** que tu observes plus tard.

Ton résultat principal est l'écart, en **points de pourcentage**, entre la fréquence des hausses après un mois positif et la fréquence générale des hausses sur les mêmes mois évaluables.

## 2. Périmètre

- **Instrument** — SPY, comme dans P01.
- **Données** — Clôtures journalières ajustées depuis 2015. Note la dernière date incluse pour que ton expérience soit reproductible.
- **Fréquence de l'analyse** — Mensuelle. Utilise la clôture ajustée de la dernière séance de chaque mois complet.
- **Signal** — Le rendement simple du mois qui vient de se terminer. Un mois est positif si son rendement est strictement supérieur à zéro.
- **Résultat** — Le rendement simple du mois suivant. Pour la fréquence de hausse, un rendement nul compte comme non positif.
- **Outils** — `main.ipynb`, avec `pandas`, `numpy`, `matplotlib` et `yfinance` si tu dois récupérer les données à nouveau.

## 3. Missions

### Mission 1 — Passer des clôtures journalières aux rendements mensuels

**Objectif** — Obtenir un rendement simple par mois complet.

- Reprends les clôtures ajustées de P01. Vérifie qu'elles sont ordonnées, sans doublons ni clôtures manquantes.
- Garde la dernière clôture de cotation de chaque mois complet. Si ton historique s'arrête en cours de mois, écarte ce dernier mois.
- Calcule chaque rendement en divisant la clôture de fin de mois par celle du mois précédent, puis en soustrayant 1. La première clôture mensuelle donne un rendement manquant si tu n'as pas celle du mois précédent.
- Vérifie à la main un rendement avec deux clôtures de fin de mois consécutives.

**Terminé lorsque** tu peux montrer les deux clôtures utilisées pour ton contrôle et expliquer pourquoi le rendement porte le nom du second mois.

### Mission 2 — Aligner le signal et le résultat

**Objectif** — Construire un tableau où chaque ligne associe un mois au rendement du mois suivant.

- Sur chaque ligne, garde le mois du signal, son rendement et le rendement du mois suivant.
- Affiche deux lignes et retrouve à la main les mois utilisés. Par exemple, le rendement de janvier sert de signal et celui de février sert de résultat.
- Écarte les lignes où le signal ou le résultat manque, y compris le dernier mois si le suivant n'est pas encore observable. Si un mois entier manque dans les données, vérifie que l'alignement ne saute pas directement au mois d'après.

**Terminé lorsque** tu peux dire ce que tu savais à la date du signal et ce que tu as appris seulement le mois suivant.

### Mission 3 — Comparer les fréquences de hausse

**Objectif** — Mesurer si le signal apporte une information par rapport à la fréquence habituelle de hausse.

- Sur l'ensemble des lignes valides, compte les mois suivants positifs et divise par le nombre de lignes. C'est ta **fréquence de référence**.
- Refais le calcul uniquement sur les lignes où le mois du signal est positif. Indique le nombre de hausses, le nombre de mois évalués et la fréquence obtenue.
- Calcule l'écart entre cette fréquence et la référence, en points de pourcentage. Par exemple, 62 % contre 58 % donne **+4 points de pourcentage**.
- Fais un graphique en barres qui compare les deux fréquences avec leurs effectifs. Pour approfondir, regarde aussi ce qui se passe après un mois nul ou négatif.

**Terminé lorsque** tu peux expliquer ce que signifie l'écart observé et dire sur combien de mois il repose.

### Mission 4 — Conclure et préparer P03

**Objectif** — Rédiger une conclusion proportionnée aux résultats.

- Donne la période, les deux fréquences, les effectifs et l'écart. Dis si le résultat historique va dans le sens de ton hypothèse, dans le sens inverse ou reste peu concluant.
- Discute la taille de l'échantillon, le fait que l'étude porte sur un seul ETF et la dépendance possible entre des mois consécutifs. L'écart observé mesure une relation dans l'historique étudié. La capacité du signal à prédire sur une période future reste à tester.
- Si tu veux aller plus loin, compare le même écart sur deux sous-périodes chronologiques avec une date de coupure fixée avant la comparaison. Garde la même définition du signal dans les deux périodes.
- Note ce que tu réutiliserais dans P03. Par exemple, un rendement passé connu à la date du signal peut devenir une variable d'entrée d'un modèle.

**Terminé lorsque** ton notebook s'exécute du début à la fin et que ton bilan permet de comprendre la méthode, le résultat et ses limites.

#### Mon interprétation

> En gros on peut conclure que si le mois précédent est positif nos chance que le mois suivant soit positif augmente a 70% cependant c'est surement de la variabiltié car on a regardé ca sur une temps de période des 10 derniers années et que sur le long terme ca doit ce lisser et y'a juste aucun avantage en réalité

#### Nuance

Sur la période étudiée, SPY a progressé le mois suivant dans environ **70 % des cas après un mois positif**, contre **68 % en général**. L'écart d'environ **2 points de pourcentage** va dans le sens de l'hypothèse. Il pourrait refléter un petit effet ou la variabilité de l'échantillon. Il reste à vérifier si cet écart se retrouve sur une autre période.

## 4. Livrables

- `main.ipynb` avec la préparation des données, le tableau signal/résultat, les comparaisons, le graphique commenté et la conclusion.
- `P02_MOMENTUM_README.md` avec le journal de recherche complété à la fin.

## 5. Critères de validation

- [ ] Les clôtures ajustées, la période d'étude et la règle de sélection des mois complets sont documentées.
- [ ] Un rendement mensuel est vérifié à la main.
- [ ] Deux lignes confirment que le résultat correspond au mois qui suit le signal.
- [ ] Les deux fréquences reposent sur le même ensemble de mois évaluables, avec un filtre supplémentaire pour les signaux positifs.
- [ ] Le graphique indique les fréquences et les effectifs, et l'écart est exprimé en points de pourcentage.
- [ ] Le notebook se relance dans l'ordre et la conclusion distingue le constat historique de la capacité prédictive.

## 6. Documentation utile

- **Pandas** — Cherche `resample()` pour passer des données journalières aux données mensuelles, puis `shift()` pour aligner les rendements de deux mois consécutifs.
- **Rendements simples** — La méthode utilisée dans P01 s'applique aussi aux clôtures mensuelles. [Documentation Pandas sur `pct_change()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.pct_change.html)
- **Graphique** — Cherche `bar()` pour comparer les fréquences. [Guide de démarrage Matplotlib](https://matplotlib.org/stable/users/explain/quick_start.html)

## 7. Journal de recherche

### Hypothèse et attente initiale

_À compléter avant de comparer les fréquences._

### Résultats et effectifs

_À compléter après l'analyse._

### Limites et question à emporter vers P03

_À compléter à la fin._

---

**Règle Quant Lab** — Tu écris le code et interprètes les résultats. L'IA t'aide à clarifier les étapes, à trouver les outils utiles et à relire ton travail.
