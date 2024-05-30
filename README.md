Julian Dezarnaud, SI4 Polytech Nice-Sophia, Valbonne

Alpine Ski

Jeu en ligne: https://dezarnaud-julian.github.io/
Code source: https://github.com/Dezarnaud-Julian/OlympicSavoyard
Site Web Buildé: https://github.com/Dezarnaud-Julian/dezarnaud-julian.github.io
 


Mode d’emploi :
-	Flèche du haut : Duck/Démarrer la course
-	Flèche du bas :  Chasse-Neige (Freiner)
-	Flèche de gauche : Tourner à gauche
-	Flèche de droite : Tourner à droite
-	R pour rejouer

IL EST AUSSI POSSIBLE DE JOUER SUR MOBILE !!!!
Mode d’emploi Mobile :
-	Toucher le Haut de l’écran : Duck/Démarrer la course
-	Toucher le Bas de l’écran :  Chasse-Neige (Freiner)
-	Toucher le côté gauche : Tourner à gauche
-	Toucher le côté droite : Tourner à droite



Description :
Le projet Olympic Savoyard ou Alpine Ski est un jeu développé en utilisant Babylon.js pour la 3D et React pour la UI (User Interface) 2D.
Je suis partie d’un project React CRA (Create react App), un outil qui crée un site web vide React prêt à l’emploi en Type Script. Dedans j’ai rajouté le framework Babylon pour permettre la création du jeu en 3D.
Le jeu présente une piste de ski représentée par un mesh avec une texture de neige. Des particules sont utilisées pour simuler la neige qui tombe, et nous sommes au sein d’une sky-box représentant des montagnes, ajoutant à l'immersion dans l'environnement hivernal de nos belles montagnes savoyardes.
Le long de la piste, vous retrouverez des arbres, des roches et des logos des Jeux Olympiques pour toujours plus d’immersion.
Un soin particulier est appliqué au son pour retrouver une sensation de glisse et de vitesse proche de la réalité ! Je vous conseille vivement de jouer avec le son pour profiter du bruit du vent et de la neige. Quel plaisir de passer à toute vitesse près des drapeaux !
Le skieur, représenté par une combinaison de meshs pour le corps, les skis, le bonnet et les bâtons, évolue sur cette piste. Le parcours de ski est généré de manière procédurale en se basant sur des courbes sinusoïdales pour représenter au mieux une descente à ski. Les boosters et les drapeaux sont placés le long de ce parcours en fonction de cette sinusoïde, permettant au joueur de les atteindre de manière réaliste et réalisable.

Le skieur est contrôlé à l'aide des touches directionnelles du clavier. En appuyant vers la gauche ou la droite, il tourne dans la direction correspondante. En appuyant vers le haut, le skieur effectue une animation de duck (accroupissement), ce qui lui permet d'augmenter sa vitesse au détriment de sa maniabilité. En appuyant vers le bas, le skieur fait le chasse-neige, ralentissant son allure mais lui permettant d'effectuer des virages serrés.
Une ligne bleue représente le parcours idéal à suivre, en passant du bon côté des drapeaux. Si le joueur suit cette ligne sans dévier, aucune pénalité n'est encourue. 
Le but du jeu est de terminer le parcours le plus rapidement possible tout en évitant les pénalités pour ainsi décrocher la médaille d’or et gagner les Jeux Olympiques !
Vous pouvez voir en temps réel votre vitesse et le temps actuel de votre parcours avec et sans les pénalités (+10 sec par drapeau raté).
Alors attention au top départ et aux cloches Savoyardes, que le meilleur gagne !
 


Démarche : 
Je fais mes études dans le Sud. Mais je suis originaire de Savoie et fier de l’être ! Alors quand j’ai entendue le mot Olympic, j’ai tout de suite pensé aux jeux Olympiques d’Hiver ! Donc je me suis penché vers le slalom !
Le plus dur à été de gérer la physique. Au départ je voulais me rapprocher de quelquechose de naturel et j’ai donc réalisé une piste penché et utilisé la gravité comme force de descente. Mais cela posait des problèmes au niveau de la caméra qui avait du mal à suivre le joueur et au niveau des forces à appliquer pour gérer le freinage et surtout pour tourner. Alors je suis revenu à quelque-chose de plat.
De plus la caméra de Babylon me jouait des tours et n’étais pas la même suivant les ordinateurs. Mais avec l’aide de Michel Buffa tout à été réglé !
Avoir une physique constantes de PCs en PCs à été très compliqué et je ne suis pas tout à fait sûr que ce sois totalement stabilisé. J’ai remarqué que suivant les machines beaucoup de choses changeait sans raisons apparente (changements de vitesses). 
Les physiques vont plus ou moins vites suivant les machines. C’est un gros problème vu que c’est un jeu chonométré ! On veut quelque-chose d’uniforme. Donc en recherchant un peu sur les forums j’ai vue une fonction onBeforePhysicRenderLoop() qui permet d’avoir quelquchose de stable mais cela reste assez flou et je ne sais pas si cela règle totalement le problème.
Aussi j’ai fais l’erreur classique de charger à chaque fois le mesh pour chaque copie de drapeau alors qu’il suffisait juste de le mettre en cache pour charger le modèle une seule fois et ensuite le cloner.
Pour les décors (arbres, rochers), ils sont non interactifs, donc pour améliorer les perfs j’ai fais des instances. Elles me permettes de déplacer la charge de travail sur la carte graphique plutôt que sur le CPU.
Il est assez difficile je trouve de doser la difficulté du jeu pour qu’il soit sympathique pour tous !
Je ne peux pas parler de gestion de la charge de travail et de l’équipe, car je suis seul à réaliser ce projet. Néanmoins je remercie mes amis d’avoir testé mon jeu à maintes reprises pour me donner des conseils d’équilibrage !
Je voulais représenter au mieux cette sensation de glisse qui m’est cher ! Alors je suis très fier des sons de glisse qui pour moi rajoute énormément au jeu !
Les sons viennent de FreeSound !
Pour ce qui est du son et des animations, c’est géré procéduralement. Le volume du son est plus ou moins fort suivant certaines données, comme la position vis-à-vis des drapeaux et de la vitesse. Le son quand on freine ou que l’on tourne est plus ou moins fort, il boucle en loop et le volume est géré aussi en fonction de la vitesse.
Pour les animations du personnages, tout à été créé à partir de primitifs  ( cube, cylindre et sphere)  j’ai pu tourner chaque parties du corps indépendamment et les skis en fonction de la vitesse du joueur, ce qui permet d’avoir des rotations fluides et qui correspondent à ce que le joueur est en train de faire et sa vitesse.
Les drapeaux se penchent plus ou moins fort quand on les touchent suivant la vitesse et la distance avec eux. Donc au lieu d’une animation statique ont à quelque-chose en fonction de la vitesse encore une fois.
Les modèles 3D on été fait avec Blender et certains récupéré sur Sketchfab.
J’ai utilisé React car j’aime bien ce FrameWork et j’ai l’habitude de l’utiliser. Et comme je voulais faire une UI en 2D j’ai regardé si je pouvais combiner React et Babylon. Cela n’as pas été évident mais j’ai réussi et je peux donc utiliser ce que je connais de React pour réaliser la UI. Les deux communique car React s’abonne à un objet Game (une classe typeScript) et il va utiliser les objets à l’intérieur pour avoir le chrono, le temps courant du chrono, la vitesse, l’état du jeu (la course est finit ou non) pour ainsi afficher le panneau de tuto ou de fin.
Pour le hosting j’ai utilisé ce que propose Git car c’était gratuit, simple et pratique. J’ai juste à mettre le site web buildé directement sur le répo.
J’ai regardé pour faire des terrains dynamique avec des bosses et autre mais c’était trop compliqué.
Pour la version mobile, afin d’optimiser et fluidifier le jeu, j’elève beaucoup de modèles décoratifs comme les arbres, rocher et panneaux.



Vidéo Gameplay : Lien YT
