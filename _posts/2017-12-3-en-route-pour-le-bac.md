---
layout: post
comments: true
title: En route pour le Bac
categories: [lycee, statistiques]
thumbnail: /images/lycee/
published: false
---


For my english readers, this post is in French. It is talking about student data from the French education system so I thought it would be better to write it in French.
L'analyse des notes au cours du temps en dit long sur le comportement des élèves. C'est ce que nous allons voir dans cet article de blog. J'avais à disposition les notes d'élèves de la filiaire Bac Pro d'un lycée, donc les classes de seconde, première et terminale Bac Pro. Cette filiaire est particuliaire dans le sens ou il y a en proportion plus d'élèves en difficulté avec le système scolaire standard, ce qui les poussent à choisir un débouché plus rapide dans la vie active et donc à s'orienter en Bac Pro. Je les ai eu en classe pendant 2 mois et en regardant les notes de leur contrôles je dois dire que j'étais un peu perplexe. En effet, la distrubution des notes autour de la moyenne est singulière. Elle semble évoluer d'une forme Gaussienne en seconde vers une séparation en 2 groupes (2 gaussiennes) en terminale. Comme j'adore l'analyse de données et que j'ai tous les outils à ma disposition avec Python, j'ai plongé dans l'analyse des notes. J'ai utilisé non seulement des statistiques que je leur ai enseigné (au programme du Bac Pro), mais c'était aussi l'occasion mes algorithmes favoris utilisés en apprentissage automatique et intelligence artificielle, dont le modèle de mélange gaussien. Et voilà ce que j'ai découvert... 



# Introduction

Comme mon entreprise a encore du mal à décoller, j'ai décidé de chercher des alternatives. Et récemment je suis tombé sur l'annonce d'un lycée qui cherchait un prof de math à mi-temps pour 2 mois. Ca me semblait parfait pour me changer un peu les idées et sortir du code informatique que je voyais toute la journée. 

C'est une drôle d'expérience pour moi. Je dois avouer que je n'étais pas préparé à ca. J'ai appris beaucoup de choses durant ces 2 mois au contact des élèves. J'ai pu remarquer une première chose, les élèves n'ont pas le niveau de maturité auquel je m'attendais. Je les ai pris pour des adultes, je leur ai donné trop de liberté, et certain en ont profité. A la fin, bien sur, cela se voit sur les notes. J'ai trouvé un niveau très hétérogène qui va des bons élèves sérieux et motivés jusqu'au élèves en grande difficultés et totalement fermés aux mathématiques. Ces derniers ont toujours été en échec en mathématiques et ont baissé les bras, comme ils me l'ont dit en classe. Je me suis demandé comment ces personnes là évolueraient à l'approche du Bac. Prendraient-elles conscience de l'enjeu et se mettraient-elles au travail en classe de première ou de terminale? j'ai trouvé ma réponse un peu par hasard, en étudiant la distribution des notes de chaque classe.

Et bien sur, je n'ai pas pu m'empecher de faire de l'analyse de données.

# Avertissement

L'analyse statistique portent sur 2 mois de cours et 3 classes de 30 élèves chacunes. L'effectif est donc assez faible. Le lecteur ne doit pas conclure hativement car les résultats sur un si faible echantillon peuvent être simplement dûs à des fluctuations statistiques.