# Clone Complet — Messenger (https://messenger.abeto.co/)

Clone local statique complet du jeu WebGL 3D **Messenger** (*"It's a small planet, but someone's gotta make the deliveries"* par Abeto), capturé et vérifié avec **REA**.

## Architecture & Stack Technique

- **Moteur 3D** : Three.js r180 (WebGL2)
- **Rendu & Shaders** : 1 490 déclarations GLSL personnalisées, God Rays, Selective Bloom, Depth of Field, SMAA, Tone Mapping.
- **Multithreading (8 Web Workers)** :
  - `charactergeoworker` : Déformation et rigging dynamique des maillages de personnages.
  - `collisionworker` : Moteur de collision et physique sphérique sur la surface de la planète.
  - `dracoworker` : Décompression asynchrone des maillages 3D Google Draco.
  - `glyphworker` : Moteur de typographie vectorielle propulsé par WebAssembly (`glyph.wasm`).
  - `geometryworker` : Génération et batching procédural des morceaux de terrain (chunks LOD 0 à 3).
  - `exrworker` : Décodage asynchrone des cartes HDR d'environnement.
  - `msdfworker` : Rendu de texte multi-channel signed distance field.
  - `bitmapworker` : Décodage ImageBitmap off-thread.
- **Compression & Assets** :
  - Maillages polygonaux : Format Google Draco (`.drc`) pour la planète, les PNJs, les animations, les oiseaux et les colis.
  - Textures GPU : Format Basis Universal / KTX2 (`.ktx2`) décodé via `basis_transcoder.wasm`.
  - Moteur Audio spatial WebAudio : Ambiances de biomes (forêt, ville, plage, temple, usine, cascades), dialogues de PNJs, bruitages de pas, d'eau et de sauts.

## Structure du Répertoire

```
/root/cloned-messenger/
├── index.html                           # Point d'entrée HTML
├── README.md                            # Documentation d'ingénierie inverse
└── assets/
    ├── webgl-CS4l6lxD.js                # Point d'entrée bootstrap
    ├── App3D-DwM1eiaC.js                # Moteur Three.js r180, contrôleurs & quêtes
    ├── style-BgpnrCnL.css               # Styles de l'interface
    │
    ├── *worker-*.js                     # Les 8 Web Workers dédiés
    ├── libs/                            # Décodeurs WebAssembly (Draco, Basis, Glyph)
    ├── geometries/                      # Modèles 3D Draco (planètes, LODs, PNJs, colis)
    ├── images/                          # Textures KTX2, LUT, bruits de nuages/eau
    ├── audio/                           # Ambiances, dialogues, SFX WebAudio
    └── fonts/                           # Typographies vectorielles (.font)
```

## Démarrage du serveur local

Pour lancer le serveur de test :

```bash
python3 -m http.server 8081 --directory /root/cloned-messenger
```

Ouvrez ensuite `http://localhost:8081/` dans votre navigateur.

