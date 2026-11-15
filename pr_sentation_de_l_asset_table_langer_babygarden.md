# Présentation d'Asset & Pipeline MCP : La Station à Langer (*BabyGarden*)

## 1. Lien avec le Game Design Document (GDD)

Dans le cadre de notre jeu **BabyGarden**, l'environnement et le gameplay reposent sur le soin et la gestion quotidienne d'un espace dédié aux bébés. La **station à langer** (`SM_BabyChangingStation`) est un élément central du décor et des mécaniques de soin :

* **Rôle dans le jeu :** Point d'interaction principal pour les phases de soin (changement de couche, application de lotion, utilisation des serviettes).
* **Direction Artistique :** Style **Low-Poly stylisé** aux couleurs douces et pastel, pensé pour offrir une lecture visuelle claire, chaleureuse et rassurante tout en restant très léger en ressources (moins de 3 000 triangles).
* **Détails narratifs intégrés :** Pour rendre l'objet vivant dès le premier coup d'œil, le meuble intègre directement des accessoires du quotidien (piles de serviettes pliées, panier de rangement, gants de toilette et flacon de lotion).

---

## 2. Création et Modélisation sur Blender via MCP

Pour concevoir cet asset de manière rapide et structurée, j'ai piloté **Blender** directement via son serveur **MCP** à partir de Claude.

### Prompt utilisé pour la génération sur Blender

> **Phase 1 : Modeling & Texturing in Blender (`blender` MCP)**
> 1. **Scene Setup:** Clear the default Blender scene (delete the default cube, camera, and light). Ensure scene units are set to Metric (`1.0` scale) so dimensions match Unreal Engine (`1 Blender meter = 100 Unreal units`).
> 2. **Model Construction (Low-Poly Style):**
>    - Build a cohesive, stylized low-poly baby changing station centered at the world origin `(0, 0, 0)` with the base resting flat on `Z = 0` and the front facing `-Y`.
>    - **Dimensions:** Roughly `0.9m` wide, `0.55m` deep, and `0.95m` tall.
>    - **Key Details:** Main wooden dresser/cabinet frame with slightly beveled edges; two lower storage shelves containing simple low-poly props (folded towel stacks, a small basket, and a baby lotion bottle); top changing tray with raised safety guardrails on all four sides; soft, slightly rounded changing pad resting inside the top tray.
>    - Keep total triangle count under ~3,000 tris.
> 3. **Materials:** Assign clean Principled BSDF materials with soft pastel/warm stylized colors (`M_Wood_Light`, `M_Pad_Cream`, `M_Props_PastelBlue`, `M_Props_SoftYellow`).
> 4. **Cleanup & Export:** Apply all transforms (`Location`, `Rotation`, `Scale`), join parts into `SM_BabyChangingStation`, and export as FBX (`C:\Temp\SM_BabyChangingStation.fbx`) with `Apply Scalings: FBX All`, `Forward: -Y`, `Up: Z`, `Smoothing: Face`.

### Résultat obtenu dans Blender

Le serveur MCP a exécuté la construction procédurale et assemblé **37 pièces distinctes** en un seul maillage propre et optimisé :

* **Dimensions exactes :** $0,90\text{ m} \times 0,55\text{ m} \times 0,95\text{ m}$, avec le point de pivot placé au centre de la base ($Z = 0$).
* **Optimisation :** **1 468 triangles** (bien en dessous de la limite fixée à 3 000), ombrage plat (*Flat Shading*) avec biseaux légers et carte UV générée.
* **Matériaux appliqués :** 5 matériaux distincts (`M_Wood_Light`, `M_Pad_Cream`, `M_Props_PastelBlue`, `M_Props_SoftYellow` et `M_Props_SoftPink` ajouté automatiquement pour le flacon de lotion).

![Rendu de la station à langer dans Blender](image_69ae87.png)
*(Note : Pense à inclure ici également ta capture d'écran de la conversation/prompt dans Claude si nécessaire pour ton dossier).*

---

## 3. Intégration dans Unreal Engine 5 via MCP

Une fois le fichier `.fbx` généré, la seconde partie du pipeline a consisté à utiliser le serveur **Unreal MCP** (`uemcp`) pour préparer une scène vierge dans le projet **BabyGarden**, importer le modèle et le mettre en situation.

### Prompt utilisé pour l'intégration dans Unreal Engine

> **Phase 2: Scene Setup & Import in Unreal Engine (`unreal` MCP)**
> 1. **Create an Empty Level:** Create a new empty/basic level and ensure it has basic lighting (`Directional Light`, `Sky Atmosphere`, `Sky Light`, and a simple floor plane or ground mesh so the station doesn't float in void).
> 2. **Import Asset:** Import `SM_BabyChangingStation.fbx` into `/Game/Meshes/Props/` with Static Mesh settings enabled, auto-generating simple collision, and importing materials.
> 3. **Place & Frame in Scene:** Spawn `SM_BabyChangingStation` into the level at `(0, 0, 0)`. Verify its scale is `(1, 1, 1)` and that its base sits flush on the floor surface. Position the viewport camera so the changing station is clearly framed in view.

### Déroulement technique & Ajustements automatiques

Lors de l'exécution, l'agent MCP a automatiquement géré les contraintes spécifiques d'Unreal Engine :

1. **Gestion du chemin d'import :** L'importateur Unreal n'acceptant que les fichiers situés à l'intérieur de l'arborescence du projet, le FBX a été copié automatiquement dans `Saved/Imports/` du projet **BabyGarden** avant d'être importé dans `/Game/Meshes/Props/SM_BabyChangingStation` avec ses **5 matériaux**.
2. **Création du niveau de test :** Création et sauvegarde d'un nouveau niveau dédié (`/Game/Maps/LV_BabyChangingStation`) sans altérer la carte principale (`Lvl_FirstPerson`), équipé d'un éclairage complet (*Directional Light*, *Sky Atmosphere*, *Sky Light*) et d'un sol de $20 \times 20\text{ m}$.
3. **Placement et Collision :** L'acteur a été instancié exactement aux coordonnées $(0, 0, 0)$ avec une échelle de $(1, 1, 1)$ (les quatre pieds reposant parfaitement sur le sol) et une collision simple de type boîte englobante.

### Résultat final dans Unreal Engine

*(Insère ici la capture d'écran générée dans le dossier du projet : `Saved/Screenshots/BabyChangingStation_Final.png`, accompagnée de la capture du prompt d'exécution Unreal).*

![Intégration de la station à langer dans Unreal Engine](Saved/Screenshots/BabyChangingStation_Final.png)