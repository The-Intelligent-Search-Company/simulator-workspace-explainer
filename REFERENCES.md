# References and competing approaches

A selective reading map for **Giving a video model a physical workspace**. Links and paper descriptions checked against primary sources on 24 November 2026. Years denote initial paper release; the linked versions may have later revisions. These are precedents and alternatives, not evidence that our proposed system outperforms them.

Start with **PhysGen**, **PHANTOM** and **Video-Coder** for the three closest architectural perspectives. Then read the internal-representation papers to understand the motivation for a generator-feature scene adapter.

## Objects and external workspaces

- **Battaglia et al. (2016), [Interaction Networks for Learning about Objects, Relations and Physics](https://arxiv.org/abs/1612.00222).** Learns physical predictions through object and relation graphs. A foundation for interaction representations; it is a learned dynamics model, not a video diffusion generator.
- **Hu et al. (2024), [Visual Sketchpad: Sketching as a Visual Chain of Thought for Multimodal Language Models](https://arxiv.org/abs/2406.09403).** Gives multimodal language models tools to draw and reason over visual artifacts. Conceptual precedent for an inspectable workspace, not evidence for a 3D reasoning workspace inside a video generator.

## Connecting motion and simulation to video

- **Zhang et al. (2024), [PhysDreamer: Physics-Based Interaction with 3D Objects via Video Generation](https://arxiv.org/abs/2404.13026).** Uses a video model's dynamics prior to infer interactive behavior for 3D objects. Generator-informed simulator parameters are already established precedent; our proposed contribution cannot simply be that a generator helps configure a simulator.
- **Niu et al. (2024), [MOFA-Video: Controllable Image Animation via Generative Motion Field Adaptions in Frozen Image-to-Video Diffusion Model](https://arxiv.org/abs/2405.20222).** Converts sparse controls into motion fields and guides generation with warped image features. Precedent for connecting motion to appearance, rather than for recovering a physical scene.
- **Liu et al. (2024), [PhysGen: Rigid-Body Physics-Grounded Image-to-Video Generation](https://arxiv.org/abs/2409.18964).** Combines image understanding, rigid-body simulation and diffusion refinement. A direct predecessor for the overall pipeline; our proposed distinction is the generator-feature scene adapter and its downstream training.
- **Chu et al. (2025), [Wan-Move: Motion-controllable Video Generation via Latent Trajectory Guidance](https://arxiv.org/abs/2512.08765).** Propagates first-frame features along dense point trajectories. Relevant motion-control precedent and a backbone for our implementation; controllable trajectories do not themselves determine physically correct motion.

## Physical information inside generators

- **Zhang et al. (2025), [VideoREPA: Learning Physics for Video Generation through Relational Alignment with Foundation Models](https://arxiv.org/abs/2505.23656).** Aligns generator token relations with a video-understanding model. A competing representation-based route to physical generation without explicit simulator execution.
- **Hassan et al. (2026), [Proprio: Latent Self-Scoring and Inference-Time Refinement for Physically Plausible Video Generation](https://arxiv.org/abs/2605.28230).** Uses a frozen generator's flow residuals for self-scoring, selection and refinement. Motivates making internal information actionable; our proposal instead decodes and executes a physical scene.
- **Esmati et al. (2026), [The Invisible Hand of Physics: When Video Diffusion Models Know More Than They Show](https://arxiv.org/abs/2606.05328).** Probes physical plausibility along inverted real-video trajectories. Reconstruction-path decoding motivates further study but does not demonstrate reliable scene inference during free generation.
- **Kneifl et al. (2026), [Does Physics Live in the Activations? Localizing Physical Quantities in Video Diffusion Models](https://arxiv.org/abs/2610.03154).** Finds physical quantities readable from object-associated activations, including early denoising stages, and explores steering. Supports testing the adapter's feature source; physical readouts alone do not establish full scene reconstruction.

## Closest architectural alternatives

- **Shen et al. (2026), [PHANTOM: Physics-Infused Video Generation via Joint Modeling of Visual and Latent Physical Dynamics](https://arxiv.org/abs/2604.08503).** Jointly models visual generation and physics-aware latent dynamics. Our proposed distinction is an explicit executable scene with inspectable states and physical rules, rather than a learned physical embedding alone.
- **Wang et al. (2026), [Video-Coder: Reinforcing Agentic Coding for Video Generation](https://arxiv.org/abs/2611.09894).** Trains an agent to write and execute dynamic 3D scene code that guides downstream generators. Our adapter instead starts from the target generator's own features and observed geometry. Any advantage in quality, training requirements or cost needs a matched comparison.

## How the proposal fits

The prospective contribution is a learned connection from generator features to executable physical computation and back to generation. The fixed version builds one scene and reuses its consequences. The proposed continual version revises that scene at selected denoising stages. These papers establish the neighboring components and approaches; they do not establish the superiority, novelty, faithful interpretability or efficiency of our combination. The current design comparison also does not substitute for matched external-feature scene builders, direct latent-conditioning baselines or a fixed-versus-continual comparison.
