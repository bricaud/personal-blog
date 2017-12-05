---
layout: post
comments: true
title: En route pour le Bac
categories: [lycee, statistiques]
thumbnail: /images/lycee/loinormale.svg
published: false
---


*For my english readers, this post is in French. It is talking about student data from the French education system so I thought it would be better to write it in French.*
L'analyse des notes au cours du temps en dit long sur le comportement des élèves. C'est ce que nous allons voir dans cet article de blog. J'avais à disposition les notes d'élèves de la filiaire Bac Pro d'un lycée, donc les classes de seconde, première et terminale Bac Pro. Cette filiaire est particuliaire dans le sens ou il y a en proportion plus d'élèves en difficulté avec le système scolaire standard, ce qui les poussent à choisir un débouché plus rapide dans la vie active et donc à s'orienter en Bac Pro. Je les ai eu en classe pendant 2 mois et en regardant les notes de leur contrôles je dois dire que j'étais un peu perplexe. En effet, la distribution des notes autour de la moyenne est singulière. Elle semble évoluer d'une forme Gaussienne en seconde vers une séparation en 2 groupes (2 gaussiennes) en terminale. Comme j'adore l'analyse de données et que j'ai tous les outils à ma disposition avec Python, j'ai plongé dans l'analyse des notes. J'ai utilisé non seulement des statistiques que je leur ai enseigné (au programme du Bac Pro), mais c'était aussi l'occasion d'utiliser mes algorithmes favoris d'apprentissage automatique et intelligence artificielle, ici le modèle de mélange gaussien. Les résultats et l'analyse en elle-meme sont interessants et c'est pour ca que je le partage. Voilà ce que j'ai découvert... 



# Introduction

Comme mon entreprise a encore du mal à décoller, j'ai décidé de chercher des alternatives. Et récemment je suis tombé sur l'annonce d'un lycée qui cherchait un prof de math à mi-temps pour 2 mois. Ca me semblait parfait pour me changer un peu les idées et sortir du code informatique que je voyais toute la journée. 

C'est une drôle d'expérience pour moi. Je dois avouer que je n'étais pas préparé à ca. J'ai appris beaucoup de choses durant ces 2 mois au contact des élèves. J'ai pu remarquer une première chose, les élèves n'ont pas le niveau de maturité auquel je m'attendais. Je les ai pris pour des adultes, je leur ai donné trop de liberté, et certain en ont profité. A la fin, bien sur, cela se voit sur les notes. J'ai trouvé un niveau très hétérogène qui va des bons élèves sérieux et motivés jusqu'aux élèves en grande difficultés et totalement fermés aux mathématiques. Ces derniers ont toujours été en échec en mathématiques et ont baissé les bras, comme ils me l'ont dit en classe. Je me suis demandé comment ces personnes là évolueraient à l'approche du Bac. Prendraient-elles conscience de l'enjeu et se mettraient-elles au travail en classe de première ou de terminale? j'ai trouvé ma réponse un peu par hasard, en étudiant la distribution des notes de chaque classe.


# Avertissement

L'analyse statistique portent sur 2 mois de cours et 3 classes de 30 élèves chacunes. L'effectif est donc assez faible. Le lecteur ne doit rien conclure hativement car les résultats sur un si faible échantillon peuvent être simplement dûs à des fluctuations statistiques.

# But de l'analyse

On va étudier la distribution des notes des élèves autour de la moyenne. Ce que l'on s'attend à trouver, c'est une distribution gaussienne, la fameuse courbe en cloche que l'on voit sur la figure ci-dessous. On l'appelle encore "[loi normale](https://fr.wikipedia.org/wiki/Loi_normale)". [Par Nusha sur Wikipedia slovène — Transféré de sl.wikipedia à Commons., GFDL](https://commons.wikimedia.org/w/index.php?curid=8710900)

![Loi normale]({{ site.baseurl }}/images/lycee/loinormale.svg "Loi normale")



Beaucoup d'élèves se situent autour de la moyenne de la classe et plus les notes sont élevés plus le nombre d'élèves qui ont de telles notes diminue. De la même manière pour les notes basses. Il faut imaginer que sur la figure, le centre de la courbe (le zéro) est en fait la note moyenne. Le symbole sigma est l'écart type et permet de caractériser l'étalement de la distribution autour de la moyenne. Pour les classes de Bac Pro, on va d'estimer ce sigma et vérifier aussi qe l'on a bien une gaussienne.

# Les Données

Pour les seconde Pro (2PRO), la moyenne de chaque élèves a été obtenue avec 2 interrogations coefficient 2 et un devoir à la maison coefficient 1. J'ai été plutôt indulgent sur les notes et la moyenne de la classe est à 11.39. Ci-dessous, on peut voir la distribution des notes autour de la moyenne. L'histogramme montre le nombre d'élèves pour chaque note de 0 à 20. Par exemple, 2 élèves ont une note comprise entre 7 et 8 (les moins bonnes notes).

![Distribution 2PRO]({{ site.baseurl }}/images/lycee/distribution2PRO.png "Distribution 2PRO")

La distribution n'est pas une belle courbe en cloche et c'est normal. Quand on a peu de résultats, on a de fortes fluctuations statistiques. Mais on a quand même, grossièrement, une forme avec beaucoup de personnes avec des notes autour de la moyenne de la classe, puis une diminution assez forte quand on s'en éloigne.

Pour les premières (1PRO), la moyenne a été calculée à partir de 2 interrogations. La moyenne est de 7.9. J'ai été plus sévère sur les notes mais je ne crois pas que ca ait eu un impact sur leur travail. Voici la distribution des notes.

![Distribution 1PRO]({{ site.baseurl }}/images/lycee/distribution1PRO.png "Distribution 1PRO")

On remarque tout de suite que la distribution est plus étalée. Si on fait l'hypothèse que c'est une gaussienne, l'écart type est plus grand ici. Néanmoins, la distribution ne ressemble pas vraiment à une cloche. Il y aurait plutôt 2 bosses de part et d'autre de la moyenne, un peu comme s'il y avait 2 gaussiennes cote à cote.

Voyons maintenant ce que donne la distribution pour les terminales (TPRO). On a ici seulement une interrrogation. 

![Distribution TPRO]({{ site.baseurl }}/images/lycee/distributionTPRO.png "Distribution TPRO")

Le phénomène observé en 1PRO est confirmé ici. Il semble y avoir 2 groupes dans la classe: le groupe des bons élèves et le groupes des élèves qui ont décroché.

# Estimation des distributions

Il existe des algorithmes pour trouver la gaussienne la plus adaptée à la distribution de valeur de nos données. J'utilise ici le [modèle de mélanges gaussiens](https://fr.wikipedia.org/wiki/Mod%C3%A8le_de_m%C3%A9langes_gaussiens).

![Distribution 2PRO]({{ site.baseurl }}/images/lycee/distribution2PROfit.png "Distribution 2PRO")

