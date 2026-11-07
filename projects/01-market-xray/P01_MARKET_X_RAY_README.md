# P01 — Market X-Ray

**Quant Lab · Projet 01 · Data & Scientific Foundations**

> **Statut :** à commencer  
> **Durée :** libre. Une séance dure souvent environ une heure, mais ce projet peut prendre autant d'heures que nécessaire.  
> **Prérequis :** bases de Python. Les notions statistiques seront revues au moment où elles seront utiles.

## 1. Mission

**Question centrale : que peut-on apprendre sur le comportement d'un marché à partir de ses prix historiques ?**

Construis toi-même un notebook Python qui explore les données journalières de **SPY**, un ETF suivant le S&P 500. Tu dois arriver à décrire les rendements, leur dispersion, les périodes de volatilité et les baisses marquantes à l'aide de calculs et de visualisations reproductibles.

Ce projet est une **étude descriptive**, pas une stratégie de trading et pas une démonstration de pouvoir prédictif. Les données préparées serviront dans **P02 — Is Momentum Real?**

## 2. Périmètre

- **Marché :** SPY, pour commencer avec un seul instrument liquide.
- **Fréquence :** journalière.
- **Période :** depuis le 1er janvier 2015 jusqu'à la dernière date disponible, ou une date de fin explicitement définie.
- **Prix étudié :** série de clôture ajustée pour les divisions et distributions, en expliquant le choix de la donnée.
- **Environnement :** Python dans un notebook Jupyter (`main.ipynb`).
- **Bibliothèques permises :** `yfinance` (acquisition), `pandas`, `numpy`, `matplotlib`. Pas de framework de trading, de backtesting ou de ML dans ce premier projet.

**Principe :** utiliser les librairies pour les opérations standard, mais réaliser toi-même les analyses, les visualisations et les interprétations. L'IA guide sans écrire le code du projet.

## 3. Missions

### Mission 1 — Récupérer et comprendre les données

**Objectif :** obtenir une série de prix exploitable et savoir précisément ce qu'elle représente.

- Récupérer les données journalières de SPY.
- Examiner les colonnes, le type de l'index et la période réellement couverte.
- Vérifier l'ordre des dates, les doublons et la présence de valeurs manquantes.
- Identifier quelle série de prix tu utiliseras et pourquoi les ajustements comptent.

**Terminé lorsque :** tu peux expliquer d'où viennent les données, combien d'observations tu possèdes et pourquoi ta série est cohérente.

### Mission 2 — Mesurer les rendements

**Objectif :** passer des prix à des mesures exploitables statistiquement.

- Calculer les **rendements simples journaliers**.
- Calculer les **rendements logarithmiques** et expliquer leur différence conceptuelle.
- Résumer les rendements simples : moyenne, médiane, écart-type, minimum et maximum.
- Vérifier au moins deux calculs sur quelques dates avec une formule comprise, pas uniquement en faisant confiance à la librairie.
- Expliquer le traitement de la première observation pour laquelle aucun rendement précédent n'est disponible.

**Terminé lorsque :** les calculs sont cohérents et tu sais expliquer ce que signifie chaque métrique.

### Mission 3 — Visualiser le marché

**Objectif :** rendre les comportements du marché lisibles.

Produire **au moins trois visualisations commentées** :

1. Évolution du prix ajusté de SPY sur la période.
2. Distribution des rendements simples journaliers (histogramme).
3. Volatilité réalisée *glissante* des rendements (par exemple sur 21 séances), avec une unité explicitée.

Chaque graphique doit avoir un titre, des axes compréhensibles et une courte phrase expliquant ce que tu observes. Évite de surinterpréter une figure.

**Terminé lorsque :** les graphiques permettent de répondre à des questions précises et leurs unités sont explicites.

### Mission 4 — Comprendre le risque des baisses

**Objectif :** distinguer rendement, volatilité et perte depuis un sommet.

- Calculer une courbe de performance cumulée fondée sur les rendements simples.
- Calculer le **drawdown** depuis le plus haut historique de cette courbe, puis le **maximum drawdown**.
- Identifier une période marquante de baisse et la confronter à ce que montre ton graphique de volatilité.
- Expliquer pourquoi la volatilité et le drawdown ne mesurent pas la même chose.

**Terminé lorsque :** tu peux commenter un épisode de baisse de marché avec des chiffres et sans prétendre qu'il serait prédictible.

### Mission 5 — Conclure comme un chercheur

**Objectif :** présenter une étude intelligible et reproductible.

Répondre brièvement dans le notebook et dans ce README :

- Quelles sont les trois observations les plus intéressantes ?
- Quelle idée initiale a été confirmée, infirmée ou laissée ouverte ?
- Quelles sont les limites des données et de cette analyse ?
- Quelle nouvelle question aimerais-tu tester dans P02 sur le momentum ?

**Terminé lorsque :** une autre personne peut exécuter ton notebook, comprendre tes résultats et savoir ce que tu en déduis — ainsi que ce que tu ne peux pas en déduire.

## 4. Livrables

```text
projects/01-market-x-ray/
├── README.md      # Ce sujet, enrichi à la fin avec tes résultats
└── main.ipynb     # Ton code, les graphiques et les explications
```

Aucun autre fichier n'est obligatoire. Ne crée pas de modules, de tests complexes, d'architecture de package ou de dossier `data/` si cela n'apporte rien. Le téléchargement des données peut rester dans le notebook pour reproduire l'expérience.

## 5. Critères de validation

- [ ] Les données historiques utilisées sont identifiées, ordonnées et contrôlées.
- [ ] Les prix sont correctement définis et les ajustements compris.
- [ ] Les rendements simples et logarithmiques sont calculés et expliqués.
- [ ] Les statistiques descriptives sont cohérentes et au moins deux rendements sont vérifiés manuellement.
- [ ] Trois visualisations minimum sont lisibles, légendées et interprétées.
- [ ] La performance cumulée et le maximum drawdown sont correctement analysés.
- [ ] Le notebook s'exécute du début à la fin et peut être reproduit.
- [ ] Les conclusions distinguent constat, hypothèse et limites.
- [ ] Tu es capable d'expliquer ton code et tes résultats sans l'aide de l'IA.

**Critère de passage à P02 :** les objectifs sont réellement compris et atteints. Aucune deadline, aucun score de performance financière à atteindre.

## 6. Documentation utile (consulter au besoin)

- [yfinance — téléchargement de données](https://ranaroussi.github.io/yfinance/reference/api/yfinance.download.html) : paramètres `tickers`, `start`, `end`, `interval`, `auto_adjust` ; attention à la structure des colonnes retournées.
- [Pandas — `Series.pct_change`](https://pandas.pydata.org/docs/reference/api/pandas.Series.pct_change.html) : variations relatives entre observations.
- [Pandas — `Rolling.std`](https://pandas.pydata.org/docs/reference/api/pandas.core.window.rolling.Rolling.std.html) : écart-type sur une fenêtre glissante.
- [Matplotlib — Quick start](https://matplotlib.org/stable/users/explain/quick_start.html) : créer et annoter des figures.

Ne consulte pas tout avant de commencer. Utilise ces références uniquement lorsqu'elles deviennent utiles pour une mission.

## 7. Journal de recherche 

J'ai pu faire différent test sur le SPY
Notament sur les rendements, la volat des rendements, l'évolution d'un capital sur la période 2015-2026 en mode Buy and hold

Ce que j'ai pu apprendre / re découvrir c'est les closes ajusté, le fait que on distribue les dividendes des actions ca affecte le prix et donc
pour effiter d'avoir par exempel un -5% d'afficher alor que en réalité tout est normal e, on ajuste les close.. ce qui fait que y'a plus de répercution des
dividendes sur les closes tout est ok

Ce que j'ai bien aimé voir aussi c'est la corrélation positive entre drawdown et volatilité sur la période observé, j'ai utilisé une corélation de Pearson et la corrélation est de
0.71 ce qui est intéréssant, au final on peut conclure que les période de forte volatilité SONT souvent associer a des périodes de baisse car panique 

Les différentes fonctions que j'ai pu revoir 

- `pct_change()` — Calcule le rendement d’une séance par rapport à la précédente.
- `shift(1)` — Décale les valeurs d’une ligne pour comparer chaque séance à la précédente. => Ici sur un dataframe
- `.loc` et `.iloc` — Sélectionnent par étiquette (une date, par exemple) ou par position (`-1` pour la dernière ligne).
- `isna()` et `dropna()` — Repèrent les valeurs manquantes ou retirent les lignes qui en contiennent.
- `describe()` — Résume une série avec la moyenne, l’écart-type, les quartiles et les extrêmes.
- `rolling(21).std()` — Calcule la volatilité glissante sur 21 séances.
- `cumprod()` — Multiplie les facteurs de rendement au fil du temps pour construire la courbe de capital.
- `cummax()` — Donne le sommet atteint à chaque date, nécessaire pour calculer le drawdown.
- `max()` et `idxmax()` — Donnent la valeur du maximum et la date où il est atteint.
- `corr()` — Mesure la corrélation entre deux séries, comme le drawdown et la volatilité.
- `plot()` et `hist()` — Tracent une courbe dans le temps ou un histogramme pour voir la distribution.

---

**Règle Quant Lab :** l'IA peut clarifier la mission, orienter vers la documentation, poser des questions et relire ton travail. **Elle ne génère ni ne corrige le code à ta place.**
