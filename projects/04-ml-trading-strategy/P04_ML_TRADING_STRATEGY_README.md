# P04 — From ML Predictions to Strategy

**Quant Lab · Projet 04 · Machine Learning for Trading**

> **Statut** — À commencer  
> **Durée** — Libre. La roadmap donne un repère de 4 à 6 séances.  
> **Prérequis** — Avoir les prédictions hors échantillon de P03 et savoir calculer une courbe de capital et son drawdown, comme dans P01.

## 1. Objectif

**Si j'utilise les prédictions de P03 pour choisir entre SPY et le cash, comment évolue mon capital après frais par rapport au buy & hold et à une règle de momentum simple ?**

En P03, chaque bonne prédiction comptait pour un point. Ici, chaque décision a un effet différent sur le capital. Éviter une baisse de 5 % et rater une hausse de 1 % sont deux décisions dont tu peux désormais mesurer l'impact. Tu vas construire un premier **backtest**, une simulation qui rejoue ces décisions sur les données historiques.

## 2. Règles du backtest

- **Instrument et période** — SPY, sur les semaines de test de P03 pour lesquelles tu disposes d'un prix d'entrée et d'un prix de sortie. Utilise exactement les mêmes intervalles pour toutes les stratégies.
- **Position** — À chaque décision, le portefeuille est soit investi à 100 % dans SPY, soit en cash. Le rendement du cash est fixé à 0 % dans cet exercice.
- **Signal et exécution** — Le signal devient disponible à la clôture de la dernière séance de la semaine. Applique la nouvelle position à l'ouverture de la séance suivante. Mesure le rendement jusqu'à l'ouverture qui suit la dernière séance de la semaine suivante.
- **Prix** — Utilise les ouvertures journalières ajustées de SPY pour les rendements du backtest. Les clôtures journalières permettent de vérifier à quelle séance correspond chaque clôture hebdomadaire de P03.
- **Frais** — Pour la simulation, fixe **0,10 % du capital par passage entre SPY et cash**, à l'exécution. Une entrée et une sortie coûtent chacune 0,10 % sur le capital au moment du changement. Une position conservée d'une semaine à l'autre n'entraîne aucun nouveau frais.
- **Capital** — Toutes les stratégies commencent à 100 en cash. Le buy & hold paie son entrée. À la fin, valorise les positions encore ouvertes au dernier prix disponible retenu pour le test.

### Stratégies à comparer

| Stratégie | Règle connue à la clôture de la semaine du signal |
|---|---|
| Forêt aléatoire | SPY si la probabilité de hausse de P03 dépasse 50 %, cash sinon. |
| Régression logistique | Même règle avec la probabilité de la régression logistique. |
| Buy & hold | Investi dans SPY à partir de la première ouverture de la période. |
| Momentum simple | SPY si le rendement de la semaine qui vient de se terminer est positif, cash sinon. |

Fixe ces règles et les frais avant de regarder les résultats. La comparaison porte sur les signaux déjà calculés hors échantillon en P03.

## 3. Missions

### Mission 1 — Relier chaque prédiction à un prix exécutable

**Objectif** — Construire le calendrier du backtest à partir de `resultat_test` de P03.

- Récupère les probabilités de hausse et `rendement_w` pour les semaines de test. Garde l'ordre des dates.
- Associe chaque étiquette de semaine à la date réelle de la clôture utilisée pour le signal. Compare deux clôtures hebdomadaires de P03 aux clôtures journalières, idéalement sur une semaine ordinaire et une semaine écourtée. Si les prix ou les semaines ne concordent pas, résous l'alignement avant de simuler les positions.
- Pour chaque signal, relève la date et le prix de la prochaine ouverture, puis ceux de l'ouverture suivante à laquelle une décision serait appliquée. Si le dernier intervalle n'a pas de prix de sortie, retire-le de toutes les stratégies. Vérifie aussi qu'une semaine manquante ne transforme pas un rendement hebdomadaire en rendement sur plusieurs semaines.
- Calcule le rendement de SPY entre les deux ouvertures. Dans P03, `rendement_w1` compare deux **clôtures** pour vérifier la direction. Pour le backtest, utilise le rendement entre les deux **ouvertures**.

**Terminé lorsque** tu peux expliquer sur deux lignes quand le signal était connu, quand la position pouvait changer et quels prix donnent le rendement.

### Mission 2 — Transformer les signaux en positions

**Objectif** — Obtenir une position par stratégie et par intervalle.

- Applique les quatre règles du tableau. Utilise `1` pour SPY et `0` pour le cash.
- Affiche quelques semaines de la forêt côte à côte avec la probabilité, la position et le rendement de SPY sur l'intervalle exécuté.
- Vérifie à la main un passage en cash et un retour dans SPY. Une ligne en cash a un rendement brut de 0 %. Une ligne investie reprend le rendement de SPY sur cet intervalle.

**Terminé lorsque** chaque position s'applique à un rendement qui commence après la publication du signal.

### Mission 3 — Construire les courbes de capital après frais

**Objectif** — Mesurer le rendement brut, le coût des changements de position et le rendement net.

- Compare chaque position à celle de l'intervalle précédent. Leur écart absolu mesure la part du portefeuille échangée. Pour la première ligne, la position précédente vaut 0.
- Déduis les frais au moment du changement, puis applique le rendement de la position au capital restant. Par exemple, un passage de cash à SPY avec un capital de 100 laisse **99,90** investis avant le rendement de la période.
- Vérifie à la main la suite de positions `0 → 1 → 1 → 0`. Elle comporte deux transactions. Trace ensuite les courbes de capital en base 100 avec le produit cumulé des facteurs de rendement net, comme dans P01. Garde le point initial à 100 avant les premiers frais pour le calcul du drawdown.

**Terminé lorsque** tu peux retrouver à la main le rendement net et le capital sur quelques lignes, y compris une sortie vers le cash.

### Mission 4 — Comparer la performance et le risque

**Objectif** — Voir ce que chaque stratégie a apporté en rendement, en risque et en transactions.

- Trace les quatre courbes de capital net sur les mêmes dates. Si deux courbes se superposent, vérifie si leurs positions sont identiques.
- Fais un tableau avec le **rendement cumulé net**, le **drawdown maximal**, le **ratio de Sharpe** et le **nombre de changements de position**. Pour le Sharpe, indique la convention utilisée, par exemple un taux sans risque nul et une annualisation des rendements hebdomadaires avec un facteur `√52`.
- Compare le temps passé dans SPY et le nombre de transactions. Regarde sur quelques semaines si les sorties ont surtout évité des baisses ou fait rater des hausses.

### Mission 5 — Conclure

**Objectif** — Répondre à la question de départ avec tes chiffres.

- Dis si les signaux ML ont amélioré le rendement net ou réduit le drawdown par rapport au buy & hold et au momentum sur cette période.
- Relie le résultat à P03. Quelle différence observes-tu entre le taux de bonnes prédictions et l'effet de ces décisions sur le capital ?
- Note les limites de la simulation. Les résultats dépendent des dates, de SPY et des hypothèses d'exécution, de frais et de rendement du cash.

## 4. Livrables

- `main.ipynb` avec les dates de signal et d'exécution, les positions, les rendements nets, les courbes de capital et le tableau comparatif.
- Ce README avec le bilan du test dans le journal de recherche.

## 5. Critères de validation

- [ ] Les prédictions viennent de la période de test de P03 et chaque signal précède l'ouverture à laquelle il est utilisé.
- [ ] Deux lignes sont vérifiées à la main, de la date du signal au rendement entre ouvertures.
- [ ] Les frais sont appliqués aux changements de position, y compris l'entrée initiale depuis le cash.
- [ ] Les quatre stratégies sont évaluées sur les mêmes intervalles et avec les mêmes hypothèses de frais.
- [ ] Le notebook se relance de bout en bout et le bilan explique les écarts de rendement net, de drawdown et de transactions.

## 6. Repères pour démarrer

- **P03** — Repars de `resultat_test` pour les dates, les probabilités et le rendement connu à la date du signal.
- **Pandas** — Réutilise `shift()` pour comparer deux positions successives, `cumprod()` pour le capital et `cummax()` pour le drawdown.
- **P01** — Reprends ton calcul de drawdown sur une courbe en base 100.

## 7. Journal de recherche

### Hypothèse de départ

_À compléter avant la comparaison._

### Résultats après frais

_À compléter après le backtest._

### Limites et prochaine question

_À compléter à la fin._

---

**Règle Quant Lab** — Tu écris le code et interprètes les résultats. L'IA t'aide à clarifier les étapes, à trouver les outils utiles et à relire ton travail.
