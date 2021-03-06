---
layout: page
title: Generative adversarial networks
---

## Définition :
Un réseau génératif adversaire (GAN) est un type de technique d'apprentissage automatique (ML) de l'IA composée de deux réseaux qui se font concurrence dans un cadre de jeu à somme nulle. Les GAN fonctionnent généralement sans supervision et apprennent à reproduire toute distribution de données donnée.
 
Les deux réseaux de neurones qui constituent un GAN sont appelés le générateur et le discriminateur. Le générateur est un type de réseau de neurones de convolution qui créera de nouvelles instances d'un objet, et le discriminateur sera un type de réseau de neurones déconvolutionnel qui déterminera son authenticité ou son appartenance à un jeu de données.

![gan1](/Images/gan1.png/)

Les deux entités se font concurrence lors du processus d’entraînement, où leurs pertes se font l’un l’autre pour améliorer les comportements, ce qu’on appelle la rétropropagation. Le but du générateur est de produire une sortie passable sans être attrapé tandis que le but du discriminateur est d’identifier les contrefaçons. Alors que la double boucle de rétroaction continue, le générateur produit une sortie de meilleure qualité et le discriminateur devient plus apte à signaler les imposteurs.

## Principe :
La première étape de la création d'un GAN consiste à identifier le résultat final souhaité et à rassembler un jeu de données d'apprentissage initial en fonction de ces paramètres. Ces données sont ensuite randomisées et entrées dans le générateur jusqu'à ce qu'il acquière une précision de base dans la production de sorties. En effet, GAN échantillonne le bruit **z** en utilisant une distribution normale ou uniforme et utilise un générateur de réseau en profondeur **G** pour créer une image **x (x = G (z))**.

![gan2](/Images/gan2.png/)

Ensuite, les images générées sont introduites dans le discriminateur avec les points de données réels du concept d'origine. Le discriminateur filtre les informations et renvoie une probabilité comprise entre 0 et 1 pour représenter l'authenticité de chaque image (1 corrélation avec réel et 0 corrélation avec faux).

![gan3](/Images/gan3.png/)

Ces valeurs sont ensuite vérifiées manuellement pour vérifier leur succès et répétées jusqu'à ce que le résultat souhaité soit atteint. Les deux réseaux tentent d'optimiser une fonction différente et opposée dans un jeu à somme nulle.

## Fonction de coût et backpropagation :
Maintenant, nous allons passer par quelques équations simples. Le discriminateur émet une valeur **D(x)** indiquant le risque que x soit une image réelle. Notre objectif est de maximiser les chances de reconnaître les images réelles en tant qu'images réelles et les images générées en tant que faux, c'est-à-dire le maximum de vraisemblance des données observées. Pour mesurer la perte, nous utilisons une entropie croisée comme dans la plupart des Deep Learning pour une image réelle, **p** (l’étiquette vraie pour les images réelles) est égal à **1**. Pour les images générées, nous inversons l’étiquette (c’est-à-dire une étiquette moins). Donc l'objectif devient:

![gan4](/Images/gan4.png/)

où **D(x)** est la probabilité que **D** classe **x** comme une vraie image, et **G(z)** est une image générée par **G** à partie de **z**. Le terme de gauche de la somme correspond donc à optimiser la probabilité qu'une vraie image **x** soit classée vraie et le terme de droite correspond à optimiser la probabilité qu'une image générée **G(z)** ne soit pas classée vraie.
D’un côté,  le discriminateur **D** essaie de maximiser son taux de réussite. Donc il va faire une ascension de gradient de la fonction du coût:

![gan5](/Images/gan5.png/)

Du côté du générateur, sa fonction objective veut que le modèle génère des images avec la plus grande valeur possible de **D(x)** pour tromper le discriminateur :

![gan6](/Images/gan6.png/)

Une fois les deux fonctions objectives définies, elles sont apprises conjointement par la descente à gradient alterné. Nous fixons les paramètres du modèle du générateur et effectuons une seule itération de descente de gradient sur le discriminateur en utilisant les images réelles et générées. Ensuite, nous alternons entre les deux réseaux jusqu'à ce que le générateur produise des images de bonne qualité. Le schéma qui suit résume le flux de données et les gradients utilisés pour la rétro-propagation.

![gan7](/Images/gan7.png/)

Le pseudo-code ci-dessous rassemble tout et montre comment le GAN est entrainé.

![gan8](/Images/gan8.png/)

## Les bénéfices d’une architecture adversaire :
* **GAN fonctionne avec nombre de données minimal** : Le premier obstacle à l’entrainement d’un réseau de neurones sur un nouveau sujet est la nécessité d’un vaste pool de jeux de données étiquetées. La création de ce pool de données est un travail monotone et extrêmement fastidieux. Les réseaux génératifs adversaires partent toutefois d'une perspective différente. Les données précieuses sont les données sur lesquelles le réseau apprend, à savoir les données fournies par le réseau génératif. L'ensemble de données réelles auquel le réseau discriminant a accès peut rester relativement petit, par rapport à d'autres méthodes.
* **Entraînement avec moins de supervision humaine** : Ce procédé a pour premier avantage d’être un apprentissage de type non supervisé. En effet en décrivant le fonctionnement des réseaux classifieurs on avait souligné le besoin de données préalablement labélisées afin de pouvoir enseigner au réseau à correctement classer. Hors ici aussi on utilise un réseau classifieur, à savoir D, cependant ici les classes correspondent à l’origine des images : vraie pour celle venant de la base de donnée et générée pour celles produites par G. Ces labels sont donc générés automatiquement et ne requiert pas qu’un opérateur humain annote à la main chaque image, ce qui est toujours un gain de temps immense. Il faut cependant tempérer cet avantage car si effectivement on n’a pas besoin d’annoter de données, il faut toutefois disposer d’une base de données avec laquelle comparer celles provenant de G, et le travail de sélection et collection de données correspondant à ce que l’on souhaite produire n’est pas toujours négligeable non plus.
* **Résultats de haute qualité** : L’un des résultats les plus intéressants de cette architecture  adversaire  artificielle basée sur de faibles données et sur une faible supervision humaine est qu’elle génère des résultats de très haute qualité. *Ian Goodfellow*, qui a présenté son exposé en 2016, a montré plusieurs résultats cocasses dans lesquels une IA adversaire produisait des images d'animaux présentant de nombreuses caractéristiques d'animaux réalistes dépourvus d'anatomie perceptible. 

## Limites : 
* **Un processus instable** : La force des GANs vient de la saine compétition entre le faussaire et le détective, qui permet à chacun de progresser en s’appuyant sur la réussite de l’autre. Cependant pour que cette compétition se déroule réellement de cette façon, il faut que chaque réseau progresse au même rythme. En effet dans le cas où un des deux réseaux se met à progresser plus rapidement que l’autre, l’écart va se creuser et ce réseau va finir par écraser l’autre sans que ce dernier puisse rattraper son retard. Il faut donc contrôler que la progression de chaque réseau afin d’éviter une situation de domination d’un réseau sur l’autre.
* **Ne fonctionne que sur de faibles résolutions** : Une des conséquences de la nécessité d’une convergence homogène et simultanée des réseaux est la difficulté de travailler avec des données complexes, en particulier de haute résolution dans le cas des images. En effet plus il y a de pixels et de possibilités de détails, plus le réseau génératif aura une convergence lente car il aura beaucoup de détails à essayer avant de trouver ceux réalistes. A l’inverse le réseau classifieur aura plus de détails sur lesquels s’appuyer pour différencier le vrai du faux et verra donc sa tâche simplifiée, il convergera donc plus rapidement. Conserver une convergence homogène entre les deux réseaux dans ces conditions se révèle alors très difficile. Toutefois des solutions à ce problème commencent à apparaître, notamment avec les travaux de Nvidia, dont des résultats ont été présentés plus haut. Leur solution consiste à augmenter progressivement la résolution des images. On commence alors par des images très peu précises pour lesquelles le faussaire aura plus de faciliter à tromper le détective, puis petit à petit le niveau de détails augmente et le faussaire pourra s’appuyer sur ses croquis grossiers pour leur ajouter des détails au fur et à mesure. Cela ressemble au processus réalisation d’une peinture réaliste : on commence par faire un croquis grossier de la scène pour ajuster les proportions puis on va se concentrer sur des zones précises pour ajouter des détails, puis les couleurs principales et enfin les effets de lumière plus subtiles.
* **Le mode collapse** : Générer des données ne présente un intérêt que si l’on génère de nouvelles données. La plus-value réside dans la nouveauté, la diversité des données générées, sinon il suffirait de copier une donnée existante et on ne s’embêterait pas avec toutes ces histoires compliquées de réseaux. Cependant le réseau génératif G lui n’a pas de notion de nouveauté à maximiser, son objectif est simplement de tromper D et dans ce but, fournir une copie d’une donnée existante est une très bonne solution. Ce comportement est évité car G n’a pas accès aux données réelles que D examine, il est alors extrêmement improbable que par hasard G reproduise parfaitement une donnée de la base de données par hasard. La nouveauté est donc a priori garantie. Ce n’est en revanche pas le cas de la diversité. En effet la diversité des images crées vient du vecteur latent z donné en entrée de G. Pour obtenir deux images différentes en sortie il suffit de générer (aléatoirement ou non) deux z différents. Cependant cela n’est pas toujours aussi simple, en effet un comportement vers lequel G peut converger est celui de très peu tenir compte de z et renvoyer des images presque toujours identiques. Cela vient du fait que si G trouve lors de son apprentissage une image qui marche très bien pour confondre D, une solution pour gagner souvent le jeu du faussaire sera d’exploiter cette image le plus souvent possible quel que soit l’entrée z. On obtiendra alors un réseau qui génère des images toujours très semblables. C’est que qu’on appelle le mode collapse. Ce problème est particulièrement présent dans les utilisations de GANs conditionnels (cGANs) qui permettent d’imposer des conditions sur la donnée que l’on veut produire en choisissant le vecteur latent dans certaines zones du latent space correspondant aux caractéristiques imposées par la condition. L’amplitude de variation de z sera alors contrainte par les conditions ce qui implique que si le réseau n’est pas sensible à de petites variations de z, on aura toujours le même résultat pour une condition donnée. Par exemple on peut observer ce comportement pour le réseau de Reed et al qui permet de générer des images à partir d’un texte. On constate sur la figure ci-dessous que pour les images d’oiseaux la diversité semble acceptable tandis que les images de fleurs sont sensiblement identiques.

## Références :
* https://arxiv.org/pdf/1406.2661.pdf
* https://blog.exxactcorp.com/the-benefits-of-adversarial-ai/
* https://medium.com/@jonathan_hui/gan-why-it-is-so-hard-to-train-generative-advisory-networks-819a86b3750b

