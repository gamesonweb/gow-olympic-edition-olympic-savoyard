## Documentation Globale du Projet

 Le projet utilise des technologies telles que Python, React, Babylon.js, et diverses bibliothèques pour gérer les éléments 3D, la physique, les sons, et l'interface utilisateur.

### Structure Générale du Projet

1. **Scripts Python**
   - **Description**: Un script pour diviser une image cubemap/skymap en 6 fichiers images séparés pour une utilisation dans une skybox sous Unity.
   - **Dépendances**: PIL (Python Imaging Library)
   - **Exécution**: `python script.py image-name.png`

2. **Application React avec Babylon.js**
   - **Description**: Une application de jeu de slalom en ski utilisant Babylon.js pour le rendu 3D et React pour l'interface utilisateur.
   - **Composants**:
     - `SceneComponent`: Gère le rendu de la scène Babylon.js.
     - `UI`: Gère l'affichage de l'interface utilisateur, y compris le chronomètre, les pénalités, et le tableau de scores.
     - `Score`: Affiche le score d'un joueur.
     - `App`: Point d'entrée principal de l'application, initialise le jeu et les composants de la scène.

3. **Jeu SkiSlalomGame**
   - **Description**: Un jeu de slalom en ski où le joueur navigue à travers une piste, évite les obstacles et essaie de terminer le parcours le plus rapidement possible.
   - **Classes**:
     - `Player`: Représente le joueur avec ses propriétés et comportements.
     - `Door`: Représente une porte dans le jeu, avec des états et des sons associés.
     - `Ramp`: Représente une rampe dans le jeu.
     - `Boost`: Représente un boost dans le jeu.
     - `Terrain`: Représente le terrain du jeu, généré dynamiquement.
   - **Fonctionnalités**:
     - Gestion de la physique avec le plugin Havok.
     - Effets de particules pour la neige.
     - Sons pour diverses interactions dans le jeu.
     - Génération dynamique du terrain et des obstacles.

### Documentation du Script Python

#### Description

Ce script divise une image cubemap/skymap PNG produite par Blender en 6 images séparées pour une utilisation dans une skybox sous Unity.

#### Prérequis

- Python Imaging Library (PIL): [http://www.pythonware.com/products/pil/](http://www.pythonware.com/products/pil/)

#### Utilisation

Pour exécuter le script, utilisez la commande suivante :
```bash
python script.py image-name.png
```

#### Script

```python
#!/usr/bin/env python

from PIL import Image
import sys, os

path = os.path.abspath("") + "/"
processed = False

def processImage(path, name):
    img = Image.open(os.path.join(path, name))
    size = img.size[0] / 3  # Divise la largeur de l'image par 3, attend une disposition 3x2 produite par Blender.
    splitAndSave(img, 0, 0, size, addToFilename(name, "_px"))
    splitAndSave(img, size, 0, size, addToFilename(name, "_nz"))
    splitAndSave(img, size * 2, 0, size, addToFilename(name, "_nx"))
    splitAndSave(img, 0, size, size, addToFilename(name, "_ny"))
    splitAndSave(img, size, size, size, addToFilename(name, "_py"))
    splitAndSave(img, size * 2, size, size, addToFilename(name, "_pz"))

def addToFilename(name, add):
    name = name.split('.')
    return name[0] + add + "." + name[1]

def splitAndSave(img, startX, startY, size, name):
    area = (startX, startY, startX + size, startY + size)
    saveImage(img.crop(area), path, name)

def saveImage(img, path, name):
    try:
        img.save(os.path.join(path, name))
    except Exception as e:
        print("*   ERROR: Could not convert image.", e)
        pass

for arg in sys.argv:
    if ".png" in arg or ".jpg" in arg:
        processImage(path, arg)
        processed = True

if not processed:
    print("*  ERROR: No Image")
    print("   usage: 'python script.py image-name.png'")
```

### Documentation de l'Application React avec Babylon.js

#### Composants

1. **SceneComponent**

   Rendu d'un canvas HTML pour afficher une scène Babylon.js.

   ```javascript
   import { useEffect, useRef } from "react";
   import { Engine, Scene } from "@babylonjs/core";

   export default function SceneComponent({ autoFocus, width, height, antialias, engineOptions, adaptToDeviceRatio, sceneOptions, onRender, onSceneReady, id }) {
     const reactCanvas = useRef(null);

     useEffect(() => {
       const { current: canvas } = reactCanvas;

       if (!canvas) return;

       const engine = new Engine(canvas, antialias, engineOptions, adaptToDeviceRatio);
       const scene = new Scene(engine, sceneOptions);

       if (scene.isReady()) {
         onSceneReady(scene);
       } else {
         scene.onReadyObservable.addOnce((scene) => onSceneReady(scene));
       }

       engine.runRenderLoop(() => {
         if (typeof onRender === "function") onRender(scene);
         scene.render();
       });

       const resize = () => {
         scene.getEngine().resize();
       };

       if (window) {
         window.addEventListener("resize", resize);
       }

       return () => {
         scene.getEngine().dispose();
         if (window) {
           window.removeEventListener("resize", resize);
         }
       };
     }, [antialias, engineOptions, adaptToDeviceRatio, sceneOptions, onRender, onSceneReady]);

     return <canvas ref={reactCanvas} id={id} autoFocus style={{ width: "100vw", height: "100vh" }} />;
   };
   ```

2. **UI**

   Affichage de l'interface utilisateur pour un jeu de slalom en ski.

   ```javascript
   import React, { Fragment, useEffect, useState } from "react";
   import "./UI.css";
   import { SkiSlalomGame } from "../../games/ski-slalom/SkiSlalomGame";
   import ReactCountryFlag from "react-country-flag";

   export const UI = (props: { game: SkiSlalomGame }) => {
     const [currentTime, setCurrentTime] = useState(0);
     const [penalties, setPenalties] = useState(0);
     const [showTuto, setShowTuto] = useState(false);

     props.game.chrono.onTimeChanges = (time) => {
       setCurrentTime(time);
       setPenalties(props.game.chrono.penalties);
     };

     props.game.onGameFinished = (score: number) => {
       const yourScore = {
         playerName: "YOU",
         score: score,
         countryCode: "FR",
       };
       setLeaderboard([...leaderboard, yourScore]);
     };

     const initLeaderboard = [
       { playerName: "Ingemar Stenmark", score: 49.20, countryCode: "SE" },
     ];

     const [leaderboard, setLeaderboard] = useState(initLeaderboard);

     const firstPlayer = leaderboard.length > 0 ? leaderboard.sort((a, b) => a.score > b.score ? 1 : -1)[0] : undefined;

     return (
       <div className="ui-container">
         {props.game.finished ?
           <div>/* ... */</div> :
           props.game.gameStarted ?
           <Fragment>/* ... */</Fragment> :
           <Fragment>/* ... */</Fragment>
         }
       </div>
     );
   }
   ```

3. **Score**

   Affichage du score d'un joueur.

   ```javascript
   import React from 'react';
   import './Score.css';

   const Score = ({ name, score }) => {
     return (
       <div className="score-container">
         <div className="score-rectangle">
           <p className="score-name">{name}</p>
           <p className="score-value">{score}</p>
         </div>
       </div>
     );
   };

   export default Score;
   ```

4. **App**

   Point d'entrée principal de l'application.

   ```javascript
   import React from 'react';
   import './App.css';
   import { SkiSlalomGame } from '../../games/ski-slalom/SkiSlalomGame';
   import { UI } from '../UI/UI';
   import SceneComponent from "../SceneComponent";

   function App() {
     const game = new SkiSlalomGame();
     return (
       <>
         <UI game={game} />
         <SceneComponent
           adaptToDeviceRatio
           autoFocus
           antialias
           onSceneReady={game.onStart}
           onRender={game.onUpdate}
           id="game-canvas"
           width={window.innerWidth}
           height={window.innerHeight}
           engineOptions={{ deterministicLockstep: true, lockstepMaxSteps: 1 }}
         />
       </>
     );
   }

   export default App;
   ```

### Documentation du Jeu SkiSlalomGame

#### Description Générale



Le jeu consiste en un parcours de slalom où le joueur doit naviguer à travers des portes, éviter des obstacles et atteindre la ligne d'arrivée le plus rapidement possible.

#### Classes et Méthodes

1. **Player**

   Représente le joueur avec ses propriétés et comportements, y compris la gestion des mouvements et des collisions.

2. **Door**

   Représente une porte dans le jeu. Gère les interactions lorsque le joueur passe à travers la porte, y compris les sons et les pénalités.

3. **Ramp**

   Représente une rampe dans le jeu. Peut donner un boost ou un saut au joueur lorsqu'il la traverse.

4. **Boost**

   Représente un boost dans le jeu. Augmente la vitesse du joueur lorsqu'il le collecte.

5. **Terrain**

   Représente le terrain du jeu, généré dynamiquement avec des obstacles, des portes et des éléments de boost.

#### Fonctionnalités

- **Physique**: Utilisation du plugin Havok pour gérer la physique des objets et des collisions.
- **Particules**: Utilisation des systèmes de particules pour les effets de neige.
- **Sons**: Intégration des sons pour les interactions du joueur avec les portes, les boosts et les obstacles.
- **Terrain Dynamique**: Génération du terrain avec des éléments aléatoires pour augmenter la rejouabilité du jeu.