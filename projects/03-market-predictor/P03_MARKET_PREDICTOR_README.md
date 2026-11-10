# P03 — My First Market Predictor

**Quant Lab · Projet 03 · Machine Learning for Trading**

> **Statut** — À commencer  
> **Durée** — Libre. La roadmap donne un repère de 4 à 6 séances.  
> **Prérequis** — Savoir calculer un rendement et aligner un signal avec le rendement de la période suivante, comme dans P02.

## 1. Mission

**Peut-on prédire si SPY va monter la semaine suivante avec les informations disponibles à la clôture de cette semaine ?**

Dans P02, tu comparais des fréquences de hausse selon le signe du mois précédent. Ici, tu combines plusieurs informations passées pour produire une prédiction chaque semaine. Tu compares deux modèles, une **régression logistique** et une **forêt aléatoire**, à une référence simple sur une période qu'ils n'ont jamais vue à l'entraînement.

## 2. Périmètre

- **Données** — Clôtures journalières ajustées de SPY depuis 2015, comme dans P01. Note la date de fin retenue.
- **Fréquence** — Une ligne par semaine de cotation complète. Prends la clôture de la dernière séance réellement cotée. Lors d'une semaine écourtée par un jour férié, cette séance peut être un jeudi.
- **Date du signal** — La dernière séance de la semaine courante, après sa clôture.
- **Cible** — Le rendement entre la clôture de cette séance et la clôture de la dernière séance de la semaine suivante. Attribue la classe **1** si le rendement est strictement positif, et **0** sinon.
- **Outils** — `main.ipynb`, avec `pandas`, `numpy`, `matplotlib`, `scikit-learn` et `yfinance` si tu dois télécharger les données à nouveau.

## 3. Missions

### Mission 1 — Construire le tableau hebdomadaire

**Objectif** — Obtenir une ligne par semaine complète et vérifier la cible.

- Reprends tes clôtures ajustées, vérifie les dates et garde la dernière clôture de chaque semaine complète. Écarte la dernière semaine si le téléchargement s'arrête avant sa dernière séance de cotation.
- Calcule les rendements hebdomadaires. Comme dans P02, associe à la ligne de la semaine courante le rendement de la semaine suivante.
- Garde la **date réelle du signal** et la **date de la clôture suivante**. Vérifie à la main deux paires de dates et leur classe.
- Retire les lignes sans rendement futur connu avant de convertir la cible en 0 ou 1. Vérifie qu'un décalage ne saute pas une semaine manquante.

**Terminé lorsque** tu peux expliquer la différence entre la date où tu émets la prédiction et celle où tu observes le résultat.

### Mission 2 — Créer les variables d'entrée

**Objectif** — Réutiliser les calculs de P01 et P02 pour décrire ce que tu sais au moment du signal.

- Commence avec trois variables (*features*) simples. Prends le rendement de la semaine courante, le rendement cumulé sur les quatre dernières semaines et l'écart-type des quatre derniers rendements hebdomadaires.
- Vérifie qu'à chaque date, les fenêtres se terminent au plus tard à la clôture du signal. Le rendement futur et sa classe restent à part.
- Écarte les lignes où les fenêtres n'ont pas assez d'historique. Fixe et note une date de séparation chronologique avant de comparer les modèles.
- Entraîne sur la période la plus ancienne et teste sur la plus récente. Vérifie que tous les résultats utilisés pour l'entraînement étaient connus avant le premier signal de test. Si une ligne traverse la coupure, retire-la de l'entraînement.

**Terminé lorsque** tu peux indiquer quelles dates et colonnes sont disponibles au moment de chaque prédiction de test.

### Mission 3 — Construire la référence et entraîner les modèles

**Objectif** — Donner aux deux modèles un point de comparaison commun.

- Identifie la classe la plus fréquente **dans l'entraînement**. Ta référence prédit toujours cette classe.
- Entraîne une régression logistique et une forêt aléatoire sur les mêmes variables. Si tu standardises les variables pour la régression, calcule les paramètres de standardisation sur l'entraînement seul. Une `Pipeline` peut regrouper ces étapes.
- Commence avec des réglages simples. Fixe une graine aléatoire pour la forêt et garde la période de test à part jusqu'à l'évaluation.

**Terminé lorsque** tu peux produire les prédictions de la référence et des deux modèles sur les mêmes semaines de test.

### Mission 4 — Évaluer sur la période de test

**Objectif** — Voir ce que chaque modèle apporte sur des semaines hors échantillon, c'est-à-dire des semaines qu'il n'a pas vues à l'entraînement.

- Compare la **proportion de prédictions correctes** (*accuracy*) des trois approches. Indique le nombre de semaines et la fréquence réelle des hausses sur le test.
- Regarde les matrices de confusion. Combien de hausses et de semaines nulles ou négatives chaque approche a-t-elle correctement reconnues ?
- Fais un graphique comparant les scores des trois approches sur la même période. Donne les écarts avec la référence en points de pourcentage.
- Conserve un tableau avec la date du signal, la date du résultat, la classe observée, la classe prédite et la probabilité de hausse donnée par chaque modèle. Ce tableau servira dans P04.

**Terminé lorsque** tu peux expliquer les erreurs de chaque modèle et montrer à quelles dates ses prédictions auraient été disponibles.

### Mission 5 — Conclure et préparer P04

**Objectif** — Dire ce que l'expérience montre sur la capacité des modèles à prédire le signe du rendement.

- Résume quel modèle obtient le meilleur score sur le test et s'il dépasse la référence. Un résultat faible ou non concluant est un résultat valable.
- Discute le nombre de semaines testées et le fait que l'étude porte sur un seul ETF. Si tu veux essayer d'autres variables ou réglages, utilise une validation chronologique sur la partie entraînement.
- Explique ce que P04 devra encore mesurer. Ici, tu évalues la direction des rendements. Dans P04, tu vérifieras si les probabilités obtenues peuvent servir à construire une stratégie et comment les frais et le moment d'exécution influencent le résultat.

## 4. Livrables

- `main.ipynb` avec le tableau hebdomadaire, la séparation chronologique, la référence, les modèles, le graphique et la conclusion.
- Un tableau de prédictions hors échantillon dans le notebook, daté et réutilisable pour P04.
- Ce README avec le journal de recherche complété.

## 5. Critères de validation

- [ ] Deux lignes du tableau sont vérifiées à la main, avec la bonne semaine pour le signal et pour le résultat.
- [ ] Chaque variable d'entrée est connue à la date du signal.
- [ ] La séparation est chronologique, les cibles d'entraînement sont connues avant le premier signal de test et le prétraitement est ajusté sur l'entraînement seul.
- [ ] La référence et les deux modèles sont comparés sur les mêmes semaines de test.
- [ ] Les probabilités de hausse hors échantillon sont datées et conservées pour P04.
- [ ] Le notebook s'exécute du début à la fin et la conclusion distingue la performance de classification de la performance d'une stratégie.

## 6. Repères pour démarrer

- **Pandas** — Reprends `resample()`, `pct_change()` et `shift()` de P02. Les calculs glissants de P01 te servent pour les variables sur quatre semaines.
- **Scikit-learn** — Cherche `DummyClassifier` pour la référence, `LogisticRegression` et `RandomForestClassifier` pour les modèles.
- **Évaluation** — Cherche `accuracy_score` et `confusion_matrix`. Regarde aussi `predict_proba()` et l'ordre des classes pour identifier la probabilité de hausse.

## 7. Journal de recherche

Jordi : J'ai tester des approches simple dans un premier temps poru revoir les basses, l'objectif c'etait de voir si des approches simpel de prediction pouvais donner des choses cool, j'ai utiliser trois features, les rendements de la semaines, rendements des 4 derniers semaines cumulmé et la volat glissante des rendements des 4 dernieres semaines. J'ai utliser une regression logistique , Random forest et une approche naive qui consiste a buy chaque début de semaine. La questioin c'était de savoir si faire des petits models de prediction avec ces trois features aller me permettre de faire mieux que l'approche naive qui est de buy en début de semaine. In fine la regression logistique de change rien car elle nosu dis de buy chaque début de semaine CEPENDANT la random forest ce qui est intéréssant c'est que ici a certain moment elle nous dis de pas acheter, au final en creusant un peu elle nous permet a certain moment d'esquiver des semaiens down mais elle nous fait rater beaucoup de hausse donc avec des faux négatif donc en gros elle nous dis que ca baisse alors que non.. C'est intéréssant de voir que au final la random forest a mis son nez la dedans et en terme de résultat de prediction, la random forest predit moins souvent ducoup la bonne réponse qui est de savoir si la semaines suivante aura un rendement positif ou négatif

---

**Règle Quant Lab** — Tu écris le code et interprètes les résultats. L'IA t'aide à clarifier les étapes, à trouver les outils utiles et à relire ton travail.
