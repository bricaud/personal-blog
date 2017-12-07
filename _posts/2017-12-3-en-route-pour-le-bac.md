---
layout: post
comments: true
title: En route pour le Bac
categories: [lycee, statistiques]
thumbnail: /images/lycee/loinormale.svg
published: true
---


*For my english readers, this post is in French. It is talking about student data from the French education system so I thought it would be better to write it in French.*
L'analyse des notes au cours du temps en dit long sur le comportement des élèves. C'est ce que nous allons voir dans cet article de blog. J'avais à disposition les notes d'élèves de la filière Bac Pro d'un lycée, donc les classes de seconde, première et terminale Bac Pro. Cette filière est particulière dans le sens ou il y a en proportion plus d'élèves en difficulté avec le système scolaire standard, ce qui les poussent à choisir un débouché plus rapide dans la vie active et donc à s'orienter en Bac Pro. Je les ai eu en classe pendant 2 mois et en regardant les notes de leur contrôles je dois dire que j'étais un peu perplexe. En effet, la distribution des notes autour de la moyenne est singulière. Elle semble évoluer d'une forme Gaussienne en seconde vers une séparation en 2 groupes (2 gaussiennes) en terminale. Comme j'adore l'analyse de données et que j'ai tous les outils à ma disposition avec Python, j'ai plongé dans l'analyse des notes. J'ai utilisé non seulement des statistiques que je leur ai enseigné (au programme du Bac Pro), mais c'était aussi l'occasion d'utiliser mes algorithmes favoris d'apprentissage automatique et intelligence artificielle, ici le modèle de mélange gaussien. Les résultats et l'analyse en elle-même sont intéressants et c'est pour ca que je le partage. Voilà ce que j'ai découvert... 



# Introduction

Comme mon entreprise a encore du mal à décoller, j'ai décidé de chercher des alternatives. Et récemment je suis tombé sur l'annonce d'un lycée qui cherchait un prof de math à mi-temps pour 2 mois. Ca me semblait parfait pour me changer un peu les idées et sortir du code informatique que je voyais toute la journée. 

C'est une drôle d'expérience pour moi. Je dois avouer que je n'étais pas préparé à ca. J'ai appris beaucoup de choses durant ces 2 mois au contact des élèves. J'ai pu remarquer une première chose, les élèves n'ont pas le niveau de maturité auquel je m'attendais. Je les ai pris pour des adultes, je leur ai donné trop de liberté, et certain en ont profité. A la fin, bien sur, cela se voit sur les notes. J'ai trouvé un niveau très hétérogène qui va des bons élèves sérieux et motivés jusqu'aux élèves en grande difficultés et totalement fermés aux mathématiques. Ces derniers ont toujours été en échec en mathématiques et ont baissé les bras, comme ils me l'ont dit en classe. Je me suis demandé comment ces personnes là évolueraient à l'approche du Bac. Prendraient-elles conscience de l'enjeu et se mettraient-elles au travail en classe de première ou de terminale? j'ai trouvé ma réponse un peu par hasard, en étudiant la distribution des notes de chaque classe.


# Avertissement

L'analyse statistique portent sur 2 mois de cours et 3 classes de 30 élèves chacune. L'effectif est donc assez faible. Le lecteur ne doit rien conclure hâtivement car les résultats sur un si faible échantillon peuvent être simplement dus à des fluctuations statistiques.

# But de l'analyse

On va étudier la distribution des notes des élèves autour de la moyenne. Ce que l'on s'attend à trouver, c'est une [distribution gaussienne](https://fr.wikipedia.org/wiki/Fonction_gaussienne), la fameuse courbe en cloche que l'on voit sur la figure ci-dessous. On l'appelle encore "[loi normale](https://fr.wikipedia.org/wiki/Loi_normale)". [Par Nusha sur Wikipedia slovène — Transféré de sl.wikipedia à Commons., GFDL](https://commons.wikimedia.org/w/index.php?curid=8710900)

![Loi normale]({{ site.baseurl }}/images/lycee/loinormale.svg "Loi normale")



Beaucoup d'élèves se situent autour de la moyenne de la classe et plus les notes sont élevés plus le nombre d'élèves qui ont de telles notes diminue. De la même manière pour les notes basses. Il faut imaginer que sur la figure, le centre de la courbe (le zéro) est en fait la note moyenne. Le symbole sigma est l'écart type et permet de caractériser l'étalement de la distribution autour de la moyenne. Pour les classes de Bac Pro, on va d'estimer ce sigma et vérifier aussi que l'on a bien une gaussienne.

# Les Données

Pour les seconde Pro (2PRO), la moyenne de chaque élèves a été obtenue avec 2 interrogations coefficient 2 et un devoir à la maison coefficient 1. J'ai été plutôt indulgent sur les notes et la moyenne de la classe est à 11.39. Ci-dessous, on peut voir la distribution des notes autour de la moyenne. L'histogramme montre le nombre d'élèves pour chaque note de 0 à 20. Par exemple, 2 élèves ont une note comprise entre 7 et 8 (les moins bonnes notes).

![Distribution 2PRO]({{ site.baseurl }}/images/lycee/distribution2PRO.png "Distribution 2PRO")

La distribution n'est pas une belle courbe en cloche et c'est normal. Quand on a peu de résultats, on a de fortes fluctuations statistiques. Mais on a quand même, grossièrement, une forme avec beaucoup de personnes avec des notes autour de la moyenne de la classe, puis une diminution assez forte quand on s'en éloigne.

Pour les premières (1PRO), la moyenne a été calculée à partir de 2 interrogations. La moyenne est de 7.9. J'ai été plus sévère sur les notes mais je ne crois pas que ca ait eu un impact sur leur travail. Voici la distribution des notes.

![Distribution 1PRO]({{ site.baseurl }}/images/lycee/distribution1PRO.png "Distribution 1PRO")

On remarque tout de suite que la distribution est plus étalée. Si on fait l'hypothèse que c'est une gaussienne, l'écart type est plus grand ici. Néanmoins, la distribution ne ressemble pas vraiment à une cloche. Il y aurait plutôt 2 bosses de part et d'autre de la moyenne, un peu comme s'il y avait 2 gaussiennes cote à cote. ce pourrait être comme dans la figure ci-dessous (prise sur [wikipedia](https://fr.wikipedia.org/wiki/Mod%C3%A8le_de_m%C3%A9langes_gaussiens)):

![Distribution double gaussienne]({{ site.baseurl }}/images/lycee/Double_Gauss.png "Distribution double gaussienne")

La courbe bleu est la somme des fonctions gaussiennes rouge et verte.


Voyons maintenant ce que donne la distribution pour les terminales (TPRO). On a ici seulement une interrogation. 

![Distribution TPRO]({{ site.baseurl }}/images/lycee/distributionTPRO.png "Distribution TPRO")

Le phénomène observé en 1PRO est confirmé ici. Il semble y avoir 2 groupes dans la classe: le groupe des bons élèves et le groupe des élèves qui ont décroché. On va maintenant essayer de confirmer ou infimer cette hypothèse.

# Estimation des distributions

Il existe des algorithmes pour trouver la gaussienne la plus adaptée à la distribution de nos données. J'utilise ici le [modèle de mélanges gaussiens](https://fr.wikipedia.org/wiki/Mod%C3%A8le_de_m%C3%A9langes_gaussiens). On doit faire l'hypothèse que les données sont issues de plusieurs groupes indépendants, suivant chacun une loi normale avec une moyenne et un écart type different. Les informations à donner pour construire le modèle sont les notes et le nombre de groupes que l'on suppose avoir dans la classe. Le programme va automatiquement donner les moyennes et écart types pour chaque groupe. les valeurs trouvées sont celles qui maximisent une quantité qui s'appelle la [vraisemblance](https://fr.wikipedia.org/wiki/Maximum_de_vraisemblance).

Ce modèle a bien sur des limites. Il ne trouve pas forcement le maximum global de la vraisemblance et peut converger sur un maximum local. De plus le nombre de notes reste faible pour faire une étude statistiques et cela a une influence sur les résultats du modèle. Les algorithmes d'apprentissage automatiques ne font pas des miracles et restent tributaires de la qualité et la quantité des données.

En pratique, avec Python, on peut utiliser le module [scikit-learn](http://scikit-learn.org) qui contient un ensemble assez complet de programmes d'[apprentissage automatique](https://fr.wikipedia.org/wiki/Apprentissage_automatique) (machine learning). On y trouve une version du [modèle de mélanges gaussiens](http://scikit-learn.org/stable/modules/generated/sklearn.mixture.GaussianMixture.html#sklearn.mixture.GaussianMixture) (Gaussian Mixture Model en anglais).

On importe et on lance la fonction comme suit:

```
from sklearn.mixture import GaussianMixture
n_groups=1
g = GaussianMixture(n_groups)
g.fit(notes)
```
 où `n_groups` est le nombre de groupes que l'on suppose dans la classe et `notes` est la liste des notes sur une colonne. La commande `g.fit` va lancer l'algorithme et stocker les résultats dans differentes variables associées à `g`. Pour obtenir les résultats de chaque groupe, on peut utiliser la routine itérative suivante:

```
for group_id in range(n_groups):
    weight = g.weights_[group_id]
    mean = g.means_[group_id][0]
    sigma = np.sqrt(g.covariances_[group_id][0][0])
    print("GROUP",group_id)
    print("Weight",round(weight,2))
    print("Mean",round(mean,2))
    print("Sigma",round(sigma,2))
```
Ici, `weight` est le poids de chaque gaussienne, `mean` sa moyenne et `sigma` son écart type.

Voici ce que l'on obtient pour les notes des 2PRO si on suppose un seul groupe:

![Distribution 2PRO]({{ site.baseurl }}/images/lycee/distribution2PROfit.png "Distribution 2PRO")

La gaussienne centrée en 11.39 et d'écart type 2.45 semble bien coller avec les données.
Passons maintenant au 1PRO.
Supposons d'abord qu'il y a un seul groupe dans la classe. On obtient le résultat suivant.

![Distribution 1PRO 1 groupe]({{ site.baseurl }}/images/lycee/distribution1PROfit1.png "Distribution 1PRO 1 groupe")

Si l'on suppose qu'il existe 2 groupes.

![Distribution 1PRO 2 groupes]({{ site.baseurl }}/images/lycee/distribution1PROfit2.png "Distribution 1PRO 2 groupe")

Il est difficile de départager ces résultats en regardant les graphiques. On va essayer un autre critère, celui du score "BIC" pour [Bayesian Information Criterion](https://en.wikipedia.org/wiki/Bayesian_information_criterion) ou critère d'information Bayésien. La fonction GaussianMixture nous donne accès au BIC qu'elle a obtenue avec `g.bic(notes)`. Pour 1 groupe cette valeur est 163.96 et pour 2 groupes elle est de 166.87. Le modèle pour lequel la valeur BIC est la plus petite est le meilleur (le plus adapté à la distribution). On a donc ici un modèle à un groupe d'élèves qui décrit (légèrement) mieux notre distribution de notes.

Remarquons que l'écart type de la gaussienne pour les 2PRO vaut 2.45 et pour les 1PRO il vaut 3.32. On a donc une distribution des notes bien plus étalée pour les 1PRO, ce qu'on voit sur le graphique. Les différences entre bon élèves et élèves en difficultés sont plus accentués dans la classe de 1PRO.

Finissons par la classe de TPRO. J'ai écarté la note zéro d'une élève qui est sortie de l'interrogation sans me rendre sa copie. Je ne pense pas qu'elle soit représentative et elle fausse la distribution des notes, d'autant plus que les notes n'ont pas été moyennées sur plusieurs interrogations. Voici ce que l'on obtient avec un groupe.

![Distribution TPRO 1 groupe]({{ site.baseurl }}/images/lycee/distributionTPROfit1.png "Distribution TPRO 1 groupe")

Il est plus difficile de voir la correspondence entre la distribution des notes et le modèle. Si l'on suppose qu'il existe 2 groupes.

![Distribution TPRO 2 groupes]({{ site.baseurl }}/images/lycee/distributionTPROfit2.png "Distribution TPRO 2 groupe")

La représentation semble plus adaptée.
Si l'on compare le BIC des 2 résultats, on a 127.37 pour le modèle à un groupe et 124.07 pour le modèle à 2 groupe. Il semble donc que le modèle à 2 groupes soit plus fidèle pour décrire la distribution des notes de TPRO. Cela confirme la tendance de séparation entre les élèves de la classe avec le groupe des "bon" et le groupe des élèves en difficultés qui ont baissé les bras.

Pour montrer les limites de cette étude, voici maintenant la distribution des notes pour une classe de Seconde générale, sur une interrogation courte de 30 minutes.

![Distribution 2 générale]({{ site.baseurl }}/images/lycee/distribution2generale.png "Distribution 2 générale")

On voit ici aussi une séparation en 2 groupes, comme pour les TPRO. L'évolution en 2 groupes distincts en filière Bac Pro n'est finalement peut être pas une évolution mais juste le fait que la classe, pour cette interrogation, est divisée en 2. La manière dont les questions sont posées dans l'interrogation peut avoir eu une influence, dans le sens où soit on sait faire l'exercice et on a tous les points, soit on a aucun point. Ceci creuse l'écart entre les élèves qui ont compris et les autres. Il faudrait confirmer sur les prochaines interrogations.

# Conclusion

Il faut rester prudent dans les conclusions avec une aussi faible quantité de données. Les résultats pourraient être dû purement au hasard mais je dois noter qu'ils sont en accord avec l'impression que j'ai eu en classe. En effet, j'ai trouvé la classe très inhomogène avec certains élèves attentifs et intéressés, qui posent des questions et d'autre totalement démotivés. Comme je le disais au début, certains élèves sont convaincus qu'ils ne sont pas bon en math et que ca ne sert à rien pour eux d'essayer de travailler. 

Au delà de ces résultats, il reste donc le problème de gestion de la classe. Comment combiner de la meilleure manière ces différences de niveaux afin que les élèves en difficulté se motivent et suivent et que les bon élèves ne s'ennuient pas non plus (sinon, c'est eux qui vont commencer à bavarder et à gêner le cours). Je n'ai pour le moment pas de solution pour cela.

Même s'il est difficile de conclure, c'est tout de même un bon exercice d'apprentissage automatique. Un algorithme important du domaine (le modèle de mélanges gaussiens) est utilisé et on voit son application pratique, avec les conditions dans lesquelles on peut l'utiliser, comment l'appliquer et ses limites. On voit aussi un problème central dans l'analyse de données en intelligence artificielle : on a besoin d'un grand voire très grand nombre de données pour avoir un apprentissage de qualité, et pouvoir extraire de l'information pertinante de celles-ci.