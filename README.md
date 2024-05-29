Julian Dezarnaud, SI4 Polytech Nice-Sophia, Valbonne

OlympicSavoyard

Jeu en ligne : https://dezarnaud-julian.github.io/](https://dezarnaud-julian.github.io/
Code source : https://github.com/Dezarnaud-Julian/OlympicSavoyard

Mode d’emploi :
-	 Flèche du haut : Duck/Démarrer la course
-	Flèche du bas :  Chasse-Neige (Freiner)
-	Flèche de gauche : Tourner à gauche
-	Flèche de droite : Tourner à droite

 

IL EST AUSSI POSSIBLE DE JOUER SUR MOBILE !!!!

Mode d’emploi Mobile :
-	Toucher le Haut de l’écran : Duck/Démarrer la course
-	Toucher le Bas de l’écran :  Chasse-Neige (Freiner)
-	Toucher le côté gauche : Tourner à gauche
-	Toucher le côté droite : Tourner à droite

Description :
Le projet Olympic Savoyard ou Alpine Ski est un jeu développé en utilisant Babylon.js. 
L'intégration de Babylon.js est réalisée via React.
Le jeu présente une piste de ski représentée par un mesh avec une texture de neige. Des particules sont utilisées pour simuler la neige qui tombe, et nous sommes au sein d’une sky-box représentant des montagnes, ajoutant à l'immersion dans l'environnement hivernal de nos belles montagnes savoyardes.
Le long de la piste, vous retrouverez des arbres, des roches et des logos des Jeux Olympiques pour toujours plus d’immersion.
Un soin particulier est appliqué au son pour retrouver une sensation de glisse et de vitesse proche de la réalité ! Je vous conseille vivement de jouer avec le son pour profiter du bruit du vent et de la neige. Quel plaisir de passer à toute vitesse près des drapeaux !
Le skieur, représenté par une combinaison de meshs pour le corps, les skis, le bonnet et les bâtons, évolue sur cette piste. Le parcours de ski est généré de manière sinusoïdale via une fonction pour représenter au mieux une descente à ski. Les boosters et les drapeaux sont placés le long de ce parcours en fonction de cette sinusoïde, permettant au joueur de les atteindre de manière réaliste et réalisable.
 
Le skieur est contrôlé à l'aide des touches directionnelles du clavier. En appuyant vers la gauche ou la droite, il tourne dans la direction correspondante. En appuyant vers le haut, le skieur effectue une animation de duck (accroupissement), ce qui lui permet d'augmenter sa vitesse au détriment de sa maniabilité. En appuyant vers le bas, le skieur fait le chasse-neige, ralentissant son allure mais lui permettant d'effectuer des virages serrés.
Une ligne bleue représente le parcours idéal à suivre, en passant du bon côté des drapeaux. Si le joueur suit cette ligne sans dévier, aucune pénalité n'est encourue. 
Le but du jeu est de terminer le parcours le plus rapidement possible tout en évitant les pénalités pour ainsi décrocher la médaille d’or et gagner les Jeux Olympiques !
Vous pouvez voir en temps réel votre vitesse et le temps actuel de votre parcours avec et sans les pénalités (+10 sec par drapeau raté).
Alors attention au top départ et aux cloches Savoyardes, que le meilleur gagne !
 

Démarche : 
Je fais mes études dans le Sud. Mais je suis originaire de Savoie et fier de l’être ! Alors quand j’ai entendue le mot Olympic, j’ai tout de suite pensé aux jeux Olympiques d’Hiver !
Au départ je voulais réaliser un jeu de saut à ski mais je n’ai pas réussi à rendre cette idée amusante à jouer et encore moins à développer. Donc je me suis penché vers le slalom !
Le plus dur à été de gérer la physique. Au départ je voulais me rapprocher de quelquechose de naturel et j’ai donc réalisé une piste penché et utilisé la gravité comme force de descente. Mais pour ensuite gérer le freinage et surtout pour tourner, c’est galère. Alors je suis revenu à quelque-chose de plat.
De plus la caméra de Babylon me jouait des tours et n’étais pas la même suivant les ordinateurs. Mais avec l’aide de Michel Buffa tout à été réglé !
Aussi j’ai fais l’erreur classique de charger à chaque fois le mesh pour chaque copies. Alors avec le nombre d’arbres et de drapeaux il fallait bien du temps pour que le jeu se charge ! Mais cela est maintenant réglé !
Il est assez difficile je trouve de doser la difficulté du jeu pour qu’il soit sympathique pour tous !
Je ne peux pas parler de gestion de la charge de travail et de l’équipe, car je suis seul à réalisé ce projet. Néanmoins je remercie mes amis d’avoir tester mon jeux à maintes reprises pour me donner des conseils d’équilibrage !
Je voulais représenter au mieux cette sensation de glisse qui m’est cher ! Alors je suis très fier des sons de glisse qui pour moi rajoute énormément au jeu !

Vidéo de 30 secondes de Gameplay : Lien YT

