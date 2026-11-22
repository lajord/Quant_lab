# Deep Momentum Networks — Reproduire et éprouver un trend-following appris par deep learning

**Quant Lab · Projet phare**

> **Statut** — À commencer  
> **Rythme** — Sans limite de temps : le projet avance objectif par objectif.  
> **Paper reproduit** — Lim, Zohren, Roberts (2019), *Enhancing Time Series Momentum Strategies Using Deep Neural Networks*, The Journal of Financial Data Science.

## 1. La question

**Un réseau de neurones entraîné à maximiser directement le ratio de Sharpe fait-il mieux que le trend-following classique sur un portefeuille diversifié de futures ? Et ce résultat tient-il encore dans les années qui ont suivi la publication du paper ?**

Le projet se déroule en trois temps :

1. **Reproduire** le paper avec les mêmes données et le même protocole, puis comparer tes résultats aux siens sur 1995–2015.
2. **Éprouver** la méthode hors de la période étudiée par les auteurs, de 2016 à aujourd'hui.
3. **Aller plus loin** que le paper : tester, une à une, des hypothèses qu'il n'explore pas (attention, ruptures de tendance, perte calculée sur le portefeuille).

## 2. D'où vient ce projet

- **P02** a montré que le momentum sur un seul actif donne un écart faible et fragile.
- **P03** a montré qu'un classifieur hausse/baisse est difficile à rendre utile (49 % de bonnes réponses pour la forêt aléatoire).
- **P04** a montré qu'une prédiction ne se transforme pas facilement en stratégie.

Le paper prend le problème à l'envers. Le réseau ne prédit pas une direction : il **sort directement une position**, et il est entraîné sur **la métrique qui compte pour la stratégie**, le ratio de Sharpe. Il travaille sur un portefeuille **multi-actifs**, car le trend-following tire sa force de la diversification.

## 3. Ce que dit le paper

- **Données** : 88 contrats futures continus ajustés par ratio, couvrant matières premières, obligations, devises et indices actions. Prix **journaliers** de 1990 à 2015, issus de la base CLC de Pinnacle Data.
- **Idée** : le réseau apprend en même temps la tendance et la taille de la position. Les positions sont mises à l'échelle pour viser une volatilité annuelle de 15 %.
- **Entraînement** : la fonction de perte est l'opposé du Sharpe annualisé des rendements de la stratégie. Le modèle est réentraîné tous les 5 ans sur tout l'historique disponible.
- **Références** : long-only, momentum de Moskowitz, Ooi et Pedersen (signe du rendement sur 12 mois), signaux MACD de Baz et al. (2015).
- **Résultat principal** : sans frais, le LSTM optimisé sur le Sharpe fait plus que doubler le Sharpe des méthodes classiques. Son avantage tient jusqu'à des frais d'environ 2 à 3 points de base. Une pénalité de turnover ajoutée à la perte permet au réseau de tenir compte des frais.

## 4. Données

- **Fréquence** : journalière, comme dans le paper. Aucune donnée intraday n'est nécessaire.
- **Source** : la base **CLC de Pinnacle Data Corp**, celle du paper. Elle est payante (environ 99 $ d'après des utilisateurs en 2023 ; prix actuel à confirmer sur le site officiel). Ses contrats continus ajustés par ratio remontent jusqu'à 1969 selon les marchés.
- **Univers** : reconstitue les 88 contrats du paper à partir de sa description des données, et documente chaque différence.
- **Période** : de 1990 à la dernière date disponible. La période 1995–2015 sert à la comparaison avec le paper ; 2016 à aujourd'hui sert au test post-publication.
- **Ajustement** : garde les séries ajustées par ratio, comme le paper. Elles conservent les rendements en pourcentage.
- **Licence** : les données sont sous licence. Ne les versionne pas sur GitHub : le repo contient le code qui les prépare, pas les fichiers.

## 5. Objectifs

Chaque objectif se termine par un résultat visible. L'ordre est conseillé, pas imposé.

### Objectif 1 — La fiche de réplication

Lis le paper en entier et rédige une fiche qui liste toutes ses décisions : univers, calcul des rendements et de la volatilité, features, cible, architecture, fonction de perte, découpage temporel, hyperparamètres, références, métriques et frais. Pour chaque point, note la section ou l'équation du paper, et signale ce qui n'est pas précisé.

**Terminé quand** quelqu'un pourrait reproduire le paper à partir de ta fiche seule.

### Objectif 2 — Les données et les stratégies de référence

Prépare les séries de l'univers, la volatilité ex ante et les trois stratégies de référence : long-only, momentum 12 mois et MACD. Toutes sont mises à l'échelle à la même volatilité cible.

**Terminé quand** tu obtiens les courbes de capital et les Sharpe des trois références sur 1995–2015, et que tu peux les mettre en face de ceux du paper.

### Objectif 3 — Le Deep Momentum Network

Construis le pipeline d'apprentissage : features, LSTM qui sort une position, perte Sharpe, entraînement sur une fenêtre croissante avec réentraînement tous les 5 ans, validation, early stopping et recherche d'hyperparamètres.

**Terminé quand** le modèle produit des positions hors échantillon sur 1995–2015, sans aucune information future dans les features, la normalisation ou le choix des hyperparamètres.

### Objectif 4 — Reproduire les conclusions du paper

Compare ton modèle aux références, puis aux autres fonctions de perte étudiées dans le paper, dont la classification hausse/baisse que tu utilisais dans P03. Ajoute ensuite les frais, calculés sur le turnover, et la pénalité de turnover dans la perte.

**Terminé quand** tu as un tableau qui met tes résultats en face de ceux du paper, avec une explication argumentée de chaque écart. Retrouver les mêmes conclusions compte plus que retrouver les mêmes chiffres.

### Objectif 5 — Le test post-publication

Applique exactement le même protocole de 2016 à aujourd'hui : krach du Covid en 2020, hausse des taux en 2022, puis les années suivantes.

**Terminé quand** tu peux répondre, chiffres à l'appui : l'avantage du modèle survit-il à sa publication ?

### Objectif 6 — Aller plus loin que le paper

Le paper s'arrête au LSTM et à l'année 2015. Teste des hypothèses qu'il n'explore pas, **une à la fois**, contre ton DMN reproduit : mêmes données, même protocole, plusieurs graines, résultats avant et après frais, sur 1995–2015 et après 2015. L'ordre est libre ; H1 est le point de départ naturel.

**H1 — L'attention fait mieux que la mémoire du LSTM.** Le LSTM résume le passé dans une mémoire qui s'efface peu à peu. L'attention relie chaque jour directement à tous les jours passés : elle pourrait mieux capter les tendances longues et les changements de régime.

- *Test* : remplace le LSTM par un Transformer, sans rien changer d'autre. Si besoin, essaie ensuite un hybride LSTM + attention, comme le Momentum Transformer.
- *Point critique* : si le modèle sort une position pour chaque jour de la séquence, l'attention doit être **causale** : un jour ne voit jamais les jours suivants.
- *À surveiller* : le surapprentissage (peu de données, beaucoup de bruit) et les jours passés sur lesquels l'attention se concentre.

**H2 — Détecter les ruptures de tendance limite les pertes aux retournements.** Le trend-following perd surtout quand une tendance se retourne. Un signal de rupture en entrée pourrait aider le réseau à réduire ou inverser sa position plus vite.

- *Test* : ajoute au DMN des features de détection de rupture (*changepoint detection*), sans rien changer d'autre.
- *À mesurer* : tes pires drawdowns (H2 les réduit-elle ?) et le Sharpe après 2015.

**H3 — Une perte calculée sur le portefeuille apprend la diversification.** La perte Sharpe du paper mélange tous les couples (actif, jour) : elle ignore les corrélations entre actifs. Un Sharpe calculé sur la série quotidienne du portefeuille pourrait pousser le réseau à éviter les paris corrélés.

- *Test* : change uniquement la perte.
- *À mesurer* : le Sharpe et le drawdown du portefeuille, et les corrélations entre positions.

**H4 (facultative, avancée) — S'adapter vite à un nouveau régime.** Le modèle s'appuie sur des épisodes passés similaires pour s'adapter (*few-shot learning*, X-Trend). À garder pour la fin.

Combine plusieurs hypothèses seulement après avoir mesuré l'effet de chacune séparément.

**Terminé quand** chaque hypothèse testée a une réponse argumentée (elle aide, elle n'aide pas, ou c'est inconclusif) et une explication du pourquoi.

### Objectif 7 — Le rapport

Rédige un rapport court, au format d'un mini-paper : question, méthode, réplication, test post-publication, hypothèses testées au-delà du paper et limites. Ce README résume ensuite les résultats clés avec les graphiques principaux.

**Terminé quand** un recruteur comprend en 5 minutes ce que tu as fait, ce que tu as trouvé et pourquoi c'est crédible.

## 6. Règles scientifiques

Peu de règles, mais non négociables :

- **Chronologie** : une position décidée à la date t n'utilise que des informations disponibles à t, et elle est jugée sur le rendement qui suit.
- **Périodes de test intouchables** : elles ne servent jamais à entraîner, à normaliser ni à choisir des hyperparamètres.
- **Même protocole pour tous** : mêmes actifs, mêmes périodes, même volatilité cible, mêmes frais.
- **Hasard du deep learning** : un résultat obtenu avec une seule graine aléatoire ne prouve rien. Rapporte la moyenne et la dispersion sur plusieurs entraînements.
- **Frais** : un seul paramètre en points de base, appliqué au turnover. Rapporte les résultats avant et après frais.
- **Conclusion** : un backtest favorable ne prouve pas une rentabilité future.

## 7. Livrables

- Un repo qui régénère tous les résultats à partir des données brutes.
- Du code : des notebooks pour explorer, puis des modules Python dès que le pipeline (données, features, modèle, backtest) devient réutilisable.
- Les graphiques clés : courbes de capital, tableau des métriques face au paper, comportement en 2008, 2020 et 2022, sensibilité aux frais.
- Le rapport de l'objectif 7, et ce README complété avec les résultats.

## 8. Critères de validation

Le projet est validé quand :

- la fiche de réplication est complète et sourcée ;
- les références et le modèle tournent de bout en bout, sans fuite d'information ;
- chaque écart avec le paper est expliqué, ou honnêtement signalé comme inexpliqué ;
- le test post-publication est rapporté tel qu'il sort, favorable ou non ;
- chaque hypothèse de l'objectif 6 est mesurée contre le DMN reproduit, avec le même protocole, et rapportée telle qu'elle sort ;
- le rapport permet de refaire l'expérience.

Un modèle qui ne bat pas les références peut faire un excellent projet si la démarche est rigoureuse.

## 9. Ressources

**À lire en entier**

- [Lim, Zohren, Roberts (2019) — Enhancing Time Series Momentum Strategies Using Deep Neural Networks](https://arxiv.org/abs/1904.04912) : le paper reproduit. Les sections III à VI sont essentielles ; les annexes servent à la fiche de réplication.

**À lire en partie**

- [Moskowitz, Ooi, Pedersen (2012) — Time Series Momentum](https://papers.ssrn.com/abstract=2089463) : sections 1, 2.1, 2.4, 3 et début de la section 4. Les sections 5 et 6 ne servent pas au projet.

**Facultatif**

- [Baz et al. (2015) — Dissecting Investment Strategies in the Cross Section and Time Series](https://cmegroup.com/education/alternative-investment-resource-center/research/dissecting-investment-strategies.html) : le paper de 2019 reprend ses formules MACD dans sa section III-A (équations 4 à 8).

**Pour l'objectif 6** — le résumé suffit pour l'instant ; lecture complète au moment de tester l'hypothèse correspondante.

- **H1** — [Wood et al. (2022) — Trading with the Momentum Transformer](https://arxiv.org/abs/2112.08534) : un hybride LSTM + attention, par la même équipe que le DMN.
- **H1** — [Vaswani et al. (2017) — Attention Is All You Need](https://arxiv.org/abs/1706.03762) : l'article fondateur du Transformer. [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) en donne une explication visuelle.
- **H2** — [Wood, Roberts, Zohren (2021) — Slow Momentum with Fast Reversion](https://arxiv.org/abs/2105.13727) : un module de détection de ruptures ajouté au DMN.
- **H3** — [DeePM (2026) — Regime-Robust Deep Learning for Systematic Macro Portfolio Management](https://arxiv.org/abs/2601.05975) : un apprentissage au niveau du portefeuille.
- **H4** — [Wood et al. (2024) — X-Trend : few-shot learning pour le trend-following](https://arxiv.org/abs/2310.10500)

**Outils**

- [PyTorch — `nn.LSTM`](https://pytorch.org/docs/stable/generated/torch.nn.LSTM.html)
- [PyTorch — différentiation automatique](https://pytorch.org/tutorials/beginner/basics/autogradqs_tutorial.html) : utile pour comprendre comment une perte Sharpe se dérive.
- [PyTorch — `nn.TransformerEncoder`](https://pytorch.org/docs/stable/generated/torch.nn.TransformerEncoder.html) : pour H1 ; regarde comment lui passer un masque causal.

**Code de référence des auteurs** — à ouvrir seulement après avoir obtenu tes propres résultats, pour comparer :

- [kieranjwood/trading-momentum-transformer](https://github.com/kieranjwood/trading-momentum-transformer) (licence MIT)

## 10. Journal de recherche

### Hypothèse de départ

_À compléter avant le premier entraînement._

### Réplication (1995–2015)

_À compléter après l'objectif 4._

### Test post-publication (2016 à aujourd'hui)

_À compléter après l'objectif 5._

### Au-delà du paper : hypothèses testées et résultats

_À compléter pendant l'objectif 6, une entrée par hypothèse._

### Limites et suite

_À compléter à la fin._

---

**Règle Quant Lab** — Tu écris le code et interprètes les résultats. L'IA t'aide à clarifier les étapes, à trouver les outils utiles et à relire ton travail.
