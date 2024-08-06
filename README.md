# PoC_Cyber_sequential_learning
PoC for sequential unsupervised anomaly detection

# Proof Of Concept - détection anomalies en apprentissage non supervisé séquentiel

Ce notebook est une recherche d'apprentissage non supervisé séquentiel sur un gros dataset : le Los Alamos Natioanal Laboratory "Unified Host and Network Dataset" - ce dataset est l'enregistrement pendant 90 jours de logs machines dans le SI du LANL. Le dataset comporte 90 fichiers, chaque fichier une fois décompressé contient entre 50 et 60 millions de lignes JSON, chacune représentant un évènement machine.

# Idées et recherches :

Le dataset est trop important pour être chargé en mémoire. L'apprentissage est nécessairement séquentiel.
Le dataset n'est pas labelisé, l'apprentissage est nécessairement non supervisé.

Les données sont tabulaires, il y a 22 features. (voir la partie EDA)

Le dataset est traité par batch dans un VAE. Les poids du VAE sont la "mémoire" de l'apprentissage, avec un drift que l'on approchera par une représentation graphique de l'espace latent, choisi en 2D à des fins de représentation.

Chaque nouveau batch est projeté dans l'espace latent. La distribution empirique de probabilité de cette projection est approchée par un KDE (Kernel Density Estimation), et par un modèle Bayesien de Gaussian Mixture Model, ce qui permet d'attribuer une densité de probabilité à chaque point du batch. Cette pdf peut être utilisée pour quantifier une détection.

### Article : https://www.worldscientific.com/doi/pdf/10.1142/9781786345646_001

Notes:

"The events from the host logs included in the data set are all related to authentication and process activity on each machine"

# Conclusions

La difficulté principale de l'apprentissage non supervisé séquentiel en cyber est de garder la mémoire de la distribution de probabilité du training set, sans pouvoir garder en mémoire tous les points constituant ce training set.

Idéalement, cette vue "agrégée" de la distribution du training set doit être mise à jour au fur et à mesure des batchs de nouveaux points, pour minimiser autant que faire se peut le drift. La solution dans ce notebook est de fitter un VAE.

On peut alors calculer une densité de probabilité de chaque nouveau point dans cette distrIbution agrégée, pour juger d'une éventuelle anomalie.
