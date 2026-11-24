# Giving a video model a physical workspace

**The idea:** translate a video generator’s internal features into an executable 3D scene, compute its physical consequences, and return those consequences to guide generation. This connects learned visual interpretation to explicit physical computation without requiring a person to supply a ready-made simulation scene.

[Explore the illustrated research introduction](index.html) — inspect the architecture, select its components, and compare the two approaches interactively.

**The shared pipeline**

Observed video → generator features → learned scene adapter → physics simulation → object-state / interaction-graph tokens + masks → guided generator → video.

The adapter combines internal features with observed geometry and motion to estimate objects, shapes, poses, velocities and physical properties. The simulator predicts their trajectories and interactions. Tokens describe these consequences; masks associate guidance with visible objects. The same generator supplies appearance using the original visual context.

## 1. Build the simulation once

Construct one scene and simulate its future. Keep using that prediction while the video model gradually refines its output. The objects move within the simulation, but the simulation itself is not revised during generation.

**What it adds:** the model’s understanding becomes a starting point for explicit physical computation. This could support realistic input videos without requiring someone to supply a ready-made 3D scene. This is the approach currently being tested.

## 2. Update the simulation as generation develops

At selected stages, read the model’s updated understanding, revise the scene, simulate it again, and feed the new prediction into the next stage of generation. Preserve object identities and consistent physical rules across revisions.

**What it adds:** an evolving physical workspace, rather than a commitment to the first scene estimate. We could inspect how the model’s decoded hypotheses about objects and interactions change, and test whether updating them improves the final video. This extension is proposed, not yet demonstrated.

## Learning and evidence

The current study compares global versus persistent object-based scene adapters, each trained with scene/rollout supervision alone or with an added video-generation objective. That objective passes gradients through the simulation’s continuous parameters and the fixed conditioning pathway. Generator and return-path weights remain frozen. Future training video is a target, never input to the scene reader.

The first approach repeatedly uses the **same simulated future**; the second **recomputes it**. Judge both through generated motion, contacts, identity, shape, flicker and appearance—not simulation accuracy alone. Compare with controller-off generation; the continual extension also needs a matched fixed-workspace comparison and runtime accounting. The workspace exposes a decoded physical hypothesis, not a complete record of the model’s reasoning.

[References and competing approaches](REFERENCES.md) — an annotated reading path through the foundations, simulation-to-video methods, internal physical signals and closest alternatives.
