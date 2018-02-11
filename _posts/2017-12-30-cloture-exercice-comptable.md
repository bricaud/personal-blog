---
layout: post
comments: true
title: Les démarches pour la cloture de l'exercice comptable
categories: [entreprise, comptabilité, francais]
thumbnail: /images/exerciceComptable/imagecerfa.png
published: true
---

Chaque année, les entreprises doivent faire un bilan et communiquer des informations clefs aux diverses administrations. Pour la première année d'exercice d'Evia Cybernetics, j'ai décidé de faire cela moins même. Le but était à la fois de réduire les dépenses et d'apprendre un peu les bases de la comptabilité et de l'administration d'une entreprise. J'ai appris beaucoup de choses. Je relate ici mon expérience sur les différentes étapes pour clore un exercice comptable.


# Quel bilan?

Evia Cybernetics est un SAS. Certes, c'est une petite SAS, avec un chiffre d'affaire de 8000 euros cette année, mais c'est bien une SAS assujettie à l'impôt sur les sociétés. Une entreprise de ce type doit chaque année faire un bilan, on dit aussi clore l'exercice comptable. La date de ce bilan est spécifiée dans les statuts de la société. C'est l'occasion de faire le point sur l'entreprise avec le calcul des bénéfices de l'année mais aussi de résumer son activité avec une longue liste de chiffres. 

Ce bilan sert au calcul de l'impôt et les chiffres clefs sont aussi là pour permettre l'évaluation de la santé de l'entreprise par les dirigeants et actionnaires mais aussi par un plus large public (à travers le dépôt au greffe).

Habituellement, ce travail de bilan est fait par un expert comptable mandaté par l'entreprise, ou par le comptable de l'entreprise si elle a les moyens d'en avoir un. On peut aussi faire sa comptabilité soi-même, même si cela est assez complexe. J'ai tenté l'expérience et je dois avouer que la tâche n'est pas si simple. J'ai été aidé par le logiciel open source [Openconcerto](https://www.openconcerto.org/). Ce logiciel est assez basique et on trouve très peu de documentation en ligne. On est donc assez vite limité mais pour une petite entreprise il est suffisant.

Pour cette année, ma comptabilité était facilitée. En effet, j'ai eu peu de clients, peu de factures et aucun salarié. La comptabilité était donc réduite au strict minimum.

J'ai séparé en 3 parties les démarches à faire pour cloturer son exercice. Elles sont liées mais font intervenir différentes entités et personnes.

# Les impots

![formulaire impots]({{ site.baseurl }}/images/exerciceComptable/imagecerfa.png "formulaire impots")

Une des principales raisons du bilan et pour le calcul de l'impôt. Pour ma SAS, je dois remplir plusieurs fiches cerfa, regroupées dans ce qui est appelé "[la liasse fiscale](https://www.impots.gouv.fr/portail/formulaire/2033-sd/bilan-simplifie)". Ce sont les formulaires 2033-A à 2033-G (une page chacun). Il y a aussi le formulaire 2065-SD qui résume les données importantes pour le calcul de l'impôt. La déclaration doit être faite dans les 3 mois après la cloture de l'exercice.

Le logiciel Openconcerto, a rempli automatiquement le bilan (2033-A) et le compte de résultats (2033-B). J'ai dû compléter le compte de résultats car j'ai le statut [Jeune Entreprise Innovante](https://www.legifrance.gouv.fr/affichCodeArticle.do;jsessionid=5FC3EE6C1A5F22E8450F0F89536A1068.tplgfr29s_2?idArticle=LEGIARTI000031011840&cidTexte=LEGITEXT000006069577&categorieLien=id&dateTexte=) qui permet une exonération d'impôts la première année.
Le formulaire 3 (2033-C) concerne les immobilisations et leur amortissement. J'ai dû compléter cette feuille car j'ai acheté du matériel informatique (qui s'amortit sur 3 ans), et OpenConcerto ne calcule pas les amortissements. J'ai coché "néant" pour les feuilles 2033-D et 2033-G. J'ai rempli le formulaire 2033-E (feuille 5) même si je n'ai pas à payer la CVAE avec mon petit chiffre d'affaire. J'ai simplement reporté les chiffres du feuillet 2 (2033-B). Avec cela il faut rajouter les formulaires 2059-H, 2059-I et 2069-RCI. Les 2 premiers concernent les filiales et s'addressent aux grandes entreprises, j'ai coché "néant" pour les 2. Quant au 2069-RCI, qui concerne certains crédits et réductions d'impots, j'ai choisi "néant" car la réduction d'impots "Jeune Entreprise Innovante" ne rentre pas dans le cadre de ce formulaire (c'est déjà déclaré dans les formulaires 2065 et 2033-B).

Comme il faut télédéclarer toute ces informations, il faut remplir les formulaires en ligne sur [le site des impots](https://www.impots.gouv.fr/portail/). J'ai eu un peu de mal car le lien vers les formulaires de déclaration n'apparaissaient pas. Après quelques coups de téléphone aux impots, clicks un peu partout et quelques jours d'attente, le lien est apparu mais sans vraiment savoir trop pourquoi... Le service en ligne n'est pas encore tout à fait au point... D'ailleurs j'avais accès à des formulaires de déclaration d'acomptes des impots qui n'avait pas lieu d'être car c'était ma première année d'exercice. Je les ai rempli avec des zéros et c'est peut être ca qui a débloqué ma déclaration de résultat par la suite.

# l'Assemblée générale


Comme je suis l'associé unique, le PV d'assemblée générale se réduit à un PV de décision de l'associé unique. Il est inspiré de [ce modèle](http://www.sas-sasu.info/sas-modele-de-pv-de-lassemblee-annuelle-pour-lapprobation-des-comptes/). Dans le PV, j'approuve les comptes (ceci est facultatif puisque l'unique associé fait les comptes et donc est supposé être d'accord avec lui-même!) et je statue sur [l'affectation des bénéfices](https://www.l-expert-comptable.com/a/37382-l-affectation-du-resultat-benefices-en-dividendes-ou-reserve.html). Cette année, je garde les bénéfices dans l'entreprise: je remplie la [réserve légale](https://www.lecoindesentrepreneurs.fr/reserve-legale-definition-dotation-fonctionnement/) (10% du capital social) et je fais un report à nouveau du reste. Je ne me verse donc aucun dividende. 

Pour comptabiliser l'affectation des résultats dans un logiciel de comptabilité, [voici comment procéder](https://www.compta-facile.com/comptabilisation-de-l-affectation-du-resultat/).

L'assemblée générale doit être faite au plus tard 6 mois après la cloture de l'exercice.

# Le greffe du tribunal de commerce

![Image greffe]({{ site.baseurl }}/images/exerciceComptable/imagegreffe.jpeg "Image greffe")

La liasse fiscale remplie pour la déclaration des résultats aux impôts [peut être utilisée](https://www.infogreffe.fr/informations-et-dossiers-entreprises/dossiers-thematiques/vie-de-entreprise/depot-des-comptes-sociaux.html?onglet=2) pour transmettre les informations demandées par le greffe. Il faut le bilan (formulaire 2033-A) et le compte de résultats (2033-B) contenus dans la liasse. A ceci il faut rajouter "l'annexe" [sauf si l'entreprise est très petite](https://www.compta-facile.com/simplifications-comptables-pour-les-micros-et-petites-entreprises/) (micro-entreprise) et c'est le cas d'Evia. Dernier document, le PV de décision sur la proposition et résolution d'affectation votée (PV de l'assemblée générale). La liste des documents est [ici](https://www.infogreffe.fr/documents/10179/0/liste_pieces_depot_comptes.pdf/a76662fc-dce6-4718-b79b-c774d7f8e6e9). Il ne faut pas oublier non plus que le dépot et payant. Comptez une cinquantaine d'euros.

Les documents doivent être déposés au plus tard un mois après l'AG qui statue sur les comptes. On peut déposer ses documents en ligne sur le site [infogreffe](www.infogreffe.fr), comme je l'ai fait.

# Les autres déclarations de décembre

Il ne faut pas oublier de déclarer sa TVA en décembre avec les formulaires 3517SCA12, 3514 pour les acomptes et 3517DDR pour les remboursements.
Il faut aussi payer le CFE (cotisation foncière des entreprises) avant mi-décembre.
On retrouve les 2 sur le site des impots.