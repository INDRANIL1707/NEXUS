# NEXUS — Frontier Problems to Conquer

## A Research Assessment of Photonic Interfaces, Non-Invasive Neural Interaction, Embodied Intelligence, and a Distributed Computing Operating Environment

**Version:** 1.0  
**Date:** 30 September 2026  
**Document type:** Research assessment / technical white paper  
**Scope:** NEXUS technical frontier, research gaps, measurable objectives, and falsifiable validation program

---

## Abstract

NEXUS is a proposed human–computing architecture in which wearable optics, multimodal human sensing, artificial intelligence, spatial/environmental perception, persistent context, and distributed computation operate as one closed-loop system. The central research problem is not the construction of a conventional smart-glasses product. It is the creation of a computational environment capable of estimating human intent and world state from incomplete observations, allocating computation across heterogeneous devices, and returning information or actions through an ultra-compact human interface.

Four coupled frontiers dominate the research program:

1. **Photonic interface:** generating, routing, coupling, and presenting light to the eye while simultaneously satisfying field of view, eyebox, optical efficiency, chromaticity, image quality, latency, thermal, power, and visual-comfort constraints.
2. **Non-invasive neural interaction:** extracting useful, low-latency intent information from weak and non-stationary EEG and related physiological signals without implants, excessive calibration, or laboratory-only conditions.
3. **Embodied intelligence:** maintaining a predictive representation of the user, environment, task, and available actions; reasoning under uncertainty; and closing the perception–prediction–action loop.
4. **NEXUS operating environment:** treating human context, device capabilities, AI agents, memory, tasks, and heterogeneous compute as first-class operating-system objects rather than treating the wearable as an isolated application endpoint.

The hardest NEXUS problem is not any individual component. It is the **integration of all four under realistic constraints**. A laboratory system can optimize one dimension at a time; an everyday NEXUS system must satisfy the constraints simultaneously. The resulting research objective is therefore a constrained closed-loop optimization problem rather than a single benchmark.

---

## 1. Problem Definition

The NEXUS system is modeled as a partially observable dynamical system.

Let the latent state at time \(t\) be

\[
S_t = \{W_t,U_t,T_t,D_t,C_t\},
\]

where:

- \(W_t\): physical world state,
- \(U_t\): user state,
- \(T_t\): active task and goal state,
- \(D_t\): connected-device state,
- \(C_t\): available computational resources.

NEXUS receives heterogeneous observations

\[
O_t = [V_t,A_t,EEG_t,EMG_t,EOG_t,G_t,IMU_t,D_t^{telemetry}],
\]

where:

- \(V_t\): visual observations,
- \(A_t\): audio,
- \(EEG_t\): electroencephalography,
- \(EMG_t\): electromyography,
- \(EOG_t\): electrooculography,
- \(G_t\): gaze estimate,
- \(IMU_t\): inertial sensing,
- \(D_t^{telemetry}\): device and resource telemetry.

The system constructs a belief state

\[
b_t(S)=P(S_t=S\mid O_{0:t},A_{0:t-1}),
\]

and chooses an action, task allocation, or interaction policy

\[
\pi_t = \Pi(b_t, M_t, G_t),
\]

where \(M_t\) is persistent memory and \(G_t\) is the current goal/intent hypothesis.

The physical/computational loop is therefore

\[
\boxed{
O_t \rightarrow b_t \rightarrow z_t \rightarrow \pi_t \rightarrow A_t \rightarrow O_{t+1}
}
\]

where \(z_t\) is an internal representation or world state estimate.

The scientific question is:

> **Can a compact, wearable and distributed system maintain sufficiently accurate estimates of human intent and physical context to support reliable real-time computing without requiring invasive neural interfaces or constant explicit interaction?**

---

# 2. Core Research Thesis

NEXUS should not be evaluated as a collection of features. The architecture should be evaluated as a **coupled information-and-control system**.

The core loop is:

\[
\boxed{
\text{Photons}
\rightarrow
\text{human sensing}
\rightarrow
\text{state estimation}
\rightarrow
\text{intelligence}
\rightarrow
\text{compute allocation}
\rightarrow
\text{rendering/action}
\rightarrow
\text{human}
}
\]

The central design principle is that no single modality is required to carry the complete burden of interpretation.

For neural interaction, the target is not unrestricted mind reading. The target is

\[
P(I_t \mid EEG,EMG,EOG,Gaze,Voice,Context,World)
\]

where \(I_t\) is an actionable interaction intent.

This formulation changes the research strategy. A weak neural signal can become useful when constrained by gaze, task state, environment, recent history, and available capabilities. Context becomes an information source rather than merely metadata.

---

# 3. Frontier Map

| Frontier | Primary unknown | Present state | NEXUS target | Hardest constraint |
|---|---|---|---|---|
| Optical engine | High-quality see-through/near-eye image in eyeglass form factor | Active engineering research | High-efficiency, large-eyebox, low-latency optical interface | Étendue/brightness/FOV/eyebox/power tradeoff |
| Neural acquisition | Stable high-SNR non-invasive signals during natural movement | Active research | Wearable multimodal neural/physiological sensing | Motion, electrode interface, artifacts |
| Intent decoding | Reliable semantic intent from weak signals | Active research | Low-calibration, adaptive intent channel | Inter-user and inter-session variability |
| Silent interaction | Covert speech/motor intent without overt speech | Early-to-mid research | Natural low-friction command channel | Low SNR and modality ambiguity |
| World model | Predictive representation of physical environment | Rapidly advancing | Persistent task-aware world model | Long-horizon prediction and uncertainty |
| Personal model | Stable user/context representation | Early fragmented capability | Continuously adapting user model | Privacy, drift, false inference |
| Agent system | Reliable perception–reasoning–action loop | Mature components, immature integration | Auditable closed-loop agent substrate | Hallucination, planning error, action risk |
| Compute fabric | Seamless heterogeneous execution | Mature distributed primitives exist | Human-facing compute reservoir | Scheduling, latency, bandwidth, privacy |
| NEXUS OS | New abstraction over humans/devices/agents/compute | Architecture-level concept | Executable environment with stable primitives | Cross-layer integration |
| Safety/trust | Correct action under uncertain inference | Partial solutions | Observe→infer→suggest→authorize→execute | False positive actions |
| All-day operation | Simultaneous sensing, compute, optics, networking | Not solved as an integrated system | Practical continuous operation | Power and thermal budget |

---

# 4. Frontier I — Optical Engine

## 4.1 Optical problem statement

The optical engine must transform an electronic image representation into controlled optical energy at the eye while maintaining useful see-through vision.

A simplified chain is:

```text
Digital image
   ↓
Light source / microdisplay
   ↓
Collimation and beam conditioning
   ↓
In-coupling
   ↓
Waveguide / optical transport
   ↓
Pupil expansion
   ↓
Out-coupling
   ↓
Eye pupil
   ↓
Retina
```

The real environment simultaneously contributes optical energy:

```text
World light → transparent/partially reflective optical path → eye
```

The problem is therefore a constrained photon-routing problem, not merely a display-resolution problem.

---

## 4.2 Fundamental constraints

Important variables include:

- field of view (FOV),
- exit pupil and eyebox,
- eye relief,
- optical efficiency,
- luminance/brightness,
- modulation transfer function (MTF),
- geometric and chromatic distortion,
- color uniformity,
- latency,
- focal depth,
- transmission of the real-world scene,
- thermal load,
- mass and volume.

An increase in one variable can degrade another. Current diffractive waveguide research explicitly reports tradeoffs among coupling efficiency, FOV, eyebox continuity and uniformity; proposed architectures can improve particular points in this trade space but do not remove the general systems problem. [1][2][3]

---

## 4.3 Étendue

A fundamental optical constraint is étendue:

\[
G=n^2A\Omega,
\]

where \(A\) is area, \(\Omega\) is accepted solid angle, and \(n\) is refractive index.

Passive optical systems cannot arbitrarily compress étendue. This provides a physical reason that the simultaneous requirements

\[
\text{tiny} + \text{bright} + \text{wide FOV} + \text{large eyebox}
\]

are difficult.

This is one of the primary physics-level bottlenecks for NEXUS.

---

## 4.4 Waveguide physics

For total internal reflection, a waveguide with refractive index \(n_w\) surrounded by a lower-index medium \(n_o\) supports confinement when

\[
\theta_i > \theta_c,
\qquad
\theta_c=\sin^{-1}\left(\frac{n_o}{n_w}\right).
\]

Diffractive couplers can alter the propagation direction through grating momentum:

\[
\mathbf{k}_{out}=\mathbf{k}_{in}+m\mathbf{K},
\]

where \(\mathbf K\) is the grating vector.

Pupil expansion replicates the optical pupil so that eye position can vary without immediate loss of the virtual image.

A critical engineering reality is that adding more coupling structures can enlarge eyebox or manipulate the ray distribution while also increasing losses and non-uniformities. Experimental and numerical demonstrations continue to explore methods for reducing this tradeoff. [2][3]

---

## 4.5 Full-color problem

Diffractive structures are wavelength dependent:

\[
\theta = \theta(\lambda).
\]

Therefore red, green and blue components do not naturally propagate identically.

The resulting problems include:

- chromatic dispersion,
- color-dependent pupil placement,
- wavelength-dependent efficiency,
- color non-uniformity.

Achromatic metasurface and metagrating waveguides are active research directions. A 2025 *Light: Science & Applications* study demonstrated an achromatic metasurface waveguide concept, while a 2025 *Nature Nanotechnology* report demonstrated a single-layer achromatic metagrating waveguide concept aimed at reducing multilayer complexity. These are research demonstrations, not evidence that the full NEXUS target has been solved. [4][5]

---

## 4.6 Accommodation–vergence problem

The human visual system normally coordinates ocular convergence and accommodation. Conventional near-eye displays can present disparity indicating one depth while the optical focus remains at another depth:

\[
D_{vergence}\neq D_{accommodation}.
\]

This mismatch is known as the vergence–accommodation conflict (VAC) and can contribute to visual discomfort.

Candidate solutions include:

- fixed focal plane,
- multifocal displays,
- varifocal displays,
- light-field displays,
- holographic/wavefront control,
- Maxwellian-type architectures.

Varifocal and wavefront-modulation research remains active; for example, a 2025 study demonstrated compact polarization-controlled multi-depth switching across a range from roughly 24 cm to infinity in a laboratory optical architecture. [6]

The NEXUS research problem is not to select a technology by name. It is to determine the minimum retinal optical information required for comfortable interaction and engineer the smallest architecture that provides it.

---

## 4.7 Laser/retinal projection frontier

Retinal or laser-based projection is potentially attractive for compact optical engines because the image can be created by controlling rays or scanned light rather than by placing a conventional panel at the focal plane.

It is also intrinsically safety-critical.

The eye can focus collimated optical energy onto a small retinal region, so nominal visual brightness is not a sufficient proxy for ocular safety. FDA guidance explicitly warns that laser brightness does not directly indicate relative eye hazard, and retinal-image laser display devices are recognized as a distinct regulatory product category. [7][8]

Therefore a NEXUS laser engine would require, from the beginning:

\[
\boxed{
\text{optics} + \text{power limiting} + \text{fault detection} + \text{beam control} + \text{redundant shutdown} + \text{safety verification}
}
\]

Safety cannot be treated as a post-processing stage.

---

## 4.8 Optical research objectives

### Objective O1 — End-to-end optical budget

Create a ray/wave optical model that reports:

\[
\{FOV, eyebox, efficiency, luminance, MTF, distortion, chromaticity, eye\ relief\}
\]

for every proposed architecture.

### Objective O2 — Coupling optimization

Optimize in-coupler/out-coupler geometry or metasurface parameters subject to:

\[
\eta_{opt} \geq \eta_{target}
\]

and specified uniformity/FOV constraints.

### Objective O3 — Dynamic focus

Evaluate whether a fixed, varifocal, multifocal, light-field, or wavefront architecture provides the required visual comfort and system complexity tradeoff.

### Objective O4 — Safety envelope

Define a hard optical-radiation envelope and a failure-mode analysis before high-power optical hardware is considered.

---

# 5. Frontier II — Non-Invasive BCI

## 5.1 The actual problem

The NEXUS BCI objective is frequently misunderstood as arbitrary thought decoding.

That is not the required target.

The useful NEXUS problem is:

\[
\boxed{
\text{infer actionable intent from non-invasive neural and physiological signals}
}
\]

The desired output is a probabilistic hypothesis:

\[
P(I_t\mid X_{0:t},C_t,W_t),
\]

not an assumption that raw EEG directly equals semantic intent.

---

## 5.2 Why non-invasive neural decoding is difficult

A simplified EEG measurement model is

\[
x(t)=LJ(t)+n(t),
\]

where:

- \(J(t)\) represents distributed neural sources,
- \(L\) is a volume-conduction/lead-field operator,
- \(x(t)\) is the scalp measurement,
- \(n(t)\) includes noise and artifacts.

The inverse mapping from scalp observations to precise neural sources is underdetermined.

Practical wearable measurements also contain:

\[
x = s_{brain}+s_{eye}+s_{muscle}+s_{motion}+s_{environment}+n.
\]

The objective is not simply to fit a stronger classifier. It is to improve the information available to the decoder.

Recent reviews continue to identify signal quality, movement artifacts, electrode interfaces, generalization and hardware–algorithm co-design as major requirements for practical dry-electrode EEG systems. [9][10][11]

---

## 5.3 Current scientific boundary

A 2025 *Nature Communications* study investigated decoding individual words from non-invasive EEG and MEG across seven public datasets and two newly collected datasets, totaling 723 participants, three languages and approximately five million words. The study showed meaningful progress but also demonstrated strong dependence on recording modality, task protocol, data volume and averaging. The reported results are therefore evidence of progress toward non-invasive language decoding, not evidence of an everyday unrestricted thought-to-text interface. [12]

The same work reported substantially better decoding with MEG than EEG and better performance in reading than listening conditions, illustrating how strongly measurement physics and experimental design constrain decoding. [12]

A separate 2025 *Nature Communications* study demonstrated real-time EEG-based robotic hand control at individual-finger level, illustrating that useful fine-grained control can be obtained non-invasively under controlled conditions. [13]

These results establish the frontier more accurately than the phrase "mind reading": non-invasive systems can decode meaningful information, but **robust naturalistic high-bandwidth decoding remains unresolved**.

---

# 6. BCI Should Be Multimodal

NEXUS should not force EEG to carry the full interaction channel.

The intended observation vector is

\[
X_t=[EEG_t,EMG_t,EOG_t,Gaze_t,Voice_t,Vision_t,IMU_t,Context_t].
\]

The fusion function becomes

\[
Z_t=F(X_t),
\]

followed by

\[
\hat I_t=D(Z_t).
\]

This changes the information problem.

For example:

- gaze identifies the target,
- vision identifies the object,
- context identifies the task,
- voice supplies semantic content when available,
- EMG can provide motor or silent-speech-related information,
- EOG provides eye-movement information,
- EEG contributes cortical information.

The system therefore estimates a **joint intent posterior** rather than demanding that EEG alone reconstruct a complete command.

---

## 6.1 EMG is not EEG

EMG measures electrical activity associated with muscle activation. It is not a direct measurement of cortical activity.

For NEXUS, this distinction is useful rather than limiting.

Subtle facial and speech-related muscle activity can provide an additional channel for low-friction or silent interaction. Research in silent-speech interfaces increasingly explores multimodal and on-body sensing because peripheral signals can be easier to obtain robustly than purely cortical signals in some use cases. [14]

The NEXUS architecture should therefore represent

\[
EEG \neq EMG \neq EOG,
\]

while allowing all three to contribute to the same intent model.

---

## 6.2 Calibration frontier

Conventional BCI systems frequently depend on subject-specific calibration.

For everyday NEXUS operation the desired condition is approximately

\[
T_{calibration}\rightarrow 0
\]

while preserving

\[
Accuracy\uparrow,
\quad
Robustness\uparrow.
\]

This is difficult because

\[
P_t(X\mid I)\neq P_{t+\Delta}(X\mid I)
\]

can occur as electrode position, physiology, fatigue, attention and movement change.

The appropriate research target is therefore a **foundation decoder plus personalized adaptive state**:

\[
D_{NEXUS}=D_{foundation}+D_{personalized}.
\]

Adaptation must balance plasticity and stability to avoid decoder drift.

---

# 7. The NEXUS Neural Hypothesis Pipeline

The neural subsystem should not issue actions directly.

```text
Electrodes / sensors
        ↓
Analog front end
        ↓
ADC
        ↓
Signal-quality estimation
        ↓
Artifact estimation / suppression
        ↓
Neural + physiological representation
        ↓
Decoder
        ↓
NeuralHypothesis
        ↓
Multimodal fusion
        ↓
Intent posterior
        ↓
Policy / authorization
        ↓
Action
```

The key interface object is:

\[
\boxed{NeuralHypothesis}
\]

rather than an unconditional `NeuralCommand`.

Example:

```json
{
  "hypothesis": "select",
  "confidence": 0.72,
  "target": "laptop",
  "source_modalities": ["EEG", "EOG", "gaze", "context"],
  "timestamp": 1727700000000
}
```

The runtime then decides whether that hypothesis is sufficiently reliable and sufficiently authorized for action.

---

# 8. BCI Frontier Objectives

## Objective B1 — Signal acquisition

Design and characterize a wearable electrophysiology front end compatible with the physical constraints of eyewear.

Measurements must include:

- channel impedance/contact stability,
- input-referred noise,
- common-mode rejection,
- bandwidth,
- sampling rate,
- motion sensitivity,
- long-duration comfort.

## Objective B2 — Artifact model

Construct a synchronized reference dataset containing EEG, EOG, EMG, IMU and video so that artifacts can be labeled rather than treated as unexplained noise.

## Objective B3 — Intent hierarchy

Test progressively harder tasks:

\[
\text{binary intent}
\rightarrow
\text{small command vocabulary}
\rightarrow
\text{continuous control}
\rightarrow
\text{silent speech}
\rightarrow
\text{open-vocabulary language}.
\]

## Objective B4 — Cross-session adaptation

Measure accuracy and false activation rate across separate recording days without complete recalibration.

## Objective B5 — Naturalistic validation

Require evaluation during walking, speaking, normal eye movement and ordinary head motion rather than only static laboratory sessions.

---

# 9. Frontier III — Intelligence and World Models

## 9.1 The intelligence problem

NEXUS requires more than a conversational language model.

A useful model must maintain relationships between:

\[
\text{user} + \text{world} + \text{task} + \text{time} + \text{available actions}.
\]

A compact formalization is

\[
z_t=f_\theta(O_{\leq t},A_{<t}),
\]

with predictive dynamics

\[
\hat z_{t+1}=g_\phi(z_t,a_t).
\]

The central property is not pixel reconstruction. It is whether the representation supports prediction, reasoning, and action.

---

## 9.2 World model layers

A NEXUS world model should contain at least:

### Metric/spatial state

\[
(x,y,z,R,v,a)
\]

for position, orientation, velocity and acceleration where appropriate.

### Semantic state

\[
object \rightarrow identity,category,properties,affordances.
\]

### Temporal state

\[
E_t=\{e_0,e_1,\ldots,e_t\}.
\]

### Dynamics

\[
P(S_{t+1}\mid S_t,A_t).
\]

### Uncertainty

\[
P(S_t\mid O_{0:t},A_{0:t-1}).
\]

Recent V-JEPA 2 work demonstrates the relevance of self-supervised predictive representations for visual understanding, anticipation, physical prediction and planning, including robot control experiments. It is an important research direction for NEXUS, but it is not equivalent to a complete general-purpose world model. [15][16]

---

## 9.3 Active perception

NEXUS should not necessarily process every sensor stream at maximum resolution all the time.

An intelligent system can decide which observation is worth acquiring:

\[
a_t^{sense}=\arg\max_a \mathbb E[\Delta Information(a)]
\]

subject to power and latency limits.

Examples include:

- increasing camera resolution only when object identity is uncertain,
- increasing audio processing only when speech is likely,
- consulting higher-cost compute when local confidence is low.

This connects intelligence directly to power and compute optimization.

---

## 9.4 User model

The system requires an evolving user state:

\[
U_t=f(U_{t-1},O_t,A_t,F_t)
\]

where \(F_t\) represents feedback.

The user model should represent:

- current task,
- current interaction state,
- persistent preferences,
- working context,
- historical events,
- inferred attention,
- uncertainty.

It must not be treated as a claim that the system has direct access to private mental states.

Inference remains probabilistic.

---

# 10. Memory Architecture

A practical NEXUS memory system should separate multiple timescales.

| Memory | Function | Typical timescale |
|---|---|---|
| Working | current interaction/task | ms–minutes |
| Episodic | events and experiences | hours–years |
| Semantic | facts and relationships | long-term |
| Procedural | how tasks are performed | long-term |
| Spatial | places, objects, topology | long-term |
| User model | preferences and stable interaction statistics | adaptive |

The requirement is not "store everything". The requirement is to maintain a compact state that is useful for prediction and action.

---

# 11. Reasoning and Planning

Given latent state \(z_t\), memory \(M_t\), and goal \(G_t\), a planner computes a policy

\[
\pi^*=\arg\max_\pi \mathbb E[U(S_{t:t+H},\pi)]
\]

subject to:

\[
\text{permissions},\quad
\text{latency},\quad
\text{power},\quad
\text{compute},\quad
\text{risk}.
\]

Long-horizon planning is a frontier because the system must avoid chaining small uncertainties into large failures.

A reliable NEXUS agent therefore requires uncertainty estimates and explicit execution boundaries.

---

# 12. Frontier IV — NEXUS Operating Environment

## 12.1 Why a new OS abstraction is required

Traditional operating systems primarily manage:

\[
CPU + Memory + Files + Processes + Devices.
\]

NEXUS must additionally manage:

\[
Human + Context + Intent + Capability + Agent + Model + World + Compute.
\]

The NEXUS layer should therefore sit above existing kernels/OS stacks during the initial development phase.

```text
Hardware
   ↓
Linux / Android / Windows / firmware
   ↓
NEXUS Runtime
   ↓
NEXUS Environment
   ↓
Intelligence / agents / applications
```

A new kernel is not required to prove the fundamental NEXUS abstraction.

---

## 12.2 Canonical NEXUS objects

The runtime should treat the following as first-class objects:

```text
Observation
World
Context
Memory
Intent
Capability
Action
Policy
Compute
Trust
Device
```

A minimal type system could look conceptually like:

\[
Observation \rightarrow Context \rightarrow Intent \rightarrow Action.
\]

Capabilities connect the intent/action system to physical and computational resources.

---

# 13. Control Plane and Data Plane

The NEXUS architecture should separate low-volume coordination from high-bandwidth data movement.

## Control plane

Suitable for:

- device registration,
- capability discovery,
- intent messages,
- status,
- permissions,
- task scheduling,
- synchronization metadata.

Candidate mechanisms include Binder/AIDL on Android and compact local RPC/IPC mechanisms on Linux/desktop environments.

## Data plane

Suitable for:

- camera frames,
- sensor buffers,
- audio streams,
- tensors,
- GPU resources,
- rendered frames.

The data plane should prefer shared memory, DMA-capable buffers, AHardwareBuffer/Vulkan-compatible mechanisms, or equivalent high-throughput transports rather than serializing every high-bandwidth object into JSON.

This separation is necessary because an ambient visual system can easily generate data rates far beyond what a control-oriented RPC format should carry.

---

# 14. Compute Reservoir

The NEXUS compute fabric is modeled as

\[
C=\{C_1,C_2,\ldots,C_n\}.
\]

Each node exposes properties such as:

\[
C_i=\{compute,memory,bandwidth,latency,power,privacy,availability\}.
\]

A task has requirements

\[
R_j=\{compute,memory,latency,bandwidth,privacy\}.
\]

The scheduler chooses an execution placement satisfying the constraints.

A general objective can be expressed as

\[
J=\alpha L+\beta E+\gamma C_{cost}+\delta B+\epsilon Risk,
\]

where:

- \(L\): latency,
- \(E\): energy use,
- \(C_{cost}\): computational cost,
- \(B\): bandwidth use,
- \(Risk\): privacy/security/operational risk.

Then

\[
resource^*=\arg\min J.
\]

The system should support a hierarchy:

```text
Glasses
  ↓
Phone
  ↓
Nearby laptop/workstation
  ↓
Edge compute
  ↓
Cloud
```

The hierarchy represents one logical NEXUS environment, not five unrelated applications.

---

# 15. Task Graphs and Distributed AI

Complex user requests should be decomposed into directed acyclic graphs:

\[
G=(V,E).
\]

Each node in \(V\) is a computational task and each edge in \(E\) represents a dependency.

Example:

```text
Object identification
        ↓
Memory retrieval
        ↓
External knowledge retrieval
        ↓
Comparison
        ↓
Reasoning
        ↓
Presentation
```

Different graph nodes can be scheduled on different devices.

This produces a useful research intersection between distributed systems and agentic AI: the operating environment becomes a scheduler for **semantic workloads**, not just CPU processes.

---

# 16. Model Routing

NEXUS should maintain a model set

\[
\mathcal M=\{M_1,M_2,\ldots,M_n\}
\]

rather than using one giant model for every task.

A router can select

\[
M^*=R(task,context,latency,power,privacy,compute).
\]

Examples:

- tiny local model for wake/intent detection,
- local vision model for fast scene interpretation,
- nearby GPU model for expensive multimodal reasoning,
- remote model for rare high-cost tasks.

The system objective is not maximum model size. It is maximum **useful intelligence per unit of energy, latency, bandwidth and risk**.

---

# 17. Trust and Action Policy

The system must distinguish:

\[
\boxed{confidence \neq authorization}
\]

A high-confidence prediction may still be disallowed from executing a consequential action.

The core sequence is:

\[
\boxed{
Observe
\rightarrow
Infer
\rightarrow
Suggest
\rightarrow
Authorize
\rightarrow
Execute
}
\]

A capability can be represented as

\[
Cap=(agent,resource,operation,scope,expiry).
\]

Example:

```text
Agent: Calendar
Resource: Calendar
Operation: Read
Scope: Next 7 days
Expiry: 30 minutes
```

This is substantially more expressive than a single on/off permission bit.

---

# 18. The Deep Integration Problem

The individual frontiers are already difficult. Their coupling creates a harder problem.

Consider a single interaction:

1. The user looks at an object.
2. Cameras identify it.
3. Eye tracking identifies attention.
4. EEG/EMG/EOG contribute an interaction hypothesis.
5. Context determines the relevant task.
6. The world model predicts possible actions.
7. The agent chooses an action.
8. The runtime schedules compute.
9. The compute resource returns a result.
10. The optical engine renders information.
11. The user responds.
12. The new physiological/environmental state updates the model.

The loop is:

\[
O_t
\rightarrow
\hat S_t
\rightarrow
\hat I_t
\rightarrow
\pi_t
\rightarrow
A_t
\rightarrow
O_{t+1}.
\]

A failure at any point can propagate.

For example:

\[
\text{gaze error}
\rightarrow
\text{wrong target}
\rightarrow
\text{wrong intent interpretation}
\rightarrow
\text{wrong tool}
\rightarrow
\text{wrong action}.
\]

Therefore end-to-end validation is more important than isolated component benchmarks.

---

# 19. The Most Important BCI Insight for NEXUS

The primary neural-interface research question should not be:

> Can EEG decode arbitrary thoughts?

The better question is:

> **How much neural information is actually necessary when the machine already knows the user, environment, gaze, task, device state, and recent history?**

Formally, compare

\[
P(I\mid EEG)
\]

with

\[
P(I\mid EEG,EMG,EOG,Gaze,Context,World).
\]

The difference quantifies the value of the surrounding NEXUS architecture.

If the multimodal posterior becomes sufficiently concentrated with much less neural information, NEXUS has converted the BCI problem from an unrestricted decoding problem into a constrained intent-estimation problem.

This is one of the central hypotheses that should be tested experimentally.

---

# 20. Research Program: Minimal-to-Maximal Ladder

A realistic research sequence should preserve the architecture while progressively increasing difficulty.

## Stage 0 — Pure software simulation

Inputs:

- synthetic EEG/EMG/EOG,
- simulated gaze,
- simulated camera events,
- virtual device state.

Outputs:

- intent posterior,
- world-state updates,
- task scheduling,
- rendered NEXUS UI.

This proves the architecture without requiring hardware.

---

## Stage 1 — Real multimodal sensing

Use ordinary accessible sensors to establish:

\[
Gaze + EOG + IMU + EMG + audio + vision.
\]

Evaluate intent inference without claiming cortical decoding.

---

## Stage 2 — Wearable EEG

Introduce EEG into the same synchronized stream.

Measure the incremental information gain:

\[
\Delta I_{EEG}
=I(Intent;All\ Modalities)-I(Intent;Without\ EEG).
\]

This is a stronger scientific measurement than reporting EEG classification accuracy alone.

---

## Stage 3 — Adaptive decoder

Introduce:

- subject adaptation,
- cross-day adaptation,
- uncertainty calibration,
- drift detection.

---

## Stage 4 — Naturalistic operation

Evaluate during:

- walking,
- ordinary head motion,
- conversation,
- environmental distractions,
- changing lighting,
- changing electrode contact.

---

## Stage 5 — Integrated optical interface

Connect the intelligence/runtime stack to a physical near-eye display prototype.

---

## Stage 6 — Closed-loop NEXUS prototype

The final research prototype should perform:

\[
\boxed{
\text{sense}
\rightarrow
\text{infer}
\rightarrow
\text{reason}
\rightarrow
\text{schedule}
\rightarrow
\text{render}
\rightarrow
\text{measure feedback}
}
\]

continuously.

---

# 21. Falsifiable Research Tests

The project should define explicit failure criteria.

## Test T1 — Neural information gain

Compare a multimodal model with and without EEG.

Success condition:

\[
\Delta I_{EEG}>0
\]

with statistically significant improvement under a predefined protocol.

Failure condition:

EEG provides no reproducible benefit beyond gaze/EMG/EOG/context.

---

## Test T2 — Cross-day stability

Train on day 1. Evaluate on days 2–7 with minimal adaptation.

Measure:

- accuracy,
- calibration,
- false activation,
- latency.

A strong system must preserve performance over time rather than only inside one session.

---

## Test T3 — Natural-motion robustness

Compare static and natural-motion performance.

Define degradation:

\[
\Delta A=A_{static}-A_{natural}.
\]

The objective is to minimize \(\Delta A\).

---

## Test T4 — Calibration efficiency

Measure accuracy as a function of calibration time:

\[
A(T_{cal}).
\]

The desired curve should show useful performance with short calibration and continued adaptation thereafter.

---

## Test T5 — False-command rate

For an always-on interaction system, false activations must be measured explicitly.

Report:

\[
FAR=\frac{N_{false\ actions}}{T}.
\]

A high offline classifier score is insufficient if FAR is unacceptable.

---

## Test T6 — Optical system budget

For every display architecture report:

\[
\{FOV,eyebox,efficiency,luminance,MTF,distortion,latency,power\}.
\]

No architecture should be accepted based on a single favorable metric.

---

## Test T7 — Compute offload benefit

Compare all-local and distributed execution.

Measure:

\[
\Delta latency,
\Delta power,
\Delta bandwidth,
\Delta quality.
\]

The compute reservoir is only justified if distributed placement creates measurable system-level advantage.

---

## Test T8 — End-to-end interaction success

Measure task completion rather than component accuracy.

For example:

\[
\text{detect target}
\rightarrow
\text{infer intent}
\rightarrow
\text{select capability}
\rightarrow
\text{execute}
\rightarrow
\text{render result}.
\]

Report:

- task success rate,
- time-to-completion,
- correction rate,
- false activation rate,
- energy per successful task.

---

# 22. Major Failure Modes

## Neural failure

**Cause:** insufficient signal-to-noise ratio.  
**Consequence:** unstable intent estimates.  
**Mitigation:** multimodal fusion, hardware stabilization, explicit uncertainty.

## Motion failure

**Cause:** electrode movement, cable/interface motion, facial activity.  
**Consequence:** artifacts larger than the neural signal.  
**Mitigation:** mechanical electrode design + sensor fusion + source-level artifact suppression. Recent work emphasizes that purely post-processing motion-artifact removal can involve distortion, latency, or incomplete removal; reducing artifact generation at the electrode/interface is therefore an important research direction. [10]

## Decoder drift

**Cause:** changes in physiology, electrode placement, attention, fatigue.  
**Consequence:** degraded cross-day performance.  
**Mitigation:** adaptive decoding with drift detection and stable reference representations.

## Context hallucination

**Cause:** intelligence layer infers an unsupported user goal.  
**Consequence:** wrong action despite a plausible language-model explanation.  
**Mitigation:** calibrated uncertainty, evidence tracking, action thresholds, authorization.

## Optical failure

**Cause:** insufficient efficiency or uniformity.  
**Consequence:** low brightness, large power demand, poor image quality.  
**Mitigation:** optical budget and wave-optics optimization before packaging.

## Distributed-system failure

**Cause:** latency, node loss, synchronization errors, bandwidth bottlenecks.  
**Consequence:** broken ambient interaction.  
**Mitigation:** local fallback, deadlines, replication where necessary, semantic consistency rules.

## Trust failure

**Cause:** overconfident automation.  
**Consequence:** user loses control or system executes unintended operations.  
**Mitigation:** explicit confidence, capability-based permissions, reversible actions, user confirmation for consequential operations.

---

# 23. Key Research Metrics

The project should maintain a common scorecard.

### Neural interface

\[
\{SNR,Accuracy,Information\ Rate,Calibration\ Time,FAR,FRR,Latency,Cross-day\ Stability\}
\]

### Optics

\[
\{FOV,Eyebox,Eye\ Relief,Efficiency,MTF,Luminance,Uniformity,Chromatic\ Error,Focus\ Range\}
\]

### Intelligence

\[
\{Prediction\ Error,Planning\ Success,Calibration,Uncertainty\ Calibration,Task\ Success,Memory\ Retrieval\ Accuracy\}
\]

### Runtime

\[
\{Scheduling\ Latency,Node\ Recovery,Throughput,Consistency,Power,Availability\}
\]

### Whole system

\[
\{Task\ Success,Time/Task,Energy/Task,FAR,Human\ Correction\ Rate,User\ Workload\}
\]

The highest-level metric should be:

\[
\boxed{
\text{successful useful interaction per unit of energy, time and user effort}
}
\]

---

# 24. What Is Actually Frontier-Level

Not every component of NEXUS is a frontier problem.

### Established engineering

- event systems,
- RPC/IPC,
- device discovery,
- distributed scheduling primitives,
- model serving,
- databases,
- conventional cameras,
- conventional IMUs,
- ordinary application orchestration.

These should be implemented rather than treated as research unknowns.

### Active frontier

- compact high-performance near-eye optical systems,
- large-eyebox/high-efficiency waveguides,
- dynamic focus and light-field architectures,
- wearable high-quality EEG,
- motion artifact suppression,
- cross-user and cross-session neural decoding,
- useful silent-speech interfaces,
- predictive physical world models,
- reliable long-horizon embodied planning.

### NEXUS-specific integration frontier

- useful non-invasive intent decoding combined with environment context,
- persistent multimodal user/world state,
- adaptive neural–machine co-learning,
- semantic compute scheduling,
- a stable operating abstraction spanning human intent and heterogeneous devices,
- continuous operation within practical power and latency constraints.

The third category is where the project can potentially produce a distinctive systems research contribution.

---

# 25. The Central Scientific Hypothesis

The most important NEXUS hypothesis is:

\[
\boxed{
\text{Useful neural interaction does not require complete neural decoding if context reduces the inference space.}
}
\]

More formally, if

\[
I(I;EEG)>0
\]

but is limited by noise and ambiguity, then the joint information

\[
I(I;EEG,EMG,EOG,Gaze,Context,World)
\]

may be substantially larger.

The research task is to measure that increase rather than assume it.

This can be tested experimentally.

---

# 26. The Central Systems Hypothesis

A second hypothesis is:

\[
\boxed{
\text{A human-centered compute fabric can outperform device-centric interaction by choosing computation according to context rather than device ownership.}
}
\]

A task should be placed where the joint cost

\[
J=\alpha L+\beta E+\gamma C+\delta B+\epsilon Risk
\]

is minimized subject to quality and authorization constraints.

This can also be tested experimentally.

---

# 27. The Central Optical Hypothesis

A third hypothesis is:

\[
\boxed{
\text{The useful NEXUS visual interface can be defined by retinal information requirements rather than by conventional display form factors.}
}

Instead of beginning with "build a tiny screen," the research process begins with:

\[
\text{desired retinal optical field}
\rightarrow
\text{required wavefront}
\rightarrow
\text{optical architecture}
\rightarrow
\text{hardware}
\]

This leaves room for waveguide, diffractive, holographic, Maxwellian, varifocal, or hybrid architectures.

---

# 28. The Central OS Hypothesis

The NEXUS operating abstraction is:

\[
\boxed{
\text{Human}
\leftrightarrow
\text{Context}
\leftrightarrow
\text{Capability}
\leftrightarrow
\text{Intent}
\leftrightarrow
\text{Compute}
}
\]

rather than the traditional:

\[
\text{Application}
\leftrightarrow
\text{Device}.
\]

A meaningful NEXUS OS contribution would therefore require a runtime in which human/context/capability objects are executable primitives rather than documentation concepts.

---

# 29. Research Strategy

The shortest path to scientific evidence is **not** to begin by solving the complete glasses hardware stack.

The recommended sequence is:

```text
1. Formal state model
       ↓
2. Executable NEXUS runtime
       ↓
3. Synthetic multimodal BCI
       ↓
4. Real gaze/EOG/EMG/IMU integration
       ↓
5. Real EEG integration
       ↓
6. Adaptive multimodal intent model
       ↓
7. World model + agent loop
       ↓
8. Compute reservoir scheduler
       ↓
9. Optical simulator
       ↓
10. Physical optical prototype
       ↓
11. Closed-loop integrated prototype
```

This preserves the complete architecture while moving the highest-risk hardware problems into controlled research stages.

---

# 30. Final Assessment

NEXUS contains four classes of difficulty:

\[
\boxed{
\text{physics difficulty}
+
\text{biological difficulty}
+
\text{AI difficulty}
+
\text{systems difficulty}
}
\]

The optical problem is constrained by wave physics, étendue, coupling efficiency, focal behavior, and ocular safety.

The neural problem is constrained by weak signals, volume conduction, artifacts, electrode mechanics, non-stationarity, inter-subject variation, and limited information bandwidth.

The intelligence problem is constrained by incomplete observations, uncertain world state, long-horizon prediction, memory, planning, and action reliability.

The operating-system problem is constrained by distributed systems, heterogeneous hardware, synchronization, model placement, privacy, permissions, and real-time behavior.

The final challenge is the intersection:

\[
\boxed{
\text{photonic interface}
\times
\text{neural sensing}
\times
\text{world model}
\times
\text{distributed OS}
}
\]

A successful NEXUS research system must demonstrate that these layers work as a **single closed loop**, not merely that each layer works independently.

The decisive scientific contribution would therefore not be a claim of "mind reading," a new pair of smart glasses, or another chatbot interface. It would be a demonstrated architecture in which:

1. non-invasive human signals contribute measurable information about interaction intent;
2. context and world models amplify the usefulness of those weak signals;
3. the operating environment represents intent and capabilities as first-class computational objects;
4. computation is dynamically placed across available resources;
5. optical output closes the human–machine loop; and
6. the complete system remains measurable, adaptive, safe, and robust outside ideal laboratory conditions.

That is the frontier that must be conquered for NEXUS to move from architecture to scientific and engineering reality.

---

# References

**[1]** Tian, Z. et al. *An achromatic metasurface waveguide for augmented reality displays.* **Light: Science & Applications** 14, 94 (2025).  
https://www.nature.com/articles/s41377-025-01761-w

**[2]** *Breaking the in-coupling efficiency limit in waveguide-based AR displays with polarization volume gratings.* **Light: Science & Applications** (2024).  
https://www.nature.com/articles/s41377-024-01537-8

**[3]** *Design of waveguide with double layer diffractive optical elements for augmented reality displays.* **Scientific Reports** (2024).  
https://www.nature.com/articles/s41598-024-75766-7

**[4]** Tian, Z. et al. *An achromatic metasurface waveguide for augmented reality displays.* **Light: Science & Applications** 14, 94 (2025).  
https://www.nature.com/articles/s41377-025-01761-w

**[5]** *Single-layer waveguide displays using achromatic metagratings for full-colour augmented reality.* **Nature Nanotechnology** (2025).  
https://www.nature.com/articles/s41565-025-01887-3

**[6]** *Multi-depth switching by triple wavefront modulation of quarter-waveplate geometric phase lenses for vergence-accommodation-matching extended reality.* **Light: Science & Applications** 14, 333 (2025).  
https://www.nature.com/articles/s41377-025-02026-2

**[7]** U.S. Food and Drug Administration. *Frequently Asked Questions About Lasers.*  
https://www.fda.gov/radiation-emitting-products/laser-products-and-instruments/frequently-asked-questions-about-lasers

**[8]** U.S. Food and Drug Administration. *Product Classification — Device laser visual display, display retinal image, non-medical.* Updated September 2026.  
https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfPCD/classification.cfm?ID=REE

**[9]** *Recent Advances in Portable Dry Electrode EEG: Architecture and Applications in Brain-Computer Interfaces.* PubMed PMID: 40872076 (2025).  
https://pubmed.ncbi.nlm.nih.gov/40872076/

**[10]** Huang, H. et al. *Source-level motion artifact suppression in wearable electrodes: From underlying mechanisms to advanced strategies.* **Materials Today Bio** (2026). DOI: 10.1016/j.mtbio.2026.103241.  
https://pubmed.ncbi.nlm.nih.gov/42220613/

**[11]** *A Systematic Review of Techniques for Artifact Detection and Artifact Category Identification in Electroencephalography from Wearable Devices.* PubMed PMID: 41013007 (2025).  
https://pubmed.ncbi.nlm.nih.gov/41013007/

**[12]** d’Ascoli, S. et al. *Towards decoding individual words from non-invasive brain recordings.* **Nature Communications** 16, 10521 (2025).  
https://www.nature.com/articles/s41467-025-65499-0

**[13]** Ding, Y. et al. *EEG-based brain-computer interface enables real-time robotic hand control at individual finger level.* **Nature Communications** 16, 5401 (2025).  
https://www.nature.com/articles/s41467-025-61064-x

**[14]** Tang, C. et al. *Sensing technologies for silent speech interfaces.* **Nature Sensors** 1, 16–26 (2026). DOI: 10.1038/s44460-025-00010-2.  
https://www.nature.com/articles/s44460-025-00010-2

**[15]** Meta AI. *V-JEPA 2: Self-Supervised Video Models Enable Understanding, Prediction and Planning.* June 2025.  
https://ai.meta.com/research/publications/v-jepa-2-self-supervised-video-models-enable-understanding-prediction-and-planning/

**[16]** Meta AI. *Introducing V-JEPA 2 — A self-supervised foundation world model.* June 2025.  
https://ai.meta.com/research/vjepa/

**[17]** U.S. Food and Drug Administration. *Recognized Consensus Standards — IEC 63145-22-10: Eyewear display — Specific measurement methods for AR type — Optical properties.* Updated May 2026.  
https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfStandards/detail.cfm?standard__identification_no=45471

---

## Research Integrity Note

The present document distinguishes between established engineering techniques, active research directions, and NEXUS-specific hypotheses. Experimental results from cited literature are not presented as validation of the complete NEXUS architecture. No claim of general-purpose non-invasive thought decoding is made. No particular optical, neural, AI, or OS technology is assumed to be the final implementation.
