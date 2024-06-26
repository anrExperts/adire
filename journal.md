# Journal du projet À dire d’experts

## 23 avril 2024
- Révision de la segmentation
- Z1J434d061 par Villain : il y a de ajouts en marge qui correspondent à des commentaires des descriptions faites dans le texte => ajout dans l'ontologie de MarginTextZone:commentary
- Dans ce même pv : ajout de mots/corections qui précèdent une phrase, ex : " de panneaux de (fin de ligne) de verre (en marge) cassés (retour à la ligne)". Pour l'instant "de verre" est mis dans la MainZone et non dans une MarginZone. 

## 12 avril 2024
- Révision de la segmentation
- Notes :
	- Chez Quirot il semble que les vacations soient numérotées en marge en chiffres arabes.
	- Z1J527d062 : mention "les parties se sont accomodées" => mentions administratives
  
## 8 avril 2024
- Évaluation d'un _text prompt_ pour Segment Anything avec la _library_ [lang segment anything](https://github.com/luca-medeiros/lang-segment-anything), afin de décrire ce que l'on recherche. La méthode utilise vraissemblablement le modèle de langue Bert (à vérifier !). La zones occupée par le cahier ou la feuille est parfaitement détectée, mais pas les feuilles individuellement. Cette méthode pourrait être utilisée pour les annexes ?
- le notebook est accessible sur le repo `segmentation`.

## 2-5 avril 2024
- poursuite de la segmentation (découpe) des images
- création d'une feuille XSLT pour le passage TEI => texte brut pour le versement dans eScriptorium. Un pseudo-balisage est utilisé pour l'annotation (ajouts, manques, unclear, etc.)
- les images des premières transcriptions TEI sont versées sur eScriptorium et les lignes sont segmentées. 5 transcriptions sont presentes aussi.
- discussion autour de l'ontologie et des méthodes de segmentation, notamment pour les ajouts en marge ([discussion ontologie](https://github.com/orgs/anrExperts/discussions/34)/[discussion segmentation](https://github.com/orgs/anrExperts/discussions/35)).

## 18-22 mars 2024
- amélioration du script Julia pour filtrer les segments, sauvegarde des images et de tous les segments pour corrections ultérieures le cas échéant
- les segments sont déposés sur sharedocs
- poursuite de la segmentation du corpus des 4 greffiers
- RC a transcrit une première expertise sur eScriptorium
- poursuite du travail sur l'ontologie
	- ajout de nouvelles régions
	- ajout des annotations pour le texte

## 4-8 mars 2024
- réalisation d'une XSLT pour le passage des transcriptions TEI -> eScriptorium
- premiers versements et segmentation des images (_lines_ & _masks_ uniquement)
	- dans l'ensemble le modèle par défaut (_blla_) identifie bien les lignes
	- il y a tout de même beaucoup de petites corrections à apporter lorsque du texte se trouve en marge
	- l'interface n'est pas toujours très stable (rechargements nécessaires) ni très intuitive (ordre des lignes notamment)
- rédaction d'une première ontologie à partir de [SegmOnto](https://segmonto.github.io)

## 19-23 février 2024
- Les segments retournés par _Image Segmentation_ sont trop nombreux et difficile à trier.
- le predictor automatique de SA est fonctionnel avec des images de plus faible définition. En effectuant la segmentation sur des images de 600X400 (6000X4000 pour les fichiers natifs), puis en extrapolant les coordonnées on obtient de très bon résultats.
- nous utilisons [PyCall.jl](https://github.com/JuliaPy/PyCall.jl) pour exécuter SA avec Julia. NB un _package_ julia existe mais nous rencontrons des problèmes pour l'installer.
- les premiers résultats sont plutôt bons. SA retourne beaucoup moins de segments et il est plus facile de les filtrer. D'autre part, on récupère directement les coordonnées des boîtes.
- methode :
	- les images au format paysage sont segmentées
		- les segments sont ensuite classés par taille 
		- les segments jugés trop petits ou trop grands sont éliminés (surface = 1/3 < segment < 1/2)
		- les segments sont ensuites ordonnées selon les coordonnées pour obtenir l'ordre gauche - droite
		- si aucun segment ne correspond à ces critères, l'image d'origine est retournée afin de ne pas avoir de blanc
		- cette méthode offre dans l'ensemble de bon résultats
	- les images au format portrait ne sont pas segmentées
	- un problème persistant concerne les annexes, souvent trop petites, il est très difficile de filtrer les segments correspondants (cf. point au dessus sur la taille des segments)

## 5-9 février 2024
- [JuliaImages](https://juliaimages.org) propose des solutions de segmentation par des méthodes traditionnelles (non fondées sur l'IA).
- [ImageSegmentation](https://juliaimages.org/latest/pkgs/segmentation/). Les solutions proposées par ce _package_ ne nécessitent pas une grosse puissance de calcule ni d'environnement spécifique type CUDA.
- la récupération des coordonnées des boîtes est un peu complexe. Le résultat d'une segmentation est une _matrix_, mais en travaillant sur les _extrema_ il est possible de les récupérer. 
- malgré les possibilites de personnalisation des algorithmes, la segmentation retourne beaucoup de segments (plusieurs centaines à plusieurs milliers).
- une autre piste pourrait reposer sur l'utilisation d'algorithmes de reconnaissance d'angles [imageCorners](https://juliaimages.org/ImageCorners.jl/dev/)

- En revanche Julia Images est pertinent pour la découpe des images, depuis IIIF ou en local, voir [Banaka-split](https://github.com/sardinecan/banaka-split)

## 26 janvier 2024
- travail exploratoire pour trouver d'autres méthodes de segmentation

## 23 janvier 2024
- tentative de résolution de l'erreur `cuda out of memory` (voir [errors.md](https://github.com/anrExperts/segmentation/blob/main/doc/errors.md#erreurs-python))… Toujours non résolue !
- possibilité de lancer la prédiction sur le CPU, c'est fonctionnel, mais très, très long !

## 22 janvier 2024
- creation d'un repo `segmentation` [https://github.com/anrExperts/segmentation](https://github.com/anrExperts/segmentation) 
- travail sur le dépot des sources prosopographiques sur Nakala

## 12 janvier 2024
- Si on réduit la taille de l'image le predictor automatique est fonctionnel.
- évaluation de l'influence des 3 modèles H, L, B > sans surprise le modèle H trouve plus de mask que le L, qui en trouve plus que le B. Mais pour la détection des pages le B est largement suffisant.
- modification des parametres de reconnaissance pour essayer d'affiner les prédictions, mais je ne comprends pas très bien sur quoi joue ces paramètres (à part ajouter des erreurs...)

## 11 janvier 2024
- mise en place d'un kernel pour les jupyter notebooks à partir du virtual env.
- test des notebooks fournis par Segment-Anything.
	- le predictor est fonctionnel…
	- …mais l'automatique rencontre toujours des erreurs (`CUDA OUT OF MEMORY`), malgré des essais avec 4 GPU et 44GB de RAM (config. max dispo). Vraisemblablement c'est la mémoire des GPU de pose problème (chaque GPU disposant "uniquement" de 12GB).

## 10 janvier 2024
- le virtual env est opérationnel avec `python 3.8.6` (toujours des erreurs avec les versions postérieures)
- rédaction d'un guide pour répertorier ce qui fonctionne et les erreurs rencontrées

## 9 janvier 2024
- Personnalisation des shells (pour y voir quelque chose…)
- mise en place d'un virtual env. Python pour Segment Anything… quelques erreurs persistent

## 8 janvier 2024
- lecture de la documentation de l'IN2P3 et premières explorations de la ferme de calcul

## 22 décembre 2023
- **ANR** révision des scripts pour l'importation des réceptions et des IAD. Le rapatriement est quasi prêt, il reste à voir quelques éléments avec RC cf [issue#35](https://github.com/anrExperts/data/issues/35).
- État des transcriptions : 17 expertises transcrites et encodées en TEI (soit 42f.) : 432d001 ; 433d068, d110 ; 434d067, d083, d122 ; 480d058 ; 481d050 ; 482d049 ; 483d033 ; 484d022, d035 ; 526d059 ; 527d062 ; 528d024 ; 663d036 ; 664d017.
- notre demande d'accès à la ferme de calcul IN2P3 a été acceptée.

## 21 décembre 2023
- Lectures sur Yolo et nouvelle tentative d'installation (le précédent notebook portait en réalité sur une application vidéo)
- Présentation des premiers résultats de segmentation avec SA à EC et nouvelle tentative d'utiliser le mode automatique (installation des drivers CUDA notamment), mais après lecture de la documentation fournie par Huma-num, la carte graphique de la ferme de calcul niveau 1 est bridée, ce qui pourrait expliquer les erreurs que nous avons :
> Les GPU Nvidia A100 sont partitionnés via MIG, par conséquenc il n’est pas possible d’utiliser les bibliothèques graphique (Vulkan, DirectX, OpenGL). Chaque compte ne peut accéder qu’à une fraction de la GPU.
- On s'orienterait donc vers l'utilisation de SA, mais il faut attendre la réponse d'Huma-num pour l'accès à l'IN2P3.

## 19 décembre 2023
- Test du mode automatique de SA
  - malheureusement, ce mode est trop gourmand en ressources pour s'exécuter avec la version gratuite de Google Colab, peu importe le modèle de données utilisé (`B`, `L` ou `H`).
  - la ferme de calcul niveau 1 d'Huma-num dispose bien d'une carte graphique NVidia A100 compatible `CUDA` (nécessaire pour SegmentAnything). Mais impossible d'exécuter le script…

## 18 décembre 2023
- Poursuite du travail sur SegmentAnything. 
  - SA propose deux modes de fonctionnement :
    - Predictor : l'utilisateur détermine lui même ce qu'il recherche (ou ce qu'il ne veut pas) par l'intermédiaire de pointeurs ou de box
    - Automatic mask générator : SA détect automatiquement les objets sur une image.
  - 3 modèles (SAM) différents peuvent être chargés `vit_B`, `vit_L`, `vit_H`. Les modèles sont vraisemblablement les mêmes sauf ce qui concerne la taille du réseau de neuronne. `B` = "base" ; `L` = "large" ; `H` = "huge". `H` est plus performant que `B`, mais `L` offre tout de même de bons résultats tout en étant plus léger… Le choix dépend donc plus de la puissance de calcul disponible.
    - En revanche, il semble difficile d'effectuer du *fine tuning* avec SAM. Les modèles ayant été entrainés avec une grande quantité de données, il en faudrait d'autant plus pour le fine tuné…
- Les premiers résultats avec le predictor sont encourageants, avec l'utilisation de `vit_H` SA a retrouvé à chaque fois les feuillets demandés, peu importe la méthode employée :
  - point unique <img src="files/img/predictorPoint.png" width="150px" />
  - points multiples  <img src="files/img/predictorPoints.png" width="150px" />
  - points multiples avec exclusion  <img src="files/img/predictorPointsExclude.png" width="150px" />
  - boite  <img src="files/img/predictorBox.png" width="150px" />
  - boite et point d'exclusion <img src="files/img/predictorBoxAndPoint.png" width="150px" />
  - boites multiples  <img src="files/img/predictorMultipleBoxes.png" width="150px" />
- Le notebook annoté est sur Google Drive : `EXPERTS/Colab Notebooks/segmentAnything_predictor_example.ipynb`

## 6 décembre 2023
- test de [segment-anything](https://github.com/facebookresearch/segment-anything) et [Yolo](https://github.com/RizwanMunawar/yolov7-segmentation). Les deux reposent sur CUDA (nécessitent donc des cartes Nvidia).
  - Segment Anything : 
    - installation sur macbook : echec
    - installation sur le serveur de calcul interactif mutualisé d'Huma Num (qui dispose d'un GPU Nvidia) : échec (au niveau de CUDA, problème de drivers je pense…)
    - fonctionne sur Google Collab
  - Yolo
    - installation sur le serveur de calcul interactif mutualisé d'Huma Num : échec. Dans un premier temps, problème causé par la version des packages `torch`, `torchevision`, downgrade vers `Torch 2.0.1` et `Torchevision 0.8.1` règle une partie du problème pas tout ([cf issue sur pytorche](https://github.com/pytorch/pytorch/issues/111469)).

## 5 décembre 2023
- **ANR** ajout d'un submodule dans le repo Data pour le schema EAC et ajout des `<descriptiveNotes/>` dans les `<chronItem/>`
- **ANR** reprise des fichiers EAC-CPF pour la validation [issue #45](https://github.com/anrExperts/data/issues/45)
- notre demande d'accès à CREMMA a été acceptée, nous avons accès à une instance eScriptorium.

## 4 décembre 2023
- **ANR** Ajout de la documentation sur le formulaire dans le schéma Z1J
- **ANR** validation des fichiers EAC-CPF

## 24 novembre 2023
- **ANR** finalisation du schema Z1J

## 22 novembre 2023
- demande d'accès au serveur de calcul IN2P3
- **ANR** poursuite de la rédaction du schéma Z1J

## 20 novembre 2023
- **ANR** reprise du schéma Z1J en pure ODD (suppression des règles relaxNG) et début du travail sur les règles schematron

## 31 octobre 2023
- Poursuite des transcriptions
- Z1J 434_0589 : deux mains différentes, Villain qui débute puis certainement Quirot, puis de nouveau Villain. La main fatigue au fil du pv. Deux abréviations rencontrées qui ne sont pas faciles à transcrire (un commentaire a été laissé à chaque fois).
- corrections fiches prosopo et formulaire (vocabulaire Ric-O)
- début rédaction odd xpr

## 30 octobre 2023
- Poursuite des transcriptions
- Première proposition de [normes de transcriptions](https://github.com/anrExperts/adire/issues/6#issuecomment-1785794335)
- Kraken : semble fonctionner, en partie, avec python 3.10 sur macOs, mais uniquement pour la segmentation, toujours impossible de charger un modèle de transcription ou de segmentation.

## 20 octobre 2023
- Kraken : tentative d'installation sur macOs avec pip et conda (très long avec ce dernier) : échec. Ne semble pas fonctionner avec python 3.12

## 19 octobre 2023
- Travail sur les particularités des mains (abréviations, ponctuation, accentuation, etc.)
- Tableau comparatif des règles de transcription (Arianne Pinche, Maxime Gohier, Bernard Barbiche)
- Dépôt de la candidature [Cremma](https://cremmacall.sciencescall.org/)
- Installation de [Kraken](https://kraken.re/main/index.html)
  - Difficultés rencontrées pour l'installation de Kraken, puis pour l'installation d'un modèle de reconnaissance de texte (problèmes non résolus pour l'heure) (Ubuntu)
  - La segmentation fonctionne (modèle std BLLA) et ne semble effectivement pas très gourmande en ressource
    - sorties testées json et alto
    - outil de visualisation pour segmentation alto [https://github.com/emory-lits-labs/altoviz](https://github.com/emory-lits-labs/altoviz) - [http://emory-lits-labs.github.io/altoviz/](http://emory-lits-labs.github.io/altoviz/)

## 17 octobre 2023
- Rédaction du dossier pour accès CREMMA et instance eScriptorium

- Lecture du compte-rendu du rdv avec Alix et du Guide de la transcription d'Ariane Pinche :
  - segmentation des mots : "pardevant" (Villain) = "par devant"
  - retrait des majuscules : "parLaquelle" (Quirot) = "par laquelle"
  - les abréviations ne doivent pas être développées : "observaõn" (Quirot) 

- Recherche d'autres règles de transcription utilisées par des projets HDR et des corpus de textes modernes pour obtenir un panorama :
  - Le projet "Crimes et châtiments" a par exemple fait le choix de développer les abréviations puis de les mettre en notes dans Transkribus pour obtenir un corpus différent (cf. Élodie Paupe. ENC. (2022, 23 juin). 1.4 : Une cursive du 17e siècle , in Documents anciens et reconnaissance automatique des écritures manuscrites. [Vidéo]. Canal-U. [https://www.canal-u.tv/133333].)
  - Les Gardenotes qui propose des règles de transcription de corpus de textes période moderne [https://lesgardenotes.org/kb/regles-de-transcription/]
  - Theleme (ENC) et les Conseils pour l’édition des textes de l’époque moderne (XVIe-XVIIIe siècle) par Bernard Barbiche [http://theleme.enc.sorbonne.fr/cours/edition_epoque_moderne/edition_des_textes]

- Début de rédaction d'un tableau comparatif des règles de transcription entre Ariane Pinche, Maxime Gohier et Bernard Barbiche cf [issue 6](https://github.com/anrExperts/adire/issues/6)

- Tableaux récapitulatifs sur la sélection des greffiers ([données et commentaires](https://docs.google.com/spreadsheets/d/e/2PACX-1vQ6vEqexLuojIOKkZC8B8dhmzbV0MF6-dGOcczVKZMEZb3TPJODYsGeDU4AG33kzAP4stp1-z6a8MF0/pub?gid=313988877#))

Présélection des greffiers disposant d'un corpus d'expertises important et susceptible de couvrir une longue période :
![Tableau greffier](files/img/greffiersTableau.png)

Frise chronologique greffiers / nombre d'expertises (écriture AA et +)
![timeline greffiers expertises](files/img/greffiersExpertises.png)

Frise chronologique greffiers / qualité d'écriture (écriture AA et +)
![timeline greffiers expertises](files/img/greffiersEcriture.png)

Frise chronologique greffiers / nombre d'expertises 2 (écriture AAA et +)
![timeline greffiers expertises](files/img/greffiersExpertises_2.png)

Frise chronologique greffiers / qualité d'écriture 2 (écriture AAA et +)
![timeline greffiers expertises](files/img/greffiersEcriture_2.png)

## Réunion IERDJ, 16 octobre 2023 : Josselin, Sara, Emmanuel, Robert

### Discussion sur l’évaluation statistique

- Les recherches sur la sélection des mains ont été faites avec Jupiter. Les médianes, etc. on été calculées avec XQuery.
- Pour le NLP on va sans doute travailler avec Julia avec des appels de fonction en Python au besoin.

Environnement de travail

Pour la reconnaissance d’écriture ou la segmentation, on aura probablement besoin de faire tourner des modèles de deep learning. Il sera sans doute nécessaire d’avoir accès des GPU pour entraîner des modèles. Voir avec HumaNum ou Calcul Québec.
communication à Alix des informations sur les écritures


## Organisation du travail

Drive ou blog. Créer un lab dans le cadre du projet
Carnet de laboratoire, chaque jour prendre 10 minutes
Blogs plus ponctuels : par exemple sur la reconnaissance des écritures.

## Segmentation

Discussion des solutions possibles pour la segmentation : cf. [issue 4](https://github.com/anrExperts/adire/issues/4)

## Rdv Alix Chagué

### Collaboration avec CREMMA

Comme le projet est mené en France, il est éligible pour disposer d’un compte sur Cremma.
https://cremmacall.sciencescall.org/

--> [issue 5](https://github.com/anrExperts/adire/issues/5)

Pour formaliser une collaboration avec Almanach au-delà de la simple participation à CREMMA, il est possible d’écrire à Benoît Sagot et à Laurent Romary.

eScriptorium fonctionne actuellement encore sur le serveur Traces6. Il devrait bientôt basculer vers le nouveau serveur CREMMA.

Il peut y avoir des limites concernant l’utilisation du serveur. Notre projet présente le dimensionnement suivant :
- 60 000 vues au total
- Si choix d’un nombre limité de greffier : 1 173 expertises (circa 6 000 photos)
- limite de stockage prévue sur CREMMA c. 20 000

Appliquer un modèle de reconnaissance n’est pas coûteux en GPU, c’est l’entraînement du modèle qui est coûteux.

L’entraînement ne fonctionnent pas actuellement sur Traces6. Cela devrait fonctionner sur CREMMA d’ici la fin de l’année. En attendant, il est toujours possible d’utiliser eScriptorum pour faire l’annotation et de passer en local pour entraîner un modèle avec Kraken.

### Discussion sur la chaîne de segmentation

Alix nous recommande la pré-annotation et la détection des zones dans Kraken. Il est toutefois possible de segmenter au préalable pour travailler sur des pages uniques.

Le logiciel [ScanTaylor](https://scantailor.org) peut s’avérer utile pour cette tâche. Il présente l’inconvénient de générer des images très lourdes, mais l’assistant de détection des images fonctionne bien. La sortie est en 600dpi binarisé alors que l’on n’a pas nécessairement besoin de cette résolution ou d’une binarisation. Il est possible d’obtenir une sortie couleur. C’st une solution possible mais longue.

Il pourrait égaleemnt Possible entraîner un modèle YOLO pour faire des rectangles (dépend rectilignité). 20 pages facile à entraîner. Fait des rectangles.

Si la segmentation est gratuite, la tâche pourraît également être réalisée dans Transkribus.

Regarder si on peut utiliser SegmentAnything ?

Alix avait fait des essais avec DhSegment en 2019. À l’époque, l’outil n’était pas très bien documenté et elle n’avait pas toutes les connaissances nécessaires pour y travailler.

Voir également les outils développés dans le cadre du projet OCR4All https://github.com/OCR4all/LAREX ou encore « Eynollah ». (2020) 2023. Python. QURATOR-SPK. https://github.com/qurator-spk/eynollah.

Même si c’est un investissement en temps important, il peut être intéressant de passer suffisamment de temps préalable sur la préparation de la segmentation. La segmentation préalable du corpus pourrait également présenter l’intérêt de repérer les éléments graphiques ou d’autres phénomènes.

Dans eScriptorium le modèle de segmentation BLLA (le modèle par défaut) fonctionne bien. Malheureusement, les métriques pour la performance des modèles sont unitaires. Elles permettent pas de distinguer les lignes, les régions et le typage des régions. Par ailleurs, le calcul a lieu au niveau des pixels alors que cette précision n’est pas nécessaire. Mieux vaut tester et voir comment cela fonctionne.

Toutefois, il est possible de segmenter avec Blla et de typer les zones. Il faut alors identifier un corpus de segmentation qui présente suffisamment de diversité et de complexité par rapport au corpus que l’on souhaite segmenter. Comme c’est coûteux de faire de la segmentation, il est tentant de vouloir l’utiliser pour la transcription mais il peut s’avérer nécessaire d’en faire une tâche spécifique.

### Repérage des mains et règles de transcription

Voir le Guide de transcription mis au point par Arianne Pinche
https://hal.science/hal-03697382/document

Un premier test réalisé avec le modèle HTR-Unites-Manu McFrench V3 donne des résultats encourageants. Cela devrait bien fonctionner en transcrivant relativement peu de pages.

Présentation à Alix du travail fait sur  le repérage des écritures. Alix signale une thèse sur la caractérisation des écritures. https://helios2.mi.parisdescartes.fr/~vincent/siten/Publications/theses/pdf/Siddiqi.pdf

Sur la question des indices visuels. Niveaux d’analyse : mise en page, ligne, charactère. Les informations déjà rassemblées pourraient lui être utile. Elle réfléchit à des critères qui pourraient être repérés.


### Collaboration possible avec Alix Chagué

Alix confirme qu’il pourraît être intéressant pour elle d’être formellement associée au projet. Afin de limiter le temps de sollicitation, on prévoit d’isoler les questions de transcriptions et d’identification des mains pour les traiter dans des réunions distinctes.

## Utilisation de ce document

Ce fichier est un journal pour le suivi quotidien du travail réalisé dans le cadre du projet *À dire d’experts*. Chaque jour, faire le compte-rendu du travail réalisé en relevant les points éventuellement problématiques qui devront être discutés en signalant les issues GitHub qui sont créées à ce sujet. Plutôt que dans ce document, on privilégie les discussions dans les issues afin de pouvoir les catégoriser avec des mots-clefs et en faire un suivi approprié.
