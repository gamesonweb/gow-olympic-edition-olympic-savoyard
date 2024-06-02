<h1 align="center">❄️ SKI ALPIN ⛷️</h1>
<p align="center">
  
  <img src="https://github.com/gamesonweb/gow-olympic-edition-olympic-savoyard/assets/115253780/b7a4b48d-c540-494d-912b-82852d4ee94d" alt="cover" width="1000"/>
  <img src="https://github.com/gamesonweb/gow-olympic-edition-olympic-savoyard/assets/115253780/f251e8a9-4530-4606-b7d1-38e8d04f1907" alt="ingame" width="1000"/>
  <img src="https://github.com/gamesonweb/gow-olympic-edition-olympic-savoyard/assets/115253780/8be712a5-f575-42fc-96b7-a8ba44bb353a" alt="leaderboard" height="400"/>
  <img src="https://github.com/gamesonweb/gow-olympic-edition-olympic-savoyard/assets/115253780/7717dbe3-2dd6-4129-b821-6a23a4677788" alt="mobile" height="400">
</p>

### 🏔️ Plongez au cœur des montagnes enneigées !

Découvrez une piste de ski créée avec soin, entourée de montagnes majestueuses, d'arbres enneigés, et de rochers. 

### ⛷️🚩⏱️ Slalomez entre les drapeaux sans erreur pour obtenir le meilleur chrono !

**Suivez la ligne bleue** pour passer les drapeaux sans pénalité. 

**Accélérez, freinez, et virez à toute vitesse** pour finir le parcours le plus rapidement possible. 

**Chaque drapeau manqué ajoute 10 secondes** à votre temps – visez la perfection pour décrocher la médaille d'or!

### 🔊🎧 Sentez la glisse !

Écoutez le vent siffler, la neige crisser sous vos skis, et les cloches savoyardes résonner alors que vous dévalez la pente à toute allure!

**Jouez avec un casque pour une expérience maximale.**

### 🥇 Arriverez vous à terminer 1er au Slalom Olympique ?

<br/>

## À vos marques, prêts, partez! 🏁

### [Jouez sur **PC 💻⌨️ et mobile 📱**](https://dezarnaud-julian.github.io/)

Vous pouvez aussi lancer le jeu sur mobile en scannant ce QR Code :

<img src="https://github.com/gamesonweb/gow-olympic-edition-olympic-savoyard/assets/115253780/190b2b02-55d5-4659-8ca8-7dec89aa54ef" alt="qrcode" width="200"/>

### [Vidéo de 30 secondes de Gameplay](https://youtu.be/fGPHrEuFtO4)

### [Code source](https://github.com/Dezarnaud-Julian/OlympicSavoyard)

### [Repo du site web](https://github.com/Dezarnaud-Julian/dezarnaud-julian.github.io)

## Contrôles ⌨️ 📱
Prenez le contrôle de votre skieur avec des commandes simples.

![image](https://github.com/gamesonweb/gow-olympic-edition-olympic-savoyard/assets/115253780/c801675b-97ea-4f2f-87cf-b27759bd05f4)

Pour jouer en plein écran, clickez sur le bouton rouge en haut à droite de l'écran.

### Mode d’emploi sur ordinateur ⌨️

- **Flèche du haut** : Sprinter / Démarrer la course
- **Flèche du bas** : Chasse-Neige (Freiner)
- **Flèche de gauche** : Tourner à gauche
- **Flèche de droite** : Tourner à droite

### Mode d’emploi sur mobile 📱

- **Glisser le doight vers le haut** : Sprinter / Démarrer la course
- **Glisser le doight vers le bas** : Chasse-Neige (Freiner)
- **Glisser le doigt vers la gauche** : Tourner à gauche
- **Glisser le doigt vers la droite** : Tourner à droite

Jouez en mode portrait ou paysage !

Pour avoir une meilleure experience, **vous pouvez installez un raccourci du jeu sur votre mobile** :

Cliquez sur les 3 petits points en haut à droite de votre navigateur > Ajouter en raccourci / Add to home screen

<img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fsupport.startpage.com%2Fhc%2Farticle_attachments%2F6738385280020%2FScreenshot_20220603-095113.jpeg&f=1&nofb=1&ipt=97a250a51d63ba43d3e9fe40050bb28948e4a2ff4858111eaf397eeaf473c1f2&ipo=images" alt="drawing" width="200"/>

## Equipe 🧍

Julian Dezarnaud, SI4 Polytech Nice-Sophia, Valbonne

## Description du developpement 👨‍💻

Je fais mes études dans le Sud. Mais je suis originaire de Savoie et fier de l’être ! Alors quand j’ai entendue le mot Olympic, j’ai tout de suite pensé aux jeux Olympiques d’Hiver ! Je me suis donc penché vers le slalom !

### Problématiques & features

#### Créer la base du site ⚛️

- Babylon.js pour la 3D
- React pour la UI (User Interface) 2D.

Je suis parti d’un project React CRA (Create react App), un outil qui crée un site web vide React prêt à l’emploi en Typescript. Dedans j’ai rajouté le framework Babylon pour permettre la création du jeu en 3D.

#### Générer un parcours de slalom 🚩

Le parcours de ski est généré de manière procédurale en se basant sur des courbes sinusoïdales pour représenter au mieux une descente à ski. Les boosters et les drapeaux sont placés le long de ce parcours en fonction de cette sinusoïde, permettant au joueur de les atteindre de manière réaliste et réalisable. J'utilise la tangeante de la courbe pour savoir quand placer un drapeau. Le slalom de ce jeu enchaîne donc plusieurs sinusoidales différentes pour changer le parcours.

Exemple :

- 1ère partie du slalom : `(x) => Math.sin(x \* 0.01)`
- Dernère partie : `(x) => Math.sin(x \* 0.03) \* 0.2 + Math.sin(x \* 0.01)`

#### Avoir des physiques de jeu fun et intéressantes ⛷️

Au départ je voulais me rapprocher de quelque-chose de naturel et j’ai donc réalisé une piste penchée et utilisé la gravité comme force de descente. Mais j'ai rencontré des problèmes au niveau de la caméra qui avait du mal à suivre le joueur et au niveau des forces à appliquer pour gérer le freinage et surtout pour tourner. Je suis donc revenu à un terrain plat.

Des multiplicateurs de vitesse pour tourner / avancer sont calculés en fonction de la position du joueur : Si le joueur s'accroupie pour sprinter, le multiplcateur de vitesse augemente au dessus de 1, au contraire il descends en dessous si le joueur freine. Le multiplicateur de vitesse de direction s'adapte également et est à 1 quand le joueur ne fait rien. S'accroupir pour sprinter ou freiner pénalise le joueur dans son tournant afin de l'obliger à utiliser tous les mouvements pour réussir au mieux son parcours.

#### Chronomètre et pénalités ⏱️❌

Vous pouvez voir en temps réel votre vitesse et le temps actuel de votre parcours avec et sans les pénalités (+10 sec par drapeau raté). Le chronomètre est géré par une classe typescript qui comptailise également les pénalités.
On sait si le joueur passe correctement ou pas le drapeau en fonction de sa position en X.

#### Une camera fonctionnelle et stable d'ordinateurs en ordinateur 🎥

La caméra de Babylon ne se comportait pas pareil suivant les ordinateurs. Elle était soit trop loin, soit trop proche. Parfois même mal cadrée. Aec l’aide de Michel Buffa j'ai compris qu'il fallait écouter l'évnènement window resize pour dir à l'engine de se retailler correctement. J'ai aussi bin fait attention à ce que le canvas change bien sa taille selon la résolution des écrans

#### Avoir une physique stable 📊

Avoir une physique constante d'ordinateur en ordinateur a été très compliqué et je ne suis pas tout à fait sûr que ce soit totalement stabilisé aujourd'hui ! J’ai remarqué que suivant les machines le skieur allait plus vite ou moins vite, cassant complètement le jeu puisque le but est de réaliser le meilleur chrono (qui lui reste stable car j'utilise des intervales en javascript) ! J'ai ajouté une boucle de rendu de la physique qui est appellée sur un `onBeforeStepObservable` de la scene + rajouté un time step sur le plugin de physique Havok + Ajouté des options dans l'engine `{deterministicLockstep: true, lockstepMaxSteps: 1}`. Malheuresement ce problème ne semble pas être totalement résolu.. Si vous avez une solution à ce problème je suis preneur !

#### Optimiser le chargement de meshs et materiels ♻️

Pour la génération de la piste de slalom j'avais fait l'erreur de charger un mesh de drapeau à chaque fois que je devait poser un drapeau. Tous mes meshs/materials sont maintenant chargés/crées une fois, enregistrés en chache dans un variable et clonés lors de l'utilisation.

#### Optimiser le chargement du décor 🐑

Générer tout une forêt en utilisant des meshs classiques dépense vite des resources. Vu que ces décors sont non interactifs et ne bougent pas j'ai voulu améliorer les perfs en utilisant le système d'instances. Elles me permettes de déplacer la charge de travail sur la carte graphique plutôt que sur le CPU. Je n'ai pas pris le temps de créer des ThinInstances, qui semblent encore plus optimisés et adaptés à mon cas.

#### Doser la difficultée du jeu 🎚️

Trouver un niveau de difficultée qui aille à tout le monde est compliqué, surtout que je voulais que ce jeu soit accessible à tous mais dans lequel il y aie un grand niveau de progression pour devenir un pro de la glisse ! Je pense qu'aujourd'hui le jeu est relativement accessible, un joueur arrivera à s'approcher du podium sans trop de problème, il faudra plus d'efforts pour obtenir la première place en revanche !

#### Gestion du travail 👷

Je ne peux pas parler de gestion de la charge de travail et de l’équipe, car je suis seul à réaliser ce projet. Néanmoins je remercie mes amis d’avoir testé mon jeu à maintes reprises pour me donner des conseils d’équilibrage !

#### Gestion des sons 🔊

Je voulais représenter au mieux cette sensation de glisse qui m’est chère ! Pour ce faire j'ai vraiment travaillé tous les sons qui composent le jeu. Le bruit de vent en fond deviens plus ou moins fort selon la vitesse du joueur, de même pour le bruit de neige qui crépite. De manière générale j'utilise le changement de vitesse du joueur pour augmenter ou diminuer le volume des sons. Le bruit de vent quand on passe près d'un drapeau est aussi proportionnel à la vitesse et la rpoximité avec celui-ci.

#### Animer le personnage ⛷️

Je ne trouvais pas de modèle de skieur intéressant et je ne me sentais pas de le faire sur blender. J'ai donc réalisé le personnage à base de primitifs (cube, cylindre et sphere). Cela m'a permis de créer les animations du personnage procéduralement, car j’ai pu tourner/déplacer chaque partie du corps et le skis indépendamment en fonction de la vitesse du joueur et de ses déplacements. Cela permet vraiment d'avoir des rotations fluides et qui correspondent à ce que le joueur est en train de faire sur la piste.

#### Le fun de taper les drapeaux 🚩

Les drapeaux se penchent plus ou moins fort quand on les touche suivant la vitesse et la distance avec eux. Donc au lieu d’une animation statique ont a quelque-chose en fonction de la vitesse encore une fois.

#### Créer la UI 💻

J’ai utilisé React car j’aime bien ce Framework et j’ai l’habitude de l’utiliser. Comme je voulais faire une UI en 2D j’ai regardé si je pouvais combiner React et Babylon. Cela n’a pas été très évident au début mais à présent je peux utiliser ce que je connais en React pour réaliser la UI. La UI s’abonne à un objet Game (une classe typeScript) et il va utiliser les objets à l’intérieur pour avoir le chrono, le temps courant du chrono, la vitesse, l’état du jeu (la course est finie ou non) pour ainsi afficher le panneau de tuto ou de fin.

#### Héberger le jeu 🎮

Pour le hosting j’ai utilisé ce que propose Git car c’était gratuit, simple et pratique. J’ai juste eu à mettre le site web buildé directement sur un répo qui a le même nom que mon username.

#### Terrain bosselé ⛰️

J’ai regardé pour faire des terrains dynamique avec des bosses et autre mais c’était trop compliqué dans le temps imparti que j'allouais au projet. Son utilisation allait aussi changer grandement les physiques de mon personnages, j'ai donc vite écarté cette piste et décidé de rester simple.

#### Version mobile (optimisation et contrôles) 📱

Pour la version mobile, afin d’optimiser et fluidifier le jeu, j’enlève beaucoup de modèles décoratifs comme les arbres, rocher et panneaux. J'ai dû aussi adapter ma classe de Contrôles pour permettre de contrôler le personnage via des inputs retournant des valeurs continues plutôts que des valeurs discrètes comme sur clavier.
J'ai dû aussi porter une attention particulière à la UI pour faire en sorte qu'elle s'affiche correctement sur mobile et éviter des problèmes d'overscrolling / textes selectionnés pour les copier grâce au CSS.

#### Skybox 🌄

J'ai utilisé un script python trouvé sur le net pour adapter une skybox qui me plaisait bien au format accapté par babylon (une skybox découpé par faces plutôt qu'une grand image simple).

#### Particules ❄️

J'ai utilisé le playground de babylon pour créer mon système de particules en live. Une fois satisfait de mon système, je l'ai sauvegardé au format json et chargé dans mon code via le json importer pour les particule systems.

### Ressources 🧰

- Outils :

  - Blender (pour la 3D)
  - Photopea (pour la réalisation de graphismes)
  - Audacity (pour éditer des sons)

- Sons (Fressound) :

  - [bruit de ski sur neige](https://freesound.org/people/Nox_Sound/sounds/612662/)
  - [cloches](https://freesound.org/people/hoersturz/sounds/60514/)
  - [applaudissements_1](https://freesound.org/people/mglennsound/sounds/678542/)
  - [applaudissements_2](https://freesound.org/people/kevp888/sounds/735278/)
  - [boost](https://freesound.org/people/JomelleJager/sounds/248210/)
  - [erreur](https://freesound.org/people/F.M.Audio/sounds/662346/)
  - [chrono début/fin](https://freesound.org/people/micadoe/sounds/178713/)
  - [vent près des drapeaux](https://freesound.org/people/Josethehedgehog/sounds/390367/)
  - [drapeau touché](https://freesound.org/people/EdgardEdition/sounds/113634/)

- Modèles 3D (Sketchfab):

  - [sapin sur sketchfab](https://sketchfab.com/3d-models/low-poly-pine-tree-c1d140fc15bd40928989d0ca79365e13)
  - [rochers sur sketchfab](https://sketchfab.com/3d-models/low-poly-rocks-9823ec262054408dbe26f6ddb9c0406e)
  - le reste est crée à la main sur blender
