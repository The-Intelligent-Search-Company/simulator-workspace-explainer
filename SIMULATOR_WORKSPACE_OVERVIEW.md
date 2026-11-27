# Giving a video model a physical workspace

**The idea:** translate a video generator’s internal features into an executable 3D scene, compute its physical consequences, and return those consequences to guide generation.

[Interactive architecture and results](index.html) · [Reading list](REFERENCES.md)

**Pipeline:** observed video → generator features → learned scene adapter → physics simulation → future-state / interaction-graph tokens + object masks → guided generator → video.

The adapter uses observed geometry, motion and internal generator features to estimate objects and their physical state. The simulator computes trajectories and interactions; tokens and masks connect those consequences to video generation. Appearance still comes from the generator and original visual context.

## 1. Build once: the completed LPV16 pilot

Construct one scene, simulate its future, and reuse that prediction throughout denoising. Objects move within the simulated video, but the simulation is not revised between denoising steps.

**What it adds:** an executable physical interpretation of the generator’s input, without a person supplying a ready-made scene. Four adapters were trained with mask and geometry supervision (1,264 updates each), comparing global versus object-bound features and XYZ versus image-anchored position/depth. Generator and graph/state-token return weights stayed fixed; generation and scoring were separate from training.

**Results versus native generation, controller off:** on all 40 paired IRIS development windows, adapter C (global features + image/depth) achieves **31.4% lower perceptual error** (LPIPS 0.578 → 0.396), **47.2% higher structural similarity** (SSIM 0.324 → 0.477), and **34.3% lower pixel error** (RGB MAE 0.199 → 0.130). Percentages are changes in paired means relative to native, not percentage points or an overall quality score.

C is shown because it completed all 40 cases, not because it wins every metric. A/B/C/D completed 38/39/40/30 of 40 clips; 13 simulation failures occurred before generation, all in Hitting cones. The website also compares all four on their 30 shared successful cases. Total: 147 adapter clips + 40 native clips scored. [Aggregate results and formulas](results-lpv16.json).

These are reference-fidelity gains in four shared capture configurations, with one generation seed. They do not establish physical correctness, object permanence, causality or population-wide improvement. The reserved final test was not accessed.

## 2. Update at selected stages: the continual workspace

Read updated generator features at selected denoising stages, revise the scene while preserving object identity, simulate again, and feed the new future into subsequent generation. This could correct an initially mistaken scene estimate and make its changes inspectable.

**What it could add:** a workspace that evolves with the generator’s interpretation. Its value needs a matched comparison with a fixed workspace and a fine-tuned generator control, including runtime. No continual-workspace results are included here. The workspace is a decoded physical hypothesis, not a proven or complete record of the model’s reasoning.
