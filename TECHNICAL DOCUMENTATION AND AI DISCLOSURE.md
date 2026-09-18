# BabyGarden - Technical Documentation

## Core Blueprint Architecture

### `BP_Baby`
* **Overview:** Controls the AI Pawn's environmental interactions, emotional state, and dynamic physics/movement states.
* **Happiness Mechanics:** 
  * Features a continuous happiness gauge.
  * Interacting with toys increases happiness.
  * Being grabbed and held by the player actively regenerates happiness.
  * If the happiness gauge drops to `0`, the baby enters a flying state.
* **AI Navigation & Physics:** 
  * Roams randomly within the world's `NavMeshBoundsVolume`.
  * Dynamically toggles between `CharacterMovementComponent` (for active AI navigation) and `SkeletalMesh` physics simulation (ragdoll state when grabbed or thrown).

### `BP_Toy`
* **Interface-Driven Lifecycle:** Uses Blueprint Interfaces to toggle states safely without hard-coupling to specific actor classes.
* **Deactivation:** Disables static mesh rendering, turns off physics simulation, and disables component ticking to optimize performance.
* **Activation:** Restores mesh visibility, enables physics simulation, and re-activates ticking upon being released or spawned.

### `BP_ToyBox`
* **Overlap Logic:** Detects collision overlap events with actors implementing the `BP_Toy` interface and automatically triggers their deactivation sequence.

### `BP_CharacterController`
* **Locomotion & Line Tracing:** Handles player input, camera rotation, and line tracing (`LineTraceForObjects`) to detect interactive world items.
* **Physics Handle Grab System:** 
  * Uses a `Physics Handle Component` to lock onto target components (`BP_Baby` or `BP_Toy`).
  * Smoothly updates target location and rotation relative to camera forward vectors per frame (`Event Tick`).
  * Suppresses unwanted rotational spinning while held.
  * Applies directional velocity impulses on release for throwing mechanics.

---

## AI Disclosure Statement

Generative AI (Gemini) was utilized as a technical collaborator, code debugger, and writing assistant throughout the development of **BabyGarden**. AI assistance was used in the following capacities:

### 1. Blueprint Architecture & Physics Mechanics
* Assisted in designing the grab/throw workflow for AI Pawns, specifically managing state transitions between stopping the `CharacterMovementComponent`, disabling AI, enabling ragdoll physics, and attaching skeletal meshes to a `Physics Handle`.
* Provided solutions for updating physics handle location and rotation on `Event Tick` to eliminate object jitter, clipping, and rotation instability while objects are held.

### 2. Build System & Packaging Debugging
* Diagnosed and resolved Unreal Engine 5.7 packaging errors (`ExitCode=6`, `UnauthorizedAccessException` on restricted administrative user accounts) by configuring custom `BuildConfiguration.xml` overrides to bypass UBA executor permission locks.
* Assisted in troubleshooting MSVC C++ toolchain link errors (`LNK2019` vectorization symbol mismatches).

### 3. Documentation & Technical Writing
* Authored, structured, and formatted this technical documentation.
* Refined raw developer notes into clear, standardized technical language and Markdown structure.


### Sources

For the roaming baby logic: https://www.youtube.com/watch?v=5DyFyqUegfA&t=188s


For the grab part:  https://www.youtube.com/watch?v=1-EuJWwyt_g&vl=fr
