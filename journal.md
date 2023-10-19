# Journal du projet À dire d’experts

# 19 octobre 2023
- Travail sur les particularités des mains (abréviations, ponctuation, accentuation, etc.)
- Tableau comparatif des règles de transcription (Arianne Pinche, Maxime Gohier, Bernard Barbiche)
- Dépôt de la candidature [Cremma](https://cremmacall.sciencescall.org/)
- Installation de [Kraken](https://kraken.re/main/index.html)
  - Difficultés rencontrées pour l'installation de Kraken, puis pour l'installation d'un modèle de reconnaissance de texte (problèmes non résolus pour l'heure)
  - La segmentation fonctionne (modèle std BLLA) et ne semble effectivement pas très gourmande en ressource
    - sorties testées json et alto
    - outil de visualisation pour segmentation alto [https://github.com/emory-lits-labs/altoviz](https://github.com/emory-lits-labs/altoviz) - [http://emory-lits-labs.github.io/altoviz/](http://emory-lits-labs.github.io/altoviz/)

# 17 octobre 2023

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
    
# Réunion IERDJ, 16 octobre 2023 : Josselin, Sara, Emmanuel, Robert

## Discussion sur l’évaluation statistique

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

# Utilisation de ce document

Ce fichier est un journal pour le suivi quotidien du travail réalisé dans le cadre du projet *À dire d’experts*. Chaque jour, faire le compte-rendu du travail réalisé en relevant les points éventuellement problématiques qui devront être discutés en signalant les issues GitHub qui sont créées à ce sujet. Plutôt que dans ce document, on privilégie les discussions dans les issues afin de pouvoir les catégoriser avec des mots-clefs et en faire un suivi approprié.
