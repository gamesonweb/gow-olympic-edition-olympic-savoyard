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

### Le Jeu de Slalom

#### Description Générale

Le jeu consiste en un parcours de slalom où le joueur doit naviguer à travers des portes, utiliser les boosts et atteindre la ligne d'arrivée le plus rapidement possible.

### Classes et Méthodes

#### 1. **Player**

La classe `Player` représente le joueur avec ses propriétés et comportements. Elle gère les mouvements et les collisions du joueur, ainsi que d'autres interactions dans le jeu.

**Propriétés Principales :**
- **position** : La position actuelle du joueur sur le terrain.
- **speed** : La vitesse actuelle du joueur.
- **direction** : La direction dans laquelle le joueur se déplace.
- **mesh** : Le maillage 3D représentant le joueur dans la scène Babylon.js.

**Méthodes Principales :**
- **move** : Met à jour la position du joueur en fonction de sa vitesse et de sa direction.
- **checkCollisions** : Vérifie les collisions avec les obstacles, les portes et les boosts.
- **applyBoost** : Augmente temporairement la vitesse du joueur lorsqu'il collecte un boost.
- **applyPenalty** : Applique une pénalité (réduction de vitesse ou augmentation du temps) lorsque le joueur rate une porte.

**Exemple de Code :**

```javascript
class Player {
  constructor(scene) {
    this.position = new BABYLON.Vector3(0, 0, 0);
    this.speed = 0.1;
    this.direction = new BABYLON.Vector3(1, 0, 0);
    this.mesh = this.createMesh(scene);
  }

  createMesh(scene) {
    const mesh = BABYLON.MeshBuilder.CreateBox("player", { height: 2, width: 1, depth: 1 }, scene);
    mesh.position = this.position;
    return mesh;
  }

  move() {
    this.position.addInPlace(this.direction.scale(this.speed));
    this.mesh.position = this.position;
  }

  checkCollisions(objects) {
    for (let obj of objects) {
      if (this.mesh.intersectsMesh(obj.mesh)) {
        if (obj instanceof Door) {
          this.applyPenalty();
        } else if (obj instanceof Boost) {
          this.applyBoost();
        }
      }
    }
  }

  applyBoost() {
    this.speed *= 1.5;
    setTimeout(() => {
      this.speed /= 1.5;
    }, 5000); // Boost lasts for 5 seconds
  }

  applyPenalty() {
    this.speed *= 0.75; // Reduce speed by 25%
  }
}
```

#### 2. **Door**

La classe `Door` représente une porte dans le jeu. Elle gère les interactions lorsque le joueur passe à travers la porte, y compris les sons et les pénalités.

**Propriétés Principales :**
- **position** : La position de la porte sur le terrain.
- **mesh** : Le maillage 3D représentant la porte dans la scène Babylon.js.
- **sound** : Le son joué lorsque le joueur passe à travers la porte.

**Méthodes Principales :**
- **playSound** : Joue un son lorsque le joueur passe à travers la porte.
- **applyPenalty** : Applique une pénalité au joueur s'il rate la porte.

**Exemple de Code :**

```javascript
class Door {
  constructor(scene, position) {
    this.position = position;
    this.mesh = this.createMesh(scene);
    this.sound = new BABYLON.Sound("doorSound", "path/to/doorSound.mp3", scene);
  }

  createMesh(scene) {
    const mesh = BABYLON.MeshBuilder.CreateBox("door", { height: 4, width: 0.5, depth: 3 }, scene);
    mesh.position = this.position;
    return mesh;
  }

  playSound() {
    this.sound.play();
  }

  applyPenalty(player) {
    player.applyPenalty();
  }
}
```

#### 3. **Ramp**

La classe `Ramp` représente une rampe dans le jeu. Elle peut donner un boost ou un saut au joueur lorsqu'il la traverse.

**Propriétés Principales :**
- **position** : La position de la rampe sur le terrain.
- **mesh** : Le maillage 3D représentant la rampe dans la scène Babylon.js.
- **boost** : Indique si la rampe donne un boost de vitesse ou un saut.

**Méthodes Principales :**
- **applyEffect** : Applique un effet (boost ou saut) au joueur lorsqu'il traverse la rampe.

**Exemple de Code :**

```javascript
class Ramp {
  constructor(scene, position, boost = true) {
    this.position = position;
    this.mesh = this.createMesh(scene);
    this.boost = boost;
  }

  createMesh(scene) {
    const mesh = BABYLON.MeshBuilder.CreateBox("ramp", { height: 1, width: 3, depth: 5 }, scene);
    mesh.position = this.position;
    return mesh;
  }

  applyEffect(player) {
    if (this.boost) {
      player.applyBoost();
    } else {
      player.direction.y += 0.1; // Simple jump effect
    }
  }
}
```

#### 4. **Boost**

La classe `Boost` représente un boost dans le jeu. Elle augmente la vitesse du joueur lorsqu'il le collecte.

**Propriétés Principales :**
- **position** : La position du boost sur le terrain.
- **mesh** : Le maillage 3D représentant le boost dans la scène Babylon.js.
- **sound** : Le son joué lorsque le joueur collecte le boost.

**Méthodes Principales :**
- **applyBoost** : Applique un boost de vitesse au joueur.
- **playSound** : Joue un son lorsque le joueur collecte le boost.

**Exemple de Code :**

```javascript
class Boost {
  constructor(scene, position) {
    this.position = position;
    this.mesh = this.createMesh(scene);
    this.sound = new BABYLON.Sound("boostSound", "path/to/boostSound.mp3", scene);
  }

  createMesh(scene) {
    const mesh = BABYLON.MeshBuilder.CreateSphere("boost", { diameter: 1 }, scene);
    mesh.position = this.position;
    return mesh;
  }

  applyBoost(player) {
    player.applyBoost();
    this.playSound();
  }

  playSound() {
    this.sound.play();
  }
}
```

#### 5. **Terrain**

La classe `Terrain` représente le terrain du jeu, généré dynamiquement avec des obstacles, des portes et des éléments de boost.

**Propriétés Principales :**
- **size** : La taille du terrain.
- **mesh** : Le maillage 3D représentant le terrain dans la scène Babylon.js.
- **obstacles** : Liste des obstacles présents sur le terrain.
- **doors** : Liste des portes présentes sur le terrain.
- **boosts** : Liste des boosts présents sur le terrain.

**Méthodes Principales :**
- **generateTerrain** : Génère dynamiquement le terrain avec des obstacles, des portes et des boosts.
- **addObstacle** : Ajoute un obstacle au terrain.
- **addDoor** : Ajoute une porte au terrain.
- **addBoost** : Ajoute un boost au terrain.

**Exemple de Code :**

```javascript
class Terrain {
  constructor(scene, size) {
    this.size = size;
    this.mesh = this.createMesh(scene);
    this.obstacles = [];
    this.doors = [];
    this.boosts = [];
    this.generateTerrain(scene);
  }

  createMesh(scene) {
    const mesh = BABYLON.MeshBuilder.CreateGround("terrain", { width: this.size, height: this.size }, scene);
    return mesh;
  }

  generateTerrain(scene) {
    for (let i = 0; i < 10; i++) {
      this.addObstacle(scene, new BABYLON.Vector3(Math.random() * this.size, 0, Math.random() * this.size));
      this.addDoor(scene, new BABYLON.Vector3(Math.random() * this.size, 0, Math.random() * this.size));
      this.addBoost(scene, new BABYLON.Vector3(Math.random() * this.size, 0, Math.random() * this.size));
    }
  }

  addObstacle(scene, position) {
    const obstacle = new BABYLON.MeshBuilder.CreateBox("obstacle", { height: 2, width: 1, depth: 1 }, scene);
    obstacle.position = position;
    this.obstacles.push(obstacle);
  }

  addDoor(scene, position) {
    const door = new Door(scene, position);
    this.doors.push(door);
  }

  addBoost(scene, position) {
    const boost = new Boost(scene, position);
    this.boosts.push(boost);
  }
}
```

### Ressources et Dépendances

#### Ressources

1. **FreeSound**
   - [FreeSound](https://freesound.org/) est une plateforme collaborative où les utilisateurs peuvent partager des sons libres de droits. Utilisé pour les effets sonores dans le jeu, comme les sons des portes et des boosts.

2. **SketchMap**
   - [SketchMap](https://sketchmap.io/) est un outil en ligne pour créer des cartes simples. Utilisé pour dessiner des croquis de la disposition du terrain et des obstacles dans le jeu.

#### Dépendances

- **Python Imaging Library (PIL)** : Utilisé pour la manipulation des images dans le script Python.
- **React** : Utilisé pour créer l'interface utilisateur de l'application.
- **Babylon.js** : Utilisé pour le rendu 3D dans l'application.
- **React Country Flag** : Utilisé pour afficher les drapeaux des pays dans l'interface utilisateur

.
- **TypeScript** : Utilisé pour le développement de la logique du jeu.

Ces classes et méthodes, ainsi que les ressources et les dépendances, constituent la structure fondamentale de notre jeu de slalom en 3D. La génération dynamique du terrain et l'utilisation de sons immersifs améliorent l'expérience de jeu, rendant chaque partie unique et captivante.

#### Fonctionnalités

- **Physique**: Utilisation du plugin Havok pour gérer la physique des objets et des collisions.
- **Particules**: Utilisation des systèmes de particules pour les effets de neige.
- **Sons**: Intégration des sons pour les interactions du joueur avec les portes, les boosts et les obstacles.
- **Terrain Dynamique**: Génération du terrain avec des éléments aléatoires pour augmenter la rejouabilité du jeu.
