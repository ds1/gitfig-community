# Design Tokens for Embodied AI: A Theoretical Framework for Physical World Primitives

*Source: originally published on the [design-token-context-model wiki](https://github.com/ds1/design-token-context-model/wiki/Design-Tokens-for-Embodied-AI:-A-Theoretical-Framework-for-Physical-World-Primitives) in January 2026. Moved here September 2026; this file is now the canonical copy.*

**The systematic abstraction principles that revolutionized 2D interface design may hold the key to representing physical knowledge in robotics and embodied AI—but the translation requires fundamentally new primitive categories grounded in physics, affordances, and cognitive science.**

The concept of "design tokens" has transformed how teams build digital products by providing systematic, composable primitives that separate semantic intent from implementation. A color token like `color-action-primary` abstracts away whether that means `#0066cc` on web or `UIColor.systemBlue` on iOS. This report investigates whether analogous primitives could systematize how embodied AI systems represent physical world knowledge—the materials, forces, spatial relationships, and affordances that robots must understand to interact with reality. The synthesis reveals that while no unified "embodied design token" system exists today, the foundations are scattered across robotics, physics simulation, cognitive science, and emerging AI world models, awaiting integration.

---

## The architecture of abstraction in 2D design systems

Modern design tokens follow a **three-tier hierarchy** that separates raw values from semantic meaning from component application. The W3C Design Tokens Community Group specification (published October 2025) formalizes this architecture: **primitive tokens** store raw values (`color-purple-500: #8b5cf6`), **semantic tokens** encode intent by referencing primitives (`color-text-primary: {color-purple-500}`), and **component tokens** apply semantics to specific contexts (`button-primary-background: {color-text-primary}`). This layered abstraction enables platform-agnostic specifications—the same JSON token file generates CSS custom properties, iOS Swift code, and Android Kotlin resources.

The canonical categories span **color**, **dimension** (spacing, sizing), **typography**, **motion** (duration + easing curves as cubic Bézier arrays), **shadow/elevation**, and **border**. Motion tokens are particularly instructive: they encode `duration` (milliseconds), `delay`, and `timingFunction` (a four-point Bézier curve), capturing the temporal dynamics of interface behavior in a platform-agnostic way. The key insight is that design tokens succeed because they identify the **minimal semantic units** needed to specify visual behavior while remaining **composable**—complex components emerge from simple token combinations.

Apple's visionOS spatial design guidelines offer the closest existing bridge to 3D. The platform defines three fundamental spatial containers—**Windows** (2D/3D content in resizable planes), **Volumes** (3D objects viewable from any angle), and **Full Spaces** (immersive environments)—plus spatial primitives for depth cues, comfort zones (viewing angle limits, neck strain avoidance), and spatial audio parameters (reverb, distance attenuation). However, these remain presentation-layer concepts; they describe how content appears in space, not how AI systems should represent physical knowledge.

---

## How AI world models currently encode physical understanding

The state of the art in AI world models reveals a fundamental tension between **implicit** and **explicit** physical knowledge representation. Video generation models like **Sora** and **Genie 3** demonstrate emergent 3D consistency, object permanence, and basic physics (gravity, momentum) without explicit physical modeling—these properties arise "purely as phenomena of scale" from training on internet video. Sora's architecture processes spacetime patches of video latent codes through a diffusion transformer, learning a unified representation for variable durations and resolutions. Yet this implicit knowledge fails on precise physical reasoning: Sora cannot reliably model glass shattering, fluid dynamics, or cause-and-effect chains like food consumption.

**Yann LeCun's JEPA architecture** (Joint Embedding Predictive Architecture) offers a theoretically grounded alternative. JEPA predicts in **abstract representation space rather than pixel space**, learning to capture essential structure while discarding irrelevant details. The architecture uses two encoders mapping observations to latent representations, plus a predictor module that forecasts future latents. This embodies the **information bottleneck principle**—compress observations into representations that preserve only what's predictable. Video-JEPA (V-JEPA) extends this to temporal prediction, masking spatiotemporal regions and learning to predict their latent representations. The key insight for embodied AI: JEPA learns representations of position, orientation, and velocity without requiring pixel-perfect reconstruction.

**Foundation models for robotics** take yet another approach. Google's **RT-2** treats actions as language tokens—robot commands become strings like "1 128 91 241 5 101 127 217" representing end-effector translation and rotation deltas. This enables transfer from web-scale vision-language pretraining to physical manipulation, but the representation remains opaque. **PaLM-E** injects continuous observations (images, robot states, 3D scene representations) directly into a large language model, achieving remarkable data efficiency (**10-80 examples** for tabletop tasks) through multimodal grounding. The emerging **OpenVLA** architecture (7B parameters) even exhibits mechanistic interpretability—probing reveals that specific neurons correspond to semantic directions like "fast" or "up," suggesting an emergent internal world model.

**3D Gaussian Splatting** represents the explicit end of the spectrum: scenes are encoded as millions of 3D Gaussian ellipsoids, each with explicit parameters for position (mean), shape (covariance matrix), view-dependent appearance (spherical harmonics coefficients), and opacity. This explicit parameterization enables real-time rendering (100+ fps) and straightforward editing—properties attractive for robotics applications requiring interpretable scene representations.

---

## Robotics representations reveal candidate primitives

How robots currently represent environments provides concrete examples of physical primitives. **Scene graphs** have emerged as the dominant semantic representation, organizing environments into hierarchical layers: metric-semantic meshes at the bottom, then objects with bounding boxes and semantic labels, navigable places, structural elements (walls, corridors), and rooms at the top. Nodes carry attributes (pose, geometry, semantic class, learned features), and edges encode spatial relations (inside, adjacent, above) and transforms. The MIT SPARK Lab's **3D Dynamic Scene Graphs** extend this to temporal dynamics, while **Action-Conditioned Scene Graphs** encode not just static relations but action-dependent changes—"opening fridge reveals apple."

**Affordance detection** operationalizes Gibson's ecological psychology for robotics. Current systems predict dense affordance maps—per-pixel probabilities for action classes like grasp, support, containment, or manipulation types (pushable, openable, turnable). The **VRB** framework learns affordances from human video by extracting contact points and post-contact trajectories from datasets like Ego4D, then mapping these back to pre-interaction frames. Affordances represent a critical bridge between perception and action—they encode not what objects *are* but what can be *done* with them.

**Motion primitives** provide the action-side analog. **Dynamic Movement Primitives (DMPs)** encode trajectories as nonlinear dynamical systems with forcing terms, parameterized by spring-damper constants, temporal scaling, goal positions, and learned basis function weights. **Probabilistic Movement Primitives (ProMPs)** extend this with uncertainty, representing trajectories as Gaussian distributions over basis function weights, enabling via-point conditioning and multi-trajectory blending. These primitives are **composable**—complex skills emerge from sequencing and blending simple primitives—and **adaptable**—the same primitive generalizes to different goals through parameter modification.

**Physics simulators** expose the lowest-level primitives. MuJoCo's parameter schema includes material **friction** (5D: slide, spin, roll_x, roll_y, torsion), contact **solref** (stiffness-damping reference), constraint **solimp** (impedance parameters), and body properties (mass, inertia tensor, density). NVIDIA IsaacSim adds articulation parameters (joint damping, friction, drive stiffness), collision approximation methods, and contact offsets. These parameters represent the **minimal sufficient specification** for physical behavior in simulation—change the friction coefficient and the same robot fails to grasp; modify contact stiffness and stable contacts become unstable.

---

## Neural physics and differentiable simulation expose learned primitives

**Graph Neural Networks for physics** reveal what primitives learned simulators discover. DeepMind's **Graph Network Simulator (GNS)** takes particle states as graph nodes with features including position, 5-step velocity history, one-hot material type encoding, and global forces. Edges connect spatially proximate particles. The encode-process-decode architecture runs 10-15 message-passing steps, outputting per-particle acceleration predictions. The node feature schema represents a learned primitive vocabulary:

```
node_features = {
    position: [x, y, z],
    velocity_history: [[v_t-4], ..., [v_t]],  # temporal context
    particle_type: one_hot(material),          # material identity
    global_features: [gravity, ...]            # external forces
}
```

**MeshGraphNets** add dual-space message passing—mesh-space edges approximate differential operators for internal dynamics, while world-space edges handle collisions. This architectural choice reflects a fundamental physical distinction between internal material response and external contact interactions.

**Differentiable simulators** bridge analytical and learned physics. DiffTaichi provides source-code transformation for automatic differentiation through physics, Brax compiles entire simulations in JAX for end-to-end gradient computation, and Nimble offers analytical gradients through Linear Complementarity Problem (LCP) contact solutions. The key insight: **gradients through physics** enable learning material parameters, control policies, and even novel physical behaviors from data. Contact gradients remain challenging—discontinuous events and varying contact normals produce incorrect gradients without specialized Time-of-Impact handling.

**Physics benchmarks** reveal what physical reasoning AI systems still lack. On **PHYRE** (2D physics puzzles), DQN agents achieve ~56% success within-template but only ~40% cross-template, far below humans. On **IntPhys 2** (2025), which tests violation-of-expectation for object permanence, immutability, continuity, and solidity using photorealistic videos, **GPT-4V and Gemini perform at chance level (~50%)** while humans achieve near-perfect accuracy. This reveals a critical gap: current foundation models lack robust intuitive physics despite strong performance on language and vision tasks.

---

## Cognitive science provides the theoretical foundation

Elizabeth Spelke's **core knowledge theory** identifies the innate physical primitives humans possess from infancy: **cohesion** (objects move as bounded wholes), **continuity** (objects trace continuous paths), **contact causation** (objects affect each other only through contact), **solidity** (objects cannot pass through each other), and **persistence** (objects continue existing when occluded). These core principles operate below conscious reasoning, providing the foundation for intuitive physics. Crucially, human physical reasoning is **object-centric**—physics operates on discrete entities and their interactions, not pixel-level dynamics.

Research on **mental simulation** suggests humans run approximate internal physics engines, trading accuracy for computational efficiency. People simulate only relevant parts of scenes, hit resource limits over long horizons, and produce probabilistic predictions by sampling from uncertain initial conditions. This "simulation as an engine of physical scene understanding" operates on object-level representations with approximate physical properties.

**Slot Attention** (2020) provides the AI architecture most aligned with cognitive principles. The mechanism produces K exchangeable "slots"—abstract representations that compete through attention to explain input features. Slots are **permutation equivariant** (any slot can bind to any object), enabling compositional scene decomposition without explicit supervision. Object-centric world models built on Slot Attention—like **FOCUS** (2025) with masked reconstruction for robotic manipulation—demonstrate improved sample efficiency precisely because they mirror the structure of human physical cognition.

**Embodied cognition theory** adds the action dimension. Cognition is fundamentally grounded in sensorimotor interaction; representations acquire meaning through bodily experience. Gibson's **affordances**—environmental features that specify action possibilities—are not objective properties of objects but relations between agent capabilities and environmental structure. A chair affords sitting for a human but not for an elephant. This agent-relative nature of physical knowledge has profound implications: embodied AI primitives cannot be purely objective descriptions of physics but must encode action possibilities relative to agent capabilities.

---

## A proposed taxonomy of design tokens for embodied AI

Synthesizing across these domains, a design token system for embodied AI would require fundamentally different primitive categories than 2D design systems. The following taxonomy proposes **six primitive domains**, each with hierarchical abstraction levels mirroring the primitive→semantic→component pattern:

### 1. Material tokens

Encoding physical substance properties:

| Level | Token Example | Value Structure |
|-------|--------------|-----------------|
| Primitive | `friction-coefficient` | `[slide: 0.5, spin: 0.01, roll: 0.001]` |
| Semantic | `material-rubber` | `{friction: {high}, restitution: {medium}, deformability: {high}}` |
| Component | `gripper-contact-surface` | `{material-rubber} + compliance: adaptive` |

Material tokens would encode **friction** (sliding, torsional, rolling), **restitution** (bounciness), **density**, **deformability** (elastic/plastic response), and **constitutive model** (Neo-Hookean, linear elastic, fluid). The semantic layer maps to intuitive categories ("rubber," "glass," "cloth") while preserving physical parameterization.

### 2. Affordance tokens

Encoding action possibilities:

| Level | Token Example | Value Structure |
|-------|--------------|-----------------|
| Primitive | `contact-point` | `[x, y, z, normal, friction_cone]` |
| Semantic | `grasp-affordance` | `{contact_points: [...], grip_type: {power|precision}, force_range: [min, max]}` |
| Component | `handle-manipulation` | `{grasp-affordance} + trajectory: {pull|turn|lift}` |

Affordance tokens must be **agent-relative**—the same object has different affordances for different embodiments. They encode what actions are possible (contact points, force requirements), what actions achieve (state changes), and what preconditions must hold.

### 3. Spatial relation tokens

Encoding geometric and topological relationships:

| Level | Token Example | Value Structure |
|-------|--------------|-----------------|
| Primitive | `relative-pose` | `SE(3): [translation, quaternion]` |
| Semantic | `containment` | `{container: obj_a, contained: obj_b, fill_ratio: 0.7}` |
| Component | `stable-placement` | `{support-relation} + contact_stability: {static_friction > required}` |

These parallel scene graph edge types but with explicit parameterization: **support** (what holds what), **containment** (what's inside what), **proximity** (distance metrics), and **occlusion** (visibility relations).

### 4. Dynamics tokens

Encoding motion and force properties:

| Level | Token Example | Value Structure |
|-------|--------------|-----------------|
| Primitive | `velocity-state` | `[linear: [vx, vy, vz], angular: [wx, wy, wz]]` |
| Semantic | `trajectory-primitive` | `{type: {reach|lift|place}, duration: τ, goal: SE(3), via_points: [...]}` |
| Component | `pour-motion` | `{trajectory-primitive:tilt} + flow_rate: adaptive + termination: {level_sensor}` |

Dynamics tokens encode both observed motion states and parameterized motion primitives (DMP/ProMP style), enabling specification of temporal behavior patterns.

### 5. Uncertainty tokens

Encoding epistemic and aleatoric uncertainty:

| Level | Token Example | Value Structure |
|-------|--------------|-----------------|
| Primitive | `pose-uncertainty` | `covariance: Σ ∈ R^{6×6}` |
| Semantic | `partial-observability` | `{occluded: [obj_ids], last_observed: t, predicted_state: μ, uncertainty: Σ}` |
| Component | `search-behavior` | `{uncertainty-threshold: σ_max} → trigger: {active-sensing}` |

Uncertainty is pervasive in embodied AI but absent from 2D design tokens. Physical world knowledge requires explicit representation of what is unknown, including sensor noise, occluded regions, and stochastic outcomes.

### 6. Agent capability tokens

Encoding embodiment constraints:

| Level | Token Example | Value Structure |
|-------|--------------|-----------------|
| Primitive | `workspace-limits` | `{reach: r_max, payload: m_max, precision: ε}` |
| Semantic | `manipulation-capability` | `{grasp_types: [power, precision], max_aperture: 0.15m, force_range: [0.1, 50]N}` |
| Component | `task-feasibility` | `match(task.requirements, agent.capabilities) → {feasible|infeasible|conditional}` |

Agent tokens enable the agent-relative nature of affordances: the same physical scene has different affordance landscapes for different embodiments.

---

## Architectural principles for embodied design tokens

Several design principles emerge from this synthesis:

**Object-centricity as the fundamental unit.** Following both cognitive science (core knowledge) and AI architectures (Slot Attention), tokens should organize around discrete objects rather than spatial locations or pixel regions. Each object carries material tokens, affordance tokens, and relation tokens to other objects.

**Separation of static and dynamic properties.** Object-centric world models like Dyn-O demonstrate the value of disentangling time-invariant properties (shape, material, intrinsic affordances) from time-varying states (position, velocity, current relations). This mirrors the semantic/component distinction in 2D tokens.

**Hierarchical composition.** Just as 2D tokens compose (primitive→semantic→component), embodied tokens should support hierarchical nesting: motion primitives compose into skills, skills into tasks, tasks into behaviors. Behavior trees provide one existing formalism for this composition.

**Platform-agnosticism through abstraction.** The power of design tokens lies in separating specification from implementation. Embodied tokens should specify physical requirements abstractly (`material: rubber-like`) while supporting platform-specific binding (`MuJoCo: friction=[0.8, 0.01, 0.001]`, `IsaacSim: static_friction=0.75, dynamic_friction=0.7`).

**Uncertainty as first-class citizen.** Unlike deterministic 2D rendering, physical interaction is inherently uncertain. Token specifications must include uncertainty bounds and propagation rules.

**Simulation-grounded semantics.** Token meanings should be grounded in simulatable physics—a material token is defined by its behavior under simulation, enabling verification, optimization, and transfer learning.

---

## The path forward and open challenges

Creating a practical design token system for embodied AI faces substantial challenges. **Representation grounding** remains unsolved: how do learned representations from foundation models map to explicit token schemas? OpenVLA's emergent interpretable neurons suggest this may be possible, but systematic methods don't exist. **Compositional generalization** is limited: current object-centric models struggle beyond training distributions, and "factorized bottlenecks do not automatically confer compositional extrapolation." **Real-world complexity** far exceeds simulation: deformable objects, fluids, contact-rich manipulation, and multi-material interactions remain challenging for both analytical and learned simulators.

The most promising near-term direction may be **hybrid systems** combining explicit token-based specifications with learned components. Explicit material tokens could parameterize domain randomization for sim-to-real transfer. Affordance tokens could structure the output space of learned perception models. Dynamics tokens could provide compositional primitives for hierarchical reinforcement learning. The W3C design token specification provides a template: a JSON-based format with explicit type schemas, composable references, and platform-specific extensions.

The deeper question is whether physical world knowledge admits the same systematization as visual design. 2D design tokens succeeded because interface design, despite its complexity, operates in a constrained space of colors, shapes, and animations. The physical world is unbounded, continuous, and governed by laws we can approximate but never perfectly specify. Perhaps "design tokens for embodied AI" will always be approximations—useful abstractions that capture common cases while deferring to learned models for the long tail. That would mirror human cognition: core physical knowledge provides quick, approximate intuitions, while deliberate simulation and learning handle novel situations.

---

## Conclusion

The translation of design tokens from 2D UI/UX to embodied AI requires not just new primitive categories but a fundamentally different relationship between specification and implementation. Where 2D tokens encode appearance, embodied tokens must encode **behavior under interaction**. Where 2D tokens are deterministic, embodied tokens must represent **uncertainty**. Where 2D tokens are platform-agnostic in rendering, embodied tokens must be **embodiment-relative** in affordances.

This report proposes six primitive domains—**material**, **affordance**, **spatial relation**, **dynamics**, **uncertainty**, and **agent capability**—as the foundation for such a system, drawing on physics simulation parameters, robotics representations, neural physics architectures, and cognitive science theories. The synthesis suggests that viable embodied design tokens would mirror the three-tier hierarchy of 2D systems (primitive→semantic→component) while incorporating object-centricity from cognitive science, compositional structure from scene graphs, and physical parameterization from simulation. The gap between current AI world models' implicit physical knowledge and explicit token-based specifications represents both the central challenge and the opportunity: bridging this gap would enable systematic, reusable, composable representations of physical world knowledge—the infrastructure embodied AI currently lacks.

---

## References

- W3C Design Tokens Community Group. (2025). Design Tokens Format Module.
- Brohan, A., et al. (2023). RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control.
- Driess, D., et al. (2023). PaLM-E: An Embodied Multimodal Language Model.
- Kim, M., et al. (2024). OpenVLA: An Open-Source Vision-Language-Action Model.
- Kerbl, B., et al. (2023). 3D Gaussian Splatting for Real-Time Radiance Field Rendering.
- Sanchez-Gonzalez, A., et al. (2020). Learning to Simulate Complex Physics with Graph Networks.
- LeCun, Y. (2022). A Path Towards Autonomous Machine Intelligence.
- Spelke, E. S., & Kinzler, K. D. (2007). Core Knowledge.
- Gibson, J. J. (1979). The Ecological Approach to Visual Perception.
- Locatello, F., et al. (2020). Object-Centric Learning with Slot Attention.
- Batra, D., et al. (2024). VRB: Affordances from Human Videos as a Versatile Representation for Robotics.
- Rosinol, A., et al. (2020). 3D Dynamic Scene Graphs: Actionable Spatial Perception with Places, Objects, and Humans.
