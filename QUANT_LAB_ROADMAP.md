# Quant Lab — Objectifs, pédagogie & roadmap

> **Version 1.1 — Novembre 2026**  
> Un laboratoire personnel pour progresser en ingénierie IA et en Data Science **en construisant des stratégies de trading algorithmique**.  
> **Statut :** Phase 1 et Phase 2 terminées (P01 à P04) ; la suite reste volontairement évolutive.

---

## 1. Pourquoi ce projet existe

### Mon profil

- Je suis **ingénieur IA / Data Analyst**, avec des bases en Python, statistiques, mathématiques et Machine Learning que je souhaite **réactiver et approfondir**.
- J'ai terminé mes études.
- J'aime le trading algorithmique comme **terrain d'expérimentation technique et scientifique**.
- J'ai du temps à consacrer à cette activité, **sans contrainte de durée**. Je veux que ce soit motivant et ludique.

### Mon objectif

**Devenir meilleur en Python, Data Science, Machine Learning et démarche scientifique en appliquant systématiquement ces compétences à des problèmes concrets de trading algorithmique.**

À terme, je souhaite :

1. Construire un **portfolio GitHub** de projets quantitatifs présentables.
2. Concevoir, tester et comparer des **stratégies de trading sur des horizons plutôt longs** (hebdomadaires / mensuels, puis multi-actifs).
3. Apprendre à distinguer **un modèle prédictif intéressant** d'une **stratégie réellement robuste après coûts**.
4. Développer progressivement mes **propres outils Python** lorsque mes expériences les rendent utiles.

**Ce que ce projet n'est pas :** une formation de trader discrétionnaire, un programme académique théorique, une course à la rentabilité immédiate ou un système de trading réel dès le départ. On commence avec des données historiques et des simulations.

## 2. Philosophie pédagogique

### Apprendre en construisant

Chaque projet suit une boucle simple :

**Question concrète → hypothèse → code Python → expérience → résultat → conclusion.**

Principes :

1. **Pratique d'abord.** Du code dans presque chaque séance ; pas de longs livres ou de théorie sans application.
2. **Théorie juste à temps.** On revoit les maths, les statistiques ou les concepts ML lorsqu'un problème l'exige.
3. **ML rapidement.** Les premiers projets réactivent Python et les statistiques ; le premier modèle ML arrive dès **P03**, sans attendre de construire une infrastructure complète.
4. **Projets cumulatifs.** Un projet réutilise les données, features, prédictions ou fonctions construits auparavant.
5. **From scratch là où cela forme.** Je construis les features, signaux, logiques de stratégie et tests d'hypothèses. Je **n'ai pas besoin de réimplémenter** NumPy, Pandas ou scikit-learn.
6. **Pas de sur-ingénierie.** Notebook au début ; modules, tests et outils maison **seulement quand leur intérêt devient concret**.
7. **Rigueur sans lourdeur.** Chronologie respectée, baselines, frais, biais documentés, résultats reproductibles. Une stratégie qui échoue peut être un excellent projet.
8. **Progression flexible.** Les quatre premiers projets donnent la direction ; les suivants peuvent être modifiés selon les découvertes et les centres d'intérêt.

### Ce que l'on évite au début

Docker, cloud, orchestration, dashboards, APIs de brokers, exécution réelle, HFT, infrastructures complexes, deep learning ou reinforcement learning « pour le principe », moteur de backtest complet construit avant d'avoir une stratégie à tester.

## 3. Roadmap — première version

**Le temps est illimité** : aucune estimation de durée ni deadline. La priorité est de comprendre et terminer proprement, sans pression liée au calendrier.

### Phase 1 — Data & Scientific Foundations · **Terminée**

#### P01 — Market X-Ray · **Terminé**

**Question :** Que peut-on apprendre sur un marché en analysant ses prix historiques ?

- **À construire :** récupérer les données historiques de SPY ; calculer rendements, volatilité et drawdowns ; observer leur distribution ; produire des visualisations et conclusions.
- **Compétences :** Python, Pandas, NumPy, Matplotlib, statistiques descriptives, séries temporelles.
- **Livrable :** un notebook reproductible, environ 3 graphiques interprétés, une courte conclusion.
- **Prépare P02 :** réutilisation des données préparées et des calculs de rendements.

#### P02 — Is Momentum Real? · **Terminé**

**Question :** Les performances passées contiennent-elles de l'information sur les rendements futurs ?

- **À construire :** indicateurs de momentum, comparaisons entre périodes/groupes, premières mesures de relation entre signal et résultat futur.
- **Compétences :** rolling windows, vectorisation, corrélations, distributions conditionnelles, estimation.
- **Livrable :** une mini-étude quantitative avec graphiques, hypothèse et conclusion (positive, négative ou non concluante).
- **Prépare P03 :** les indicateurs deviennent des features ML.

### Phase 2 — Machine Learning for Trading · **Terminée**

#### P03 — My First Market Predictor · **Terminé**

**Question :** Peut-on prédire la direction des rendements de la semaine suivante à partir d'informations passées ?

- **À construire :** dataset supervisé (features / cible), baseline naïve, régression logistique, Random Forest ; évaluation sur une période future non vue.
- **Compétences :** feature engineering, classification, scikit-learn, probabilités, métriques, séparation temporelle.
- **Livrable :** comparaison des deux modèles à une baseline, métriques et graphiques, prédictions hors échantillon.
- **Prépare P04 :** les prédictions deviennent les signaux d'une stratégie.

**Point de vigilance dès ce projet :** aucune fuite de données futures dans les features ou le prétraitement ; les prédictions utilisées ensuite doivent être véritablement hors échantillon (séparation chronologique, puis protocole walk-forward si nécessaire).

#### P04 — From ML Predictions to Strategy · **Terminé**

**Question :** Des prédictions ML peuvent-elles se traduire par une stratégie pertinente après frais ?

- **À construire :** convertir probabilités → signaux → positions ; réaliser un **premier backtest vectorisé simple** avec Pandas ; intégrer un décalage d'exécution explicite et des frais approximatifs.
- **Compétences :** stratégie long/cash, equity curve, rendement net, Sharpe, drawdown, turnover, comparaisons économiques.
- **Livrable :** rapport comparant stratégie ML, Buy & Hold et règle de momentum simple, avec limites du backtest.
- **Prépare la suite :** les limites rencontrées indiquent quels tests et composants de backtesting méritent d'être améliorés.

### Phase 3 — Quant Research & Tooling

> **Directions envisagées — peuvent évoluer.** On ne fige pas maintenant leurs sujets, durées ou spécifications.

- **P05 — Can We Trust Our Model?** Robustesse, walk-forward, sensibilité aux frais, surapprentissage, incertitude et reproductibilité.
- **P06 — AI Portfolio Allocation.** Plusieurs ETF, classement des actifs, stratégie de rotation et rééquilibrage mensuel ; covariance et diversification.
- **P07 — Build Your Own Backtesting Lab.** Extraire et fiabiliser les composants du premier backtest dans un petit outil réutilisable, avec tests ciblés. **Seulement si plusieurs projets le justifient.**
- **P08+ — Open Research.** Clustering / régimes de marché, gradient boosting, prévision de volatilité, optimisation de portefeuille ou toute autre piste motivante.

**Règle de révision :** à la fin de P04, faire un bilan des compétences retrouvées, des sujets les plus motivants et des limites rencontrées, puis ajuster P05 et suivants. Éviter d'élargir la roadmap simplement par curiosité.

## 4. Repository GitHub

**Un seul repo public au départ : `quant-lab`.**

Pourquoi : les projets se suivent, les données / features peuvent être réutilisées, et le portfolio raconte une progression cohérente. Un projet particulièrement abouti pourra avoir son propre repo plus tard, **si nécessaire**.

Structure initiale **minimale** :

```text
quant-lab/
├── README.md
├── requirements.txt
└── projects/
    ├── 01-market-x-ray/
    │   ├── README.md
    │   └── main.ipynb
    ├── 02-is-momentum-real/
    │   ├── README.md
    │   └── main.ipynb
    ├── 03-market-predictor/
    │   ├── README.md
    │   └── main.ipynb
    └── 04-ml-trading-strategy/
        ├── README.md
        └── main.ipynb
```

Les dossiers se créent **au fur et à mesure**, pas obligatoirement dès le premier jour. On ajoute des fichiers `.py`, un dossier `tests/`, ou un module partagé **uniquement lorsque la duplication ou la complexité le justifie**.

- **README racine :** objectif du Quant Lab, mini-roadmap, index des projets avec liens et statut.
- **README de projet :** question, hypothèse, méthode, résultats clés, limites et apprentissages.
- **Notebook :** expérimentation exécutable de bout en bout, avec visualisations et commentaires utiles.
- **Données :** privilégier leur téléchargement reproductible ; ne pas versionner de volumineux datasets ou secrets/API keys.
- **Git :** commits réguliers mais sans obligation artificielle d'un commit quotidien.

## 5. Fonctionnement d'un projet (façon école 42, en plus léger)

Chaque projet a **un objectif unique**, divisé en **3 à 6 missions courtes**. Le sujet est posé avant de développer ; la solution complète n'est pas fournie d'emblée.

### Format du sujet

1. **Question / mission** — Ce qu'on essaie de découvrir ou de construire.
2. **Résultat attendu** — Graphique, modèle, stratégie, métriques, etc.
3. **Contraintes** — Données, horizon, règles pour éviter les biais, outils autorisés.
4. **Missions** — Étapes concrètes, de taille libre ; ajustables si besoin.
5. **Notions utiles** — Quelques concepts à (re)découvrir, avec ressources ciblées au moment nécessaire.
6. **Critères de validation** — Vérifications objectives + interprétation scientifique.

### Rendu minimal

- **`main.ipynb` :** code fonctionnel et résultats.
- **`README.md` :** 5 rubriques courtes : question, hypothèse, méthode, résultats, limites / suite.
- **Éventuels fichiers `.py` / tests :** seulement quand c'est réellement utile.

Un projet n'est **pas validé** simplement parce qu'il produit un joli graphique. On vérifie la logique du code, la chronologie des informations, la comparaison à une baseline quand elle est pertinente et la justesse de la conclusion. Inversement, une expérience qui ne produit aucun avantage statistique ou financier peut être **validée** si elle est rigoureuse.

## 6. Rythme

**Objectif :** une activité agréable et régulière. Le temps est illimité : la durée d'une séance et d'un projet n'est pas une contrainte.

- Relire l'objectif de la mission en cours.
- Coder, expérimenter, chercher, déboguer.
- Noter ce qui a été observé et le prochain pas.

Ce sont des repères, pas un rituel strict ; une journée manquée n'appelle pas de rattrapage.

### Notre règle d'interaction

**Quand je demande une nouvelle mission :** donner la question, les objectifs, les contraintes et les critères de réussite ; ne pas divulguer automatiquement la solution complète.

**Quand je bloque :** commencer par une piste ou une explication ; approfondir seulement si nécessaire.

**Quand je termine :** revue courte du code et de l'interprétation ; verdict « validé / à améliorer » avec raisons concrètes ; puis projet ou mission suivante.

## 7. Références pédagogiques (bibliothèque, pas cursus à suivre)

Ces ressources servent de documentation ciblée et d'inspiration, **sans obligation de les suivre du début à la fin** :

- [DataTalks.Club — Stock Markets Analytics Zoomcamp](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp) : progression données → features → ML → stratégie.
- [Georgia Tech — Machine Learning for Trading](https://lucylabs.gatech.edu/ml4t/) : projets cumulatifs et recherche quantitative.
- [Stefan Jansen — Machine Learning for Trading](https://github.com/stefan-jansen/machine-learning-for-trading) : feature engineering et validation.
- [DataTalks.Club — Machine Learning Zoomcamp](https://github.com/DataTalksClub/machine-learning-zoomcamp) : révision du ML appliqué.
- [QuantStart — Event-Driven Backtesting](https://www.quantstart.com/articles/Event-Driven-Backtesting-with-Python-Part-I/) : inspiration pour un futur moteur maison.

