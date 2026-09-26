# NEXUS OS

### A human-intent operating environment for personal computing

> **From signals to intent. From intent to capability. From capability to computation. From computation to action.**

Nexus OS is a research and engineering project exploring a different abstraction for personal computing: a persistent user environment in which **human intent, context, memory, world state, capabilities, and computation** are first-class system primitives.

The long-term physical product is advanced everyday eyewear with multimodal human sensing and an adaptive optical system. The present engineering target is deliberately earlier: build the **software and systems architecture that makes the eventual hardware meaningful**.

The project therefore begins above the hardware stack rather than below it.

```text
                        NEXUS PERSONAL ENVIRONMENT
                                      │
                  ┌───────────────────┼───────────────────┐
                  │                   │                   │
               IDENTITY           MEMORY              CONTEXT
                  │                   │                   │
                  └───────────────────┼───────────────────┘
                                      │
                                  WORLD MODEL
                                      │
                         HUMAN SIGNAL / SENSOR BUS
                                      │
             ┌────────────────────────┼────────────────────────┐
             │                        │                        │
            EEG                      EMG                     GAZE
             │                        │                        │
            VOICE                   VISION                    IMU
             └────────────────────────┼────────────────────────┘
                                      │
                              SIGNAL ENCODERS
                                      │
                              INTENT EVIDENCE
                                      │
                                INTENT IR / ABI
                                      │
                              PERSONAL AGENT
                                      │
                           POLICY / ADAPTATION
                                      │
                              TRUST / RISK
                                      │
                            CAPABILITY GRAPH
                                      │
                               ACTION IR / ABI
                                      │
                              COMPUTE FABRIC
                    ┌─────────────────┼──────────────────┐
                    │                 │                  │
                  LOCAL            WIRELESS            WIRED
                    │                 │                  │
                    └─────────────────┼──────────────────┘
                                      │
                                    CLOUD
                                      │
                               APPLICATIONS
                                      │
                   ┌──────────────────┼──────────────────┐
                   │                  │                  │
                 DESKTOP            MOBILE               XR
                   │                  │                  │
                   └──────────────────┼──────────────────┘
                                      │
                                    HUMAN
```

---

## 1. What this project is

Nexus OS is **not currently a conventional operating-system kernel** and does not attempt to replace Linux, Android, Windows, or macOS at the kernel/driver level.

It is a research implementation of a new semantic operating layer that can eventually sit above, beside, and later partially below existing operating systems.

The central hypothesis is:

\[
\boxed{
\text{Personal computing should be organized around human goals and intent, not around applications and input devices.}
}
\]

The system should eventually let a person interact through whatever combination of:

- voice,
- gaze,
- hand motion,
- EMG / neuromuscular signals,
- EEG / BCI signals,
- environmental perception,
- conventional keyboard/mouse/touch,

is most appropriate for the current context.

The application should not need to know which modality generated the interaction.

```text
EEG ──────┐
EMG ──────┤
Gaze ─────┤
Voice ────┼──→ IntentIR("select", target=X)
Gesture ──┤
Mouse ────┘
```

This is the first fundamental abstraction.

The second is capability:

```text
Human goal
    ↓
Intent
    ↓
Capability
    ↓
Action
```

The third is compute placement:

```text
Workload
   ↓
Nexus Compute Fabric
   ├── local
   ├── nearby wireless
   ├── high-bandwidth wired
   └── cloud
```

---

# 2. Why Nexus is not simply another AI assistant

A conventional assistant is approximately:

```text
prompt → model → answer
```

Nexus is intended to be a persistent closed loop:

```text
observe
  ↓
understand
  ↓
infer intent
  ↓
resolve context
  ↓
select capability
  ↓
allocate computation
  ↓
act
  ↓
observe outcome
  ↓
remember
  ↓
learn
```

Mathematically:

\[
O_t \rightarrow W_t \rightarrow C_t \rightarrow I_t
\rightarrow A_t \rightarrow O_{t+1}
\]

where:

- \(O_t\) = observations;
- \(W_t\) = world state;
- \(C_t\) = contextual state;
- \(I_t\) = inferred user intent;
- \(A_t\) = selected action.

A personalized policy is then represented conceptually as:

\[
\pi_\theta(a_t\mid s_t,I_t)
\]

with experience:

\[
\tau_t=(s_t,a_t,r_t,s_{t+1}).
\]

The long-term research question is whether a policy can become increasingly useful for a particular user without sacrificing agency, safety, privacy, or predictability.

---

# 3. The CUDA-like architectural thesis

Nexus takes inspiration from the **architecture of CUDA as a platform**, not from GPUs as a product category.

NVIDIA describes CUDA as a platform and software layer connecting applications to accelerated hardware, supported by a toolkit containing compilers, runtime libraries, accelerated libraries, and developer/debugging/profiling tools. 

The strategic lesson is:

\[
\boxed{
\text{Hardware becomes strategically powerful when an abstraction ecosystem forms above it.}
}
\]

Nexus therefore aims for:

| CUDA-style concept | Nexus analogue |
|---|---|
| GPU programming model | Human-intent programming model |
| CUDA runtime | Nexus runtime |
| PTX / intermediate representation | Intent IR / Action IR |
| CUDA libraries | Capability / perception / context libraries |
| Nsight profiling | Nexus interaction/latency profiler |
| CUDA-aware applications | Intent-native applications |
| GPU acceleration | Human-compute acceleration |
| GPU hardware | Future Nexus hardware |

The goal is not to make the interface proprietary merely for the sake of lock-in.

The goal is to create an abstraction that developers **want** to target because it removes complexity and gives them capabilities unavailable through lower-level interfaces.

---

# 4. Core system abstractions

Nexus currently revolves around eight foundational objects:

```text
World
Context
Memory
Intent
Capability
Action
Policy
Compute
```

### World

Represents the relevant physical and digital entities around the user.

\[
W_t=(V_t,E_t,S_t)
\]

where \(V_t\) are entities, \(E_t\) relationships, and \(S_t\) state.

### Context

Represents what is relevant now:

\[
C_t=f(W_t,M_t,G_t,H_t)
\]

where the state is conditioned on world, memory, current goals and history.

### Memory

Long-lived structured state rather than raw conversation transcripts.

Initial classes:

\[
M=\{M_E,M_S,M_P\}
\]

with episodic, semantic and procedural memory.

### Intent

A latent representation of what the user is attempting to accomplish.

### Capability

An operation the computational environment can provide.

Examples:

```text
calendar.schedule
communication.send
navigation.route
research.compare
document.create
simulation.run
media.play
machine.inspect
robot.execute
```

### Action

A concrete authorized operation against a capability.

### Policy

The decision system that maps state and intent to candidate actions.

### Compute

A resource abstraction spanning local processors, nearby machines, wired systems, and cloud services.

---

# 5. Intent IR

The **Intent Intermediate Representation** is one of the most strategically important pieces of the project.

A first version is:

```python
@dataclass
class IntentIR:
    verb: str
    targets: list[str]
    parameters: dict
    context_refs: list[str]
    confidence: float
    urgency: float
    reversibility: str
    evidence: list[str]
```

Example:

```json
{
  "verb": "compare",
  "targets": ["paper:A", "paper:B"],
  "parameters": {},
  "context_refs": ["project:thermal-lab"],
  "confidence": 0.96,
  "urgency": 0.1,
  "reversibility": "high",
  "evidence": ["gaze", "voice", "context"]
}
```

The IR is explicitly independent of the physical input source.

This allows:

```text
EEG       ─┐
EMG       ─┤
Gaze      ─┤
Voice     ─┼──→ same IntentIR
Gesture   ─┤
Keyboard  ─┘
```

The eventual goal is a stable semantic ABI so applications can survive hardware generations.

---

# 6. Action IR and capability graph

Intent alone is insufficient.

The environment also needs a description of what it can actually do.

```text
                   Capability Graph
                          │
       ┌──────────────────┼──────────────────┐
       │                  │                  │
  Communication       Computation          World
       │                  │                  │
   send_message       run_simulation      inspect
   make_call          train_model          navigate
   share              render               locate
       │                  │                  │
       └──────────────────┼──────────────────┘
                          │
                      Providers
```

A provider may be:

- a native Nexus service;
- an Android application;
- a web service;
- a desktop application;
- an MCP tool;
- a local program;
- a physical machine;
- a future Nexus device.

This is how Nexus can inherit the existing software ecosystem rather than attempting to rebuild it from zero.

---

# 7. Personal agent architecture

The assistant is a persistent agent operating over the environment.

```text
                      PERSONAL AGENT
                            │
             ┌──────────────┼──────────────┐
             │              │              │
         Perception      Context         Memory
             │              │              │
             └──────────────┼──────────────┘
                            │
                       Goal inference
                            │
                       Intent resolver
                            │
                    Candidate generation
                            │
                       Policy layer
                            │
                     Risk / trust gate
                            │
                     Action selection
                            │
                         Executor
                            │
                        Outcome
                            │
                         Reward
                            │
                        Learning
```

The learned policy should never be allowed to bypass deterministic safety or permission boundaries.

A learned component may propose:

\[
\hat a=\pi_\theta(s_t)
\]

but the final action is:

\[
a_t = G(\hat a, P_t, R_t)
\]

where \(P_t\) represents permissions and \(R_t\) risk constraints.

---

# 8. Personalization and RL

The goal is not generic reinforcement learning for its own sake.

The goal is **personalized interaction policy learning**.

The training sample should eventually resemble:

```text
signals
   ↓
perception
   ↓
intent evidence
   ↓
context
   ↓
candidate actions
   ↓
selected action
   ↓
outcome
   ↓
user correction / confirmation
   ↓
reward
```

A trajectory is:

\[
\tau=(s_0,a_0,r_0,s_1,\ldots,s_T).
\]

The reward should not simply be “user clicked something.”

A first research reward function can combine:

\[
r_t=
\alpha R_{success}
-\beta C_{interaction}
-\gamma C_{latency}
-\delta C_{error}
-\eta C_{interrupt}
\]

subject to trust and safety constraints.

This lets the system learn that a successful action that required ten unnecessary interactions is not equivalent to the same outcome obtained naturally in one step.

---

# 9. BCI architecture

Nexus treats EEG and EMG as **evidence channels**, not as magical command streams.

```text
EEG ───────→ EEG representation ─┐
                                  │
EMG ───────→ EMG representation ─┼→ multimodal intent model
                                  │
Gaze ──────→ gaze representation ─┤
                                  │
Voice ─────→ language evidence ───┤
                                  │
Vision ────→ environmental state ─┘
```

The system should initially model:

\[
P(I_t\mid EEG,EMG,Gaze,Voice,Vision,Context)
\]

rather than:

\[
P(I_t\mid EEG).
\]

This matters because the same neural or muscular signal can have different meanings in different contexts.

---

# 10. EEG data strategy

Public EEG datasets should be used for reproducible research and representation learning, not assumed to be the final proprietary training corpus.

### PhysioNet EEG Motor Movement/Imagery Dataset

[PhysioNet EEGMMIDB](https://physionet.org/content/eegmmidb/1.0.0/)

A foundational dataset for controlled motor execution and imagery experiments.

### BCI Competition IV

[BCI Competition IV](https://www.bbci.de/competition/iv/)

Dataset 2a contains 22 EEG channels, 3 EOG channels, 250 Hz sampling, four motor-imagery classes and nine subjects; Dataset 2b contains three bipolar EEG channels, 3 EOG channels, 250 Hz sampling, two classes and nine subjects. 

### MOABB

[MOABB](https://github.com/NeuroTechX/moabb)

Provides standardized BCI benchmarking across many public EEG datasets and supports cross-session evaluations. 

The first useful Nexus EEG benchmark should therefore report:

```text
within-subject
cross-session
cross-subject
cross-dataset
```

rather than only a single shuffled train/test accuracy.

---

# 11. EMG data strategy

### Meta / Facebook Research emg2pose

[emg2pose](https://github.com/facebookresearch/emg2pose)

The repository describes a dataset of 25,253 HDF5 files, approximately 370 hours of recordings, 193 participants and 2 kHz time-aligned sEMG plus hand joint angles. It explicitly includes held-out-user and held-out-stage evaluation information. 

This is exceptionally useful for studying cross-user and cross-stage generalization.

However, the repository/data licensing must be reviewed before any commercial use. Do not treat a research dataset as automatically suitable for proprietary product training.

### NinaPro

[NinaPro](https://ninapro.hevs.ch/)

A major public sEMG/hand-movement dataset family suitable for gesture and neuromuscular representation research.

### Gest-Infer

[Gest-Infer](https://github.com/HumanMachineInterface/Gest-Infer)

Contains raw EMG and EEG recordings from 33 subjects, along with acquisition and preprocessing code and pretrained models. 

This is particularly useful for testing the joint EEG+EMG problem.

---

# 12. EEG/EMG acquisition and synchronization

## BrainFlow

[BrainFlow](https://github.com/brainflow-dev/brainflow)

BrainFlow provides a uniform API for EEG, EMG and other biosensors and supports Python, C++, Java, C#, Matlab and Julia. Its core repository is MIT licensed, with an additional SimpleBLE licensing restriction that must be respected. citehttps://github.com/brainflow-dev/brainflow

## Lab Streaming Layer

[LabStreamingLayer](https://labstreaminglayer.readthedocs.io/)

LSL is intended for unified collection of time-series measurements, networking, time synchronization, near-real-time access and recording. It is particularly useful when EEG, EMG, eye tracking, motion and task events have to share a common temporal reference. 

LSL's documentation emphasizes sample timestamps and clock-offset measurements for synchronization and XDF recording. 

---

# 13. Signal-processing stack

## MNE-Python

[MNE-Python](https://github.com/mne-tools/mne-python)

Core neurophysiology analysis package for EEG/MEG and related signals. Current releases require Python 3.11+ and use a BSD-3-Clause license. 

## Braindecode

[Braindecode](https://github.com/braindecode/braindecode)

Deep-learning framework for electrophysiological decoding, including EEG/BCI applications. The main project is BSD-3-Clause, with some additional components under other licenses. 

These should provide preprocessing and baseline model infrastructure while Nexus develops its own representations and fusion architecture.

---

# 14. Optical architecture

The future hardware program is based on an adaptive optical system rather than a static “screen in front of the eye.”

The conceptual optical loop is:

```text
human state
   +
world state
   +
task state
   ↓
optical presentation requirement
   ↓
computational optical controller
   ↓
source / phase / steering / focus parameters
   ↓
photonic / diffractive / waveguide system
   ↓
formed optical field
   ↓
eye
   ↓
measurement / model feedback
   ↺
```

The physical wavefront can be written as:

\[
E(x,y)=A(x,y)e^{i\phi(x,y)}.
\]

A simplified propagation model may be represented through the angular spectrum method:

\[
E(x,y,z)=
\mathcal F^{-1}
\left[
\mathcal F(E_0)
H(f_x,f_y,z)
\right].
\]

The real engineering problem is that the simulated forward model:

\[
\hat y=\hat F(u,\Theta)
\]

does not perfectly match the real optical hardware:

\[
y=F(u,\Theta_{real}).
\]

Therefore the long-term architecture requires **camera/sensor-in-the-loop calibration and optimization**.

---

# 15. Optical research repositories

## Neural Holography

[computational-imaging/neural-holography](https://github.com/computational-imaging/neural-holography)

Provides camera-in-the-loop optimization, parameterized wave-propagation models, Holonet training and hardware automation examples. The repository states that its code and data are released under CC BY-NC, with commercial licensing available from Stanford. 

This is a research reference, **not** an automatically reusable commercial dependency.

## Synthetic Aperture Waveguide Holography

[choisuyeon/sawh](https://github.com/choisuyeon/sawh)

Code and data accompanying the *Nature Photonics* work on synthetic-aperture waveguide holography. The repository is MIT licensed and includes a computer-generated holography framework and a partially coherent implicit neural waveguide model. 

## Diffractsim

[rafael-fuente/diffractsim](https://github.com/rafael-fuente/diffractsim)

Python diffraction simulator supporting scalar propagation, phase holograms, GPU acceleration and differentiable JAX execution. 

## MEEP

[NanoComp/meep](https://github.com/NanoComp/meep)

FDTD electromagnetic simulation for rigorous optical modeling, with Python, Scheme and C++ interfaces and distributed-memory support. MEEP is GPL-2.0. 

## RCWA

[edmundsj/rcwa](https://github.com/edmundsj/rcwa)

Rigorous coupled-wave analysis for photonic structures and grating diffraction. The repository is MIT licensed. 

---

# 16. Optical physics that must eventually be mastered

The hardware research stack is built around:

### Maxwell's equations

\[
\nabla\times\mathbf E=-\frac{\partial\mathbf B}{\partial t}
\]

\[
\nabla\times\mathbf H=
\mathbf J+\frac{\partial\mathbf D}{\partial t}.
\]

### Complex wavefront

\[
E=Ae^{i\phi}.
\]

### Fourier optics

Spatial frequencies, apertures, transfer functions and propagation.

### Diffraction

How finite apertures and periodic structures transform optical fields.

### Grating coupling

Wavelength, period, incidence angle and output angle are coupled.

### Étendue

\[
G=n^2A\Omega.
\]

This is crucial for understanding why FOV, eyebox, brightness and optical volume cannot all be increased independently.

### Polarization

Required for understanding many waveguide and metasurface architectures.

### Thin-film / nanophotonic response

Needed for metasurface and subwavelength structures.

### Coherence

Laser sources introduce coherence-related effects such as speckle and interference.

### Waveguide propagation

The system must manage coupling, propagation, extraction, uniformity and loss.

### Human optics

Accommodation, vergence, binocular disparity, focus cues, aberrations and individual refractive variation.

The desired mastery is not memorization.

For each subsystem the engineering questions are:

```text
Forward model
What input physically creates the output?

State
Which variables actually determine system state?

Inverse problem
What input produces the desired output?

Bottleneck
Which physical constraint prevents the ideal system?

Control
How is error measured and corrected?
```

---

# 17. Adaptive optical control

The eventual optical controller can treat the system as a constrained optimization problem.

Let:

\[
u_t=
\{P_R,P_G,P_B,\theta_x,\theta_y,\phi,F,\Gamma\}
\]

represent controllable optical variables.

Then:

\[
y_t=F(x_t,u_t,\Theta)
\]

where \(x_t\) contains gaze, world state and task state.

A multi-objective loss can be written as:

\[
\mathcal L=
\lambda_1L_{image}
+\lambda_2L_{wavefront}
+\lambda_3L_{focus}
+\lambda_4L_{energy}
+\lambda_5L_{latency}
+\lambda_6L_{speckle}
+\lambda_7L_{calibration}.
\]

A hard safety layer remains outside the learned optimizer.

```text
learned optimizer
       ↓
candidate optical state
       ↓
deterministic safety constraints
       ↓
physical controller
       ↓
optical system
```

No experimental software in this repository should be connected to a laser source without an appropriate optical-radiation safety design and qualified engineering review.

---

# 18. Human signal bus

All human-interface sources enter through one normalized signal bus.

```text
SignalEnvelope
├── timestamp
├── modality
├── device_id
├── channel metadata
├── sampling rate
├── confidence
├── payload
└── provenance
```

Example:

```json
{
  "timestamp": 12345.678,
  "modality": "emg",
  "device_id": "board_01",
  "sampling_hz": 2000,
  "confidence": 0.92,
  "payload": "..."
}
```

This abstraction is intentionally device-independent.

---

# 19. Context graph

The personal environment needs a structured graph rather than a giant conversation log.

\[
G_t=(V_t,E_t,S_t)
\]

Possible nodes:

```text
Person
Place
Object
Project
Task
Document
Device
Event
Concept
Goal
Capability
```

Possible relations:

```text
works_on
located_at
observing
uses
created
discussed
related_to
intends
depends_on
caused_by
```

The graph evolves:

\[
G_{t+1}=F(G_t,O_t,A_t).
\]

The most important property is temporal continuity.

The environment should be able to represent that a task began yesterday, moved from one place to another today, involved particular documents and devices, and remains unfinished.

---

# 20. Personal memory architecture

Nexus memory should be structured and inspectable.

```text
memory/
├── episodic/
├── semantic/
├── procedural/
├── preferences/
├── goals/
└── provenance/
```

Every memory should carry provenance:

```text
source
confidence
created_at
updated_at
scope
retention
user_control
```

The user should be able to:

```text
remember
retrieve
inspect
edit
forget
export
```

Memory portability is a design requirement because the long-term moat should come from capability and quality, not from preventing the user from leaving.

---

# 21. Compute Fabric

The compute fabric gives Nexus a single resource model over heterogeneous devices.

```text
                         Compute Fabric
                              │
            ┌─────────────────┼─────────────────┐
            │                 │                 │
          LOCAL            WIRELESS            WIRED
            │                 │                 │
       device NPU         nearby node       workstation
       device GPU        home reservoir    high-bandwidth
            │                 │                 │
            └─────────────────┼─────────────────┘
                              │
                             CLOUD
```

Each node advertises capabilities:

```python
ComputeCapability(
    inference=True,
    rendering=True,
    training=False,
    memory_gb=32,
    bandwidth_gbps=40,
)
```

A workload describes requirements:

```python
Workload(
    task="spatial_reasoning",
    latency="critical",
    privacy="high",
    power="constrained",
)
```

The scheduler solves:

\[
N^*=
\arg\min_N
C(N\mid L,B,P,E,R)
\]

where:

- \(L\) = latency;
- \(B\) = bandwidth;
- \(P\) = privacy;
- \(E\) = energy;
- \(R\) = reliability.

---

# 22. Wireless versus wired

The architecture deliberately preserves both.

### Wireless

Best for:

- mobility;
- continuous interaction;
- ordinary daily use;
- low-friction computing;
- nearby personal compute.

### Wired

Best for:

- deterministic latency;
- high bandwidth;
- heavy rendering;
- professional workloads;
- large sensor streams;
- simulation;
- high-performance spatial computing.

The environment should automatically transition between them.

```text
walking
   ↓
wireless reservoir
   ↓
sit at workstation
   ↓
automatic wired upgrade
   ↓
heavy computation
```

The software state does not change.

Only the available computational resources change.

---

# 23. Device and OS compatibility

Nexus should initially be **above existing operating systems**.

```text
              Nexus Semantic OS
                      │
        ┌─────────────┼─────────────┐
        │             │             │
     Android        Linux         Windows/macOS
        │             │             │
        └─────────────┼─────────────┘
                      │
                physical devices
```

This is deliberate.

Android XR is rapidly expanding its own intelligent-eyewear platform. Google's current documentation lists hand interaction, eye-gaze interaction, 6DoF controllers and mouse interaction for OpenXR, while its audio/display glasses guidance says voice is the primary interaction method. The documented XR input modalities do not expose EEG or EMG as first-class XR input primitives.  

That does **not** mean Android cannot technically communicate with EEG/EMG hardware. It means Nexus has a meaningful architectural opening: BCI signals should be modeled at the semantic input layer rather than bolted onto an existing XR input vocabulary.

The correct approach is therefore not:

```text
build a completely incompatible OS first
```

but:

```text
Nexus semantic layer
        ↓
Android / Linux / Windows / macOS
        ↓
existing applications
```

and only later:

```text
Nexus semantic layer
        ↓
Nexus device OS
        ↓
Nexus hardware
```

---

# 24. Application compatibility

The largest barrier to a new OS is not the number of features.

It is the existing application ecosystem.

Nexus therefore needs an explicit compatibility ladder:

```text
Level 0 — existing application

Level 1 — Nexus can launch it

Level 2 — Nexus can expose selected capabilities

Level 3 — application adopts Nexus APIs

Level 4 — application becomes intent-native
```

This lets the platform acquire application coverage without requiring developers to rewrite their products on day one.

For example:

```text
WhatsApp
  ↓
communication.send

Maps
  ↓
navigation.route

Calendar
  ↓
calendar.schedule

Browser/search
  ↓
research.search
```

The user interacts through the Nexus semantic layer while existing software remains underneath it.

---

# 25. MCP and external capabilities

The Model Context Protocol can be treated as one integration mechanism for external tools and resources, rather than as the Nexus architecture itself.

Conceptually:

```text
Nexus Capability Graph
          │
          ├── native capability
          ├── local process
          ├── Android service
          ├── web API
          └── MCP tool
```

The architecture should preserve a distinction between:

\[
\text{semantic capability}
\]

and:

\[
\text{implementation provider}.
\]

That keeps the developer-facing API stable while implementations evolve.

---

# 26. Developer SDK

The SDK is the public face of the architecture.

```text
Nexus SDK
├── Intent API
├── Context API
├── World API
├── Memory API
├── Capability API
├── Action API
├── Compute API
├── Trust API
└── Device API
```

Example:

```python
intent = nexus.intent.current()

if intent.matches("compare"):
    result = nexus.capability.run(
        "research.compare",
        intent.targets
    )
```

A developer should not need to know whether `intent.matches("compare")` was produced by EEG, EMG, gaze, speech, keyboard or some combination.

---

# 27. Developer tooling

A CUDA-like ecosystem requires tools, not only APIs.

The Nexus development environment should eventually include:

```text
nexus-cli
nexus-sim
nexus-debug
nexus-profiler
nexus-replay
nexus-inspector
nexus-benchmark
nexus-trace
```

The profiler should expose the full interaction path:

```text
Signal acquisition       3.2 ms
Signal fusion            4.1 ms
Context resolution       5.0 ms
Intent inference         7.8 ms
Capability resolution    1.4 ms
Action dispatch          2.3 ms
Total                   23.8 ms
```

The exact numbers above are an example format, not a measured Nexus target.

---

# 28. Interaction profiling

The primary optimization metric is not “model accuracy.”

It is useful outcome per human effort.

Define:

\[
U_I=
\frac{N_{successful\ outcomes}}
{C_{explicit\ interaction}}
\]

and evaluate it alongside:

\[
T_{latency},
E_{energy},
P_{false\ action},
C_{cognitive}.
\]

A system that saves clicks but causes incorrect actions is not an improvement.

---

# 29. Experimental philosophy

The project should preserve the distinction between four levels:

### Reproduction

Can an established result be reproduced?

### Extension

Can it be improved or generalized?

### Integration

Can independent components operate together?

### Original architecture

Does the integrated system introduce a capability that was previously difficult because the components were separated?

The repository should label experiments accordingly.

```text
experiment:
    type: reproduction | extension | integration | original
    hypothesis:
    setup:
    metric:
    result:
    failure:
    interpretation:
```

This prevents the project from confusing a successful demo with a scientific breakthrough.

---

# 30. Reproducibility

Every meaningful experiment should preserve:

```text
code
configuration
seed
hardware
software version
model checkpoint
input data manifest
metrics
plots
failure cases
```

No raw biomedical dataset should be committed to the repository unless its license explicitly permits that distribution and the project has the required permissions.

Prefer dataset manifests and download scripts.

---

# 31. Recommended repository structure

```text
nexus-os/
│
├── kernel/
│   ├── intent/
│   ├── context/
│   ├── world/
│   ├── memory/
│   ├── capability/
│   ├── action/
│   ├── trust/
│   └── runtime/
│
├── agent/
│   ├── perception/
│   ├── grounding/
│   ├── planning/
│   ├── reflection/
│   ├── policy/
│   ├── personalization/
│   └── assistant/
│
├── learning/
│   ├── offline_rl/
│   ├── bandits/
│   ├── imitation/
│   ├── preference_learning/
│   ├── reward/
│   └── evaluation/
│
├── bci/
│   ├── eeg/
│   ├── emg/
│   ├── gaze/
│   ├── voice/
│   ├── imu/
│   ├── fusion/
│   └── adapters/
│
├── perception/
│   ├── vision/
│   ├── speech/
│   ├── spatial/
│   └── multimodal/
│
├── compute/
│   ├── nodes/
│   ├── scheduler/
│   ├── allocator/
│   ├── migration/
│   ├── cache/
│   └── policies/
│
├── fabric/
│   ├── state/
│   ├── synchronization/
│   ├── transport/
│   ├── wireless/
│   ├── wired/
│   └── cloud/
│
├── compatibility/
│   ├── android/
│   ├── linux/
│   ├── windows/
│   ├── macos/
│   ├── web/
│   └── mcp/
│
├── optics/
│   ├── propagation/
│   ├── holography/
│   ├── waveguide/
│   ├── metasurface/
│   ├── controller/
│   ├── calibration/
│   └── simulation/
│
├── hardware/
│   ├── reference/
│   ├── bci/
│   ├── compute/
│   └── future/
│
├── sdk/
│   ├── python/
│   ├── typescript/
│   ├── rust/
│   ├── kotlin/
│   └── schemas/
│
├── developer/
│   ├── simulator/
│   ├── debugger/
│   ├── profiler/
│   ├── replay/
│   └── cli/
│
├── datasets/
│   ├── manifests/
│   ├── eeg/
│   ├── emg/
│   └── multimodal/
│
├── benchmarks/
│   ├── bci/
│   ├── intent/
│   ├── personalization/
│   ├── agent/
│   ├── compute/
│   ├── latency/
│   └── end_to_end/
│
├── experiments/
│   ├── bci/
│   ├── intent/
│   ├── agent/
│   ├── personalization/
│   ├── compute/
│   └── optics/
│
├── tests/
├── docs/
├── papers/
├── tools/
└── examples/
```

---

# 32. Research dependency map

```text
                    NEXUS R&D
                        │
     ┌──────────────────┼──────────────────┐
     │                  │                  │
    BCI               AI/HCI             Optics
     │                  │                  │
  BrainFlow          PyTorch           Diffractsim
  MNE                Braindecode        RCWA
  MOABB              Transformers      MEEP
  LSL                RL libraries      Neural Holography
  EEG datasets       Context engines    SAWH
  EMG datasets       Agent frameworks   photonic tools
```

The principle is:

\[
\boxed{
\text{reuse mature infrastructure; own the new abstraction.}
}
\]

---

# 33. Licensing and provenance

This repository should maintain a `THIRD_PARTY.md` recording:

```text
project
version/commit
license
purpose
modifications
commercial-use status
citation
```

Important current examples:

| Project | Use | Current license / restriction to verify |
|---|---|---|
| BrainFlow | biosensor acquisition | MIT; SimpleBLE conditions also apply |
| MNE-Python | EEG/MEG analysis | BSD-3-Clause |
| Braindecode | EEG deep learning | BSD-3-Clause main project; additional components exist |
| MOABB | BCI benchmarks | verify current repository terms |
| SAWH | waveguide holography research | MIT |
| RCWA | grating / photonics simulation | MIT |
| MEEP | rigorous EM simulation | GPL-2.0 |
| Neural Holography | camera-in-loop holography | CC BY-NC; commercial licensing available |
| emg2pose | sEMG representation | dataset/repository terms must be reviewed |

Do not assume “public GitHub repository” means “commercially reusable.”

---

# 34. Hardware architecture — final direction

The eventual hardware system is organized around five physical subsystems.

```text
                     NEXUS HARDWARE
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
     SENSING             COMPUTE            OPTICS
       │                   │                   │
 EEG / EMG / Gaze      CPU / GPU / NPU      photonics
 Cameras / IMU         DSP / ISP            waveguide
 Microphones           memory               laser/source
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                        POWER
                           │
                    battery / thermal
                           │
                       MECHANICAL
                           │
                     ordinary eyewear
```

The final engineering problem is coupled:

\[
FOV, eyebox, brightness, mass, power, heat, latency, prescription,
\]

cannot be optimized independently.

The hardware program should therefore be treated as a **system co-design problem**.

---

# 35. The photonic/nanophotonic direction

The term “nanophotonic laser display” is used here to describe a research direction, not a single finalized implementation.

Candidate architectures include:

```text
laser source
   ↓
PIC / beam shaping
   ↓
scanner or phase control
   ↓
waveguide / diffractive coupler
   ↓
free-space optical field
   ↓
eye
```

or:

```text
laser
   ↓
programmable photonic structure
   ↓
waveguide
   ↓
computational holography
   ↓
eye
```

The long-term research opportunity is to make optical degrees of freedom **computationally programmable** and dynamically conditioned on human and environmental state.

---

# 36. The ultimate closed-loop optical problem

A useful formalization is:

\[
x_t=
\{
G_t,W_t,C_t,Q_t
\}
\]

where:

- \(G_t\) = gaze/eye state;
- \(W_t\) = world state;
- \(C_t\) = computational context;
- \(Q_t\) = task requirements.

Then:

\[
u_t=\pi_{opt}(x_t)
\]

and:

\[
y_t=F_{opt}(u_t,\Theta).
\]

Feedback estimates:

\[
e_t=y_t-y_t^*.
\]

The system then performs:

\[
u_{t+1}=u_t+K(e_t)
\]

or a learned/differentiable optimization variant, subject to hard constraints.

This is the point where optics, machine learning and control theory meet.

---

# 37. What “better than existing computing” actually means

Nexus should not compete on abstract intelligence alone.

The system must reduce concrete friction.

Examples:

```text
Old:
open app → find function → enter input → inspect → switch app

Nexus:
goal → intent → capability → result
```

```text
Old:
remember where I stopped

Nexus:
resume context
```

```text
Old:
choose device based on task

Nexus:
compute fabric chooses resource
```

```text
Old:
choose input modality

Nexus:
system fuses the useful modalities
```

The product metric is therefore:

\[
\boxed{
\frac{\text{useful capability}}{\text{human interaction burden}}
}
\]

---

# 38. The long-term proprietary architecture

The intended moat is not one component.

It is the interaction between:

```text
Intent IR
   +
Capability Graph
   +
Personal Agent
   +
Personalization
   +
Context Graph
   +
Compute Fabric
   +
Compatibility Layer
   +
Hardware Co-design
   +
Developer Tooling
```

A competitor can reproduce an individual sensor or device.

The harder problem is reproducing the **entire semantic system and ecosystem**.

That is the architectural analogue of a platform moat.

---

# 39. What must remain open and what can become proprietary

Nexus should distinguish between interoperability and proprietary differentiation.

### Good candidates for open/public interfaces

- Intent schemas
- capability schemas
- basic developer documentation
- interoperability formats
- data-export formats
- research benchmarks

### Strong candidates for proprietary implementation

- multimodal fusion algorithms
- personalized policy system
- context relevance engine
- world-model implementation
- compute scheduler optimizations
- hardware/firmware co-design
- optical control algorithms
- calibration methods
- specialized silicon architecture
- manufacturing process knowledge

The user should be able to leave with their data.

The reason to stay should be that Nexus remains better at turning that data into useful capability.

---

# 40. Safety and trust architecture

Because the eventual system continuously senses the user and environment, privacy is part of the engineering architecture.

The trust stack should include:

```text
Hardware trust
    ↓
Secure boot / attestation
    ↓
Permission system
    ↓
Data-scope policy
    ↓
Local-first processing
    ↓
Memory controls
    ↓
Action-risk policy
    ↓
Audit trail
```

For every action:

```text
Who requested it?
What evidence produced the intent?
What data was used?
What capability was invoked?
Where did computation occur?
Was external data transmitted?
What was the consequence?
```

The final system must also consider bystanders in addition to the wearer.

---

# 41. Optical safety

Any architecture using laser illumination requires a formal optical-radiation safety program.

IEC TS 60825-20:2025 specifically addresses products intentionally exposing the face or eyes to laser radiation, including AR/VR-type products.

This repository therefore keeps optical control and safety boundaries separate.

Simulation code must not directly drive a real laser source.

---

# 42. Current research status

The repository is deliberately honest about maturity.

### Exists now

- semantic Intent/Action abstractions;
- context/world/memory representations;
- basic personal-agent loop;
- BCI signal interfaces;
- dataset manifests/utilities;
- compute-node abstraction;
- workload routing;
- trajectory representation;
- optical-field simulation primitives;
- research dependency map.

### Under active research

- learned EEG representations;
- robust EMG decoding;
- multimodal intent fusion;
- user-specific adaptation;
- offline policy learning;
- context relevance;
- compute migration;
- hardware-independent capability APIs;
- camera-in-the-loop optical optimization;
- optical-human co-design.

### Not yet solved

- arbitrary thought-to-command BCI;
- final optical architecture;
- final laser/PIC architecture;
- final prescription-compatible hardware;
- production silicon;
- all-day final battery/thermal envelope;
- medical/clinical efficacy claims;
- universal cross-user neural decoding.

This distinction is essential.

---

# 43. The final product architecture

The eventual system is intended to look conceptually like:

```text
                         NEXUS ENVIRONMENT
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
      PERSONAL AI            MEMORY               CONTEXT
          │                     │                     │
          └─────────────────────┼─────────────────────┘
                                │
                          INTENT FABRIC
                                │
                          CAPABILITY GRAPH
                                │
                          COMPUTE FABRIC
                                │
             ┌──────────────────┼──────────────────┐
             │                  │                  │
          GLASSES             PHONE              PC
             │                  │                  │
             │             OTHER DEVICES        RESERVOIR
             │                  │                  │
             └──────────────────┼──────────────────┘
                                │
                             HUMAN
```

And the physical wearable becomes:

```text
      HUMAN
        │
   EEG / EMG / Gaze
        │
     perception
        │
      intent
        │
   personal agent
        │
   compute fabric
        │
 optical presentation
        │
        eye
        │
     perception
        │
     feedback
```

---

# 44. Development principles

### 1. Build the abstraction before the hardware

The hardware exists to execute the architecture, not to define it.

### 2. Reproduce before claiming novelty

A known result that works under your control is more valuable than an unsupported assertion of originality.

### 3. Measure every important claim

Latency, accuracy, energy, interaction burden, uncertainty and failure rate belong in the repository.

### 4. Preserve failure cases

A failed experiment is part of the technical record.

### 5. Use established infrastructure aggressively

Do not reinvent signal acquisition, EEG preprocessing, wave propagation, synchronization or standard XR interfaces unless there is a measured reason.

### 6. Own the semantic layer

The long-term platform value is in the abstraction that connects human intent to computation.

### 7. Separate learned systems from hard safety constraints

Learning can optimize; deterministic policy can constrain.

### 8. Keep the platform hardware-independent early

The same intent should work across keyboard, voice, EEG, EMG, gaze and future hardware.

---

# 45. Public technical references

### Human-computer / platform

- [NVIDIA CUDA](https://developer.nvidia.com/cuda)
- [Khronos OpenXR](https://www.khronos.org/openxr/)
- [Android XR developer documentation](https://developer.android.com/develop/xr)
- [Android XR OpenXR](https://developer.android.com/develop/xr/openxr)

### BCI / biosensing

- [BrainFlow](https://github.com/brainflow-dev/brainflow)
- [MNE-Python](https://github.com/mne-tools/mne-python)
- [Braindecode](https://github.com/braindecode/braindecode)
- [MOABB](https://github.com/NeuroTechX/moabb)
- [LabStreamingLayer](https://github.com/sccn/labstreaminglayer)
- [PhysioNet EEGMMIDB](https://physionet.org/content/eegmmidb/1.0.0/)
- [BCI Competition IV](https://www.bbci.de/competition/iv/)
- [Meta emg2pose](https://github.com/facebookresearch/emg2pose)
- [NinaPro](https://ninapro.hevs.ch/)
- [Gest-Infer](https://github.com/HumanMachineInterface/Gest-Infer)

### Computational optics / photonics

- [Neural Holography](https://github.com/computational-imaging/neural-holography)
- [Synthetic Aperture Waveguide Holography](https://github.com/choisuyeon/sawh)
- [Diffractsim](https://github.com/rafael-fuente/diffractsim)
- [MEEP](https://github.com/NanoComp/meep)
- [RCWA](https://github.com/edmundsj/rcwa)

---

# 43A. Current external landscape

Nexus is being developed against a rapidly moving ecosystem. The repository does not assume that the surrounding industry is static or empty.

- [NVIDIA CUDA](https://developer.nvidia.com/cuda) provides a useful reference for the kind of compiler/runtime/library/tooling ecosystem that can create durable hardware-software coupling.
- [Android XR OpenXR](https://developer.android.com/develop/xr/openxr) currently documents hand interaction, eye-gaze interaction, 6DoF controllers and mouse interaction as supported input modalities.
- [Android XR intelligent eyewear input](https://developer.android.com/develop/xr/jetpack-xr-sdk/input) states that voice is the primary interaction path for audio and display glasses.
- Google is explicitly building intelligent eyewear around Android XR and Gemini, spanning audio and display glasses. See [Google I/O 2026](https://blog.google/products-and-platforms/platforms/android/android-xr-io-2026/).
- Meta is simultaneously developing AI glasses, display glasses and EMG-based input, while also publishing private-processing architecture for personal-context workloads. See [Meta Engineering: Private Processing for AI Glasses](https://engineering.fb.com/2026/09/23/security/private-processing-meta-ai-glasses/).

The architectural conclusion is not that existing platforms are incapable of adding new peripherals. The opportunity is to make **human intent, BCI, context, capability, policy and compute placement first-class abstractions**, rather than device-specific add-ons.

---

# 43B. Human-made algorithm principle

Nexus algorithms should be understandable as engineering systems before they are optimized as machine-learning systems.

For each subsystem, the repository should be able to answer:

```text
What is observed?
What state is estimated?
What representation is produced?
What uncertainty exists?
What action is proposed?
What constraint can veto it?
What outcome is measured?
What is learned?
```

The preferred architecture is therefore:

```text
physics / signal model
        ↓
explicit representation
        ↓
learned estimator where useful
        ↓
policy / optimization
        ↓
hard constraints
        ↓
actuation
        ↓
measurement
```

Machine learning should compress or approximate difficult mappings; it should not hide the system's semantics.

The repository should prefer small, testable algorithms and explicit interfaces before large end-to-end black boxes.

---

# 43C. Architecture invariants

The following should remain true as implementations change:

1. **Intent is modality-independent.**
2. **Capabilities are provider-independent.**
3. **User state is terminal-independent.**
4. **Compute placement is workload-dependent.**
5. **Safety constraints can veto learned behavior.**
6. **Personal memory is user-controlled.**
7. **Optical hardware is controlled through measured state, not blind commands.**
8. **Every major claim has an experimental metric.**
9. **External dependencies are replaceable by contract.**
10. **The final hardware must serve the software architecture, not define it.**

---

# 43D. The central research object

The project ultimately studies a closed loop:

\[
oxed{
	ext{Human}\rightarrow	ext{Intent}\rightarrow	ext{Capability}\rightarrow	ext{Compute}\rightarrow	ext{Action}\rightarrow	ext{Feedback}
}
\]

conditioned by:

\[
oxed{
	ext{Context}+	ext{Memory}+	ext{World}+	ext{User Model}
}
\]

and, for a future optical terminal:

\[
oxed{
	ext{Intent}+	ext{Gaze}+	ext{World}\rightarrow	ext{Adaptive Optical State}\rightarrow	ext{Human Perception}
}
\]

The engineering value of the repository is therefore not a collection of disconnected demos. It is the progressive conversion of these relationships into **measurable, executable, inspectable system primitives**.

---

# 46. The research question at the center of the repository

The entire project can eventually be reduced to one question:

> **Can a computing system become sufficiently aware of human intent, personal context, available capabilities and physical constraints that the user can interact with computation through natural intent rather than through device-specific interaction procedures?**

The system-level extension is:

> **Can that same architecture continuously personalize itself, place computation across heterogeneous resources, and present information through an adaptive optical interface without imposing unacceptable latency, energy, safety, privacy or cognitive costs?**

These are research questions, not assumptions of success.

---

# 47. The engineering target

The long-term engineering target is:

\[
\boxed{
\text{Human signals}
\rightarrow
\text{Intent}
\rightarrow
\text{Personal agent}
\rightarrow
\text{Capability}
\rightarrow
\text{Compute}
\rightarrow
\text{Action}
\rightarrow
\text{Feedback}
}
\]

with:

\[
\boxed{
\text{Context + Memory + World state}
}
\]

conditioning every stage.

The optical extension is:

\[
\boxed{
\text{Intent + World + Gaze}
\rightarrow
\text{Adaptive optical field}
\rightarrow
\text{Human perception}
\rightarrow
\text{feedback}
}
\]

The result is not merely an assistant and not merely an AR system.

It is a proposed architecture for **personal computing in which the user, environment and computational resources are part of one continuous system**.

---

# 48. Repository status

This project is research software.

It is not a medical device, clinical BCI, finished consumer OS, or production laser controller.

Results must be distinguished as:

```text
reproduced
measured
hypothesized
simulated
prototype
validated
production-ready
```

No stronger label should be used without evidence.

---

# 49. The standard for Nexus engineering

The project aims for a particular kind of depth:

> **Understand the governing mechanism, compress it to its essential variables, build the smallest working model, measure the bottleneck, and then attack that bottleneck.**

For every subsystem:

\[
\boxed{
\text{physics / signal}
\rightarrow
\text{representation}
\rightarrow
\text{inference}
\rightarrow
\text{control}
\rightarrow
\text{system}
}
\]

That is the development language of this repository.

The eventual product may be physically small.

The architecture underneath it is not.

---

## License

The Nexus repository's own source code should carry an explicit project license once the ownership/IP strategy is finalized. Third-party repositories remain subject to their original licenses and attribution requirements.

## Contact

Technical collaboration, research discussion, reproducibility work, and engineering contributions are welcome through the repository issue/discussion system once the public repository is established.
