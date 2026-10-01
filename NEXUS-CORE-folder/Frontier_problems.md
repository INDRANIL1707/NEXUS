# NEXUS: Frontier Problems in Closed-Loop Wearable Human–Computer Interaction

*Near-eye optics, non-invasive neuromotor sensing, state estimation, and context-aware compute placement: a research assessment and falsifiable program*

| | |
|---|---|
| **Author** | Indranil Banarjee |
| **Version** | 1.0, working draft (not peer reviewed)
| **Date** | 10 September 2026 |
| **References** | Entries marked † were re-checked against publisher or index records on 1 October 2026; see the note preceding the reference list |

---

## Abstract

NEXUS is a proposed human–computer architecture in which optical see-through eyewear, multimodal physiological sensing, a persistent state estimator, and a distributed compute runtime operate as a single closed loop. This document is not a product specification. It (i) formalises the loop as a partially observable decision process; (ii) identifies, for each of four coupled subsystems, the physical or statistical constraint that bounds what is achievable; (iii) separates what the published evidence establishes from what NEXUS only hypothesises; and (iv) states five falsifiable hypotheses with refutation criteria fixed in advance.

Two conclusions shape the program. First, current evidence supports peripheral neuromotor sensing (surface EMG) and ocular signals as the dependable input channels, whereas scalp EEG has not been shown to add command bandwidth under naturalistic, eyewear-compatible conditions. The architecture is therefore designed so that a negative EEG result leaves the system intact, and that result is itself informative. Second, the binding constraints are cross-layer: optical efficiency, power, and latency budgets couple every subsystem, so the program starts an optical budget model at the outset rather than last.

## How to read this document

Claims carry one of four tags.

- **[E] Established.** Peer-reviewed result, or derivable from the equations stated alongside it.
- **[I] Inference.** Follows from [E] claims under stated assumptions; could be wrong if an assumption fails.
- **[H] Hypothesis.** Testable and untested here. Section 8 defines the tests.
- **[S] Speculation.** Plausible, no supporting evidence offered.

Untagged sentences are definitions or structure. Figures reported only by a vendor are labelled as such. Worked numerical examples use assumed inputs, which are stated; they illustrate orders of magnitude and are not predictions.

---

## 1. Problem statement

### 1.1 System and research question

The system under study consists of eyewear with an optical see-through display; an on-body sensing set (cameras, microphones, IMU, eye tracking, peripheral biopotential electrodes, optionally scalp EEG); companion devices (a wrist sEMG band, earphones, a phone); and remote compute.

> **Research question.** Can a compact, wearable, distributed system maintain estimates of user intent and physical context accurate enough to support reliable real-time interaction, without implants and without continuous explicit input, within realistic limits on power, latency, and visual comfort?

### 1.2 Formal model

The interaction is modelled as a partially observable Markov decision process $(\mathcal S,\mathcal A,\mathcal O,T,Z,\mathcal R)$ [Kaelbling1998]. The latent state factorises as

$$
s_t=(w_t,\,u_t,\,g_t,\,d_t,\,r_t),
$$

the world, the user, the active task or goal, the connected devices, and the available compute resources. The system receives a heterogeneous observation

$$
o_t=\big(o^{\mathrm{vis}}_t,\,o^{\mathrm{aud}}_t,\,o^{\mathrm{eeg}}_t,\,o^{\mathrm{emg}}_t,\,o^{\mathrm{eog}}_t,\,o^{\mathrm{gaze}}_t,\,o^{\mathrm{imu}}_t,\,o^{\mathrm{sys}}_t\big)
$$

and maintains a belief $b_t(s)=\Pr(s_t=s\mid o_{0:t},a_{0:t-1})$, updated by

$$
b_{t+1}(s')=\frac{Z(o_{t+1}\mid s',a_t)\,\sum_{s}T(s'\mid s,a_t)\,b_t(s)}{\Pr(o_{t+1}\mid b_t,a_t)} .
$$

**[I]** At this scale the exact update is intractable, so $b_t$ is replaced by a learned latent state $z_t$ that must act as an approximate sufficient statistic for decision-making. Much of the difficulty of NEXUS lies in how good that approximation can be made with weak, non-stationary sensors.

The quantity that couples sensing to action is the **interaction intent** $y_t\in\mathcal Y$, a discrete or structured variable (select, confirm, dismiss, dictate, and so on) that always includes an explicit *null* class. The system estimates

$$
p\big(y_t\mid o_{0:t},\,c_t,\,m_t\big),
$$

where $c_t$ is context (task, gaze target, scene, recent history) and $m_t$ is persistent memory. A policy $\pi$ then maps belief and memory to an action. The closed loop is

$$
o_t\;\longrightarrow\;b_t\;\longrightarrow\;\pi\;\longrightarrow\;a_t\;\longrightarrow\;o_{t+1}.
$$

The estimand is a probability over interaction intents, not a reading of thoughts. Nothing in this document claims general decoding of mental content.

### 1.3 Program assumptions and non-goals

These are design choices, not findings.

- Optical see-through eyewear; no video-passthrough or occluding display.
- Peripheral companion sensors (wrist, ear) are permitted. No implants.
- Target of at least 10 h of operation per charge **[H]**; see §7.2.
- Raw biosignals are processed on-device by default.

Non-goals: thought decoding; a new operating-system kernel; a consumer product specification.

### 1.4 Notation

| Symbol | Meaning |
|---|---|
| $w_t,\,u_t,\,g_t,\,d_t,\,r_t$ | world, user, task/goal, device, and compute-resource state |
| $o_t$, $o^{(k)}_t$ | observation vector; its component for modality $k$ |
| $a_t$ | system action (render, query, schedule, actuate) |
| $y_t$ | interaction intent, including the null class |
| $c_t$, $m_t$ | context features; persistent memory |
| $z_t$ | learned latent state |
| $I(\cdot\,;\cdot)$ | mutual information, in bits |
| $\eta$, $\Theta$, $A_{\mathrm{eb}}$ | optical efficiency, field of view, eyebox area |

---

## 2. State of the art and positioning

### 2.1 What demonstrably exists

- **Display eyewear with a neuromotor wristband reached consumers in September 2025.** Meta's Ray-Ban Display pairs a monocular in-lens display with an sEMG wristband. Launch coverage reports a 600 × 600 display, 20° field of view, about 42 pixels per degree, up to 5,000 nits peak brightness, and roughly six hours of mixed use (vendor-reported) [Meta2025]. **[E]** for existence; the specifications are vendor figures.
- **Generic sEMG decoding generalises across people.** Kaifosh et al. trained models on data from thousands of participants and report closed-loop median performance of 0.66 target acquisitions per second in continuous navigation, 0.88 gesture detections per second in a discrete-gesture task, and handwriting at 20.9 words per minute, with a further 16% handwriting improvement from personalisation [Kaifosh2025]†. **[E]**
- **Invasive speech neuroprostheses reach conversational rates** (on the order of 60–80 words per minute in implanted participants) [Willett2023]† [Metzger2023]†. This is the reference against which non-invasive decoding should be judged. **[E]**

### 2.2 Prior art behind the NEXUS ideas

| NEXUS idea | Closest prior art | Consequence for this work |
|---|---|---|
| Context narrows the intent space | Shared control of brain-actuated devices [Millan2004]; hybrid BCIs [Pfurtscheller2010] | The qualitative idea is not new. What is new would be *measuring* the context gain for wearable multimodal decoding (H2). |
| Confidence is not authorisation | Mixed-initiative decision theory [Horvitz1999] | Derivation in §4.6; the contribution is its integration with capabilities (§6.5). |
| Passive neural state as input | Passive BCI [Zander2011] | A more plausible role for EEG than command entry (§4.3). |
| Offload from wearable to nearby/edge compute | Cloudlets [Satyanarayanan2009]; wearable cognitive assistance [Ha2014]; code offload [Cuervo2010] | Placement is a known problem; the NEXUS variant adds privacy and authorisation constraints (§6.3). |
| Scoped, expiring permissions | Capabilities [DennisVanHorn1966]; macaroons [Birgisson2014] | Adopt rather than reinvent. |
| Ambient, context-aware computing | [Weiser1991]; [Dey2001] | Defines the lineage NEXUS belongs to. |
| Predictive latent world models | [LeCun2022]; V-JEPA 2 [Assran2025] | Demonstrated for robot manipulation, not for open-world human context (§5.1). |

### 2.3 What NEXUS can credibly claim as new

**[I]** No single idea above is new. The defensible contribution is integration plus measurement:

1. a typed intent object carrying calibrated uncertainty into a cost-sensitive authorisation policy;
2. a quantified measurement of how much context lowers the neural signal quality needed for a target accuracy, in a wearable, natural-motion setting;
3. an executable runtime in which placement and authorisation are decided together.

Each is testable (Section 8).

### 2.4 Maturity by component

| Component | Maturity | Strongest evidence | Binding gap |
|---|---|---|---|
| HUD-class display eyewear (~20°, monocular) | Commercial (2025) | Vendor specifications | Efficiency, brightness–power, comfort |
| Wide-FOV full-colour waveguide (>45°) | Laboratory | [Tian2025]†, [Moon2025]† | Efficiency, uniformity, manufacturability |
| Multi-depth / varifocal optics | Laboratory | [Shin2025]† | Thickness, switching, eye tracking |
| Wrist sEMG decoding | Validated at scale | [Kaifosh2025]† | Vocabulary, latency, user variability |
| Facial / silent-speech EMG | Early research | [Tang2026]† | Repositioning and session robustness |
| Scalp EEG intent, wearable, naturalistic | Research; weak evidence for command | [Ding2025]†, [dAscoli2025]† | SNR, artefacts, limited scalp access on eyewear |
| Latent world models | Advancing quickly | [Assran2025] | Open-world, long-horizon prediction |
| Compute placement | Mature primitives | [Satyanarayanan2009], [Ha2014] | Semantic workloads, privacy |

---

## 3. Frontier A: near-eye optics

### 3.1 The coupled variables

The optical engine must route light from a source to the retina while preserving see-through vision. The relevant variables are field of view $\Theta$, eyebox $A_{\mathrm{eb}}$, eye relief, luminance, efficiency $\eta$, modulation transfer function (MTF), distortion, chromaticity, world transmission, focus range, mass, thermal load, and latency. The difficulty is the coupling: improving one variable usually degrades another [Ding2023]† [KressChatterjee2021]†.

### 3.2 Étendue and the efficiency floor

Étendue is $G=n^{2}\!\iint\cos\theta\,\mathrm{d}A\,\mathrm{d}\Omega\approx n^{2}A\Omega$ in the small-angle limit, and it cannot decrease in a lossless passive system **[E]**. The eye accepts light from one position through its pupil, but the image must remain visible as the eye moves, so the display must effectively fill the whole eyebox:

$$
G_{\mathrm{display}}\;\gtrsim\;A_{\mathrm{eb}}\,\Omega_{\mathrm{FOV}},\qquad
G_{\mathrm{eye}}\approx A_{\mathrm{pupil}}\,\Omega_{\mathrm{FOV}}.
$$

If the emitted light is spread uniformly over the eyebox, the fraction entering the pupil at any instant is bounded by

$$
\eta_{\mathrm{geo}}\;\lesssim\;\frac{A_{\mathrm{pupil}}}{A_{\mathrm{eb}}}.
$$

**Worked example [I], assumed inputs.** A 4 mm pupil ($12.6\ \mathrm{mm^2}$) and a $10\times8\ \mathrm{mm}$ eyebox ($80\ \mathrm{mm^2}$) give $\eta_{\mathrm{geo}}\lesssim 0.16$. At least 84% of the light is lost geometrically before any coupler, absorption, or polarisation loss. Pupil diameter varies with ambient light, so the value is not constant.

**[I]** Usable brightness at the eye can therefore be obtained in only two ways: a smaller eyebox (which requires pupil tracking or steering) or more source power, which stresses the thermal budget. Daylight legibility raises the luminance requirement, and low efficiency converts it directly into electrical power.

### 3.3 A field-of-view bound from waveguide k-space

Consider a one-dimensional in-coupling grating of period $\Lambda$ in air feeding a waveguide of index $n_w$. First-order diffraction maps an incident ray at angle $\theta_i$ to a guided angle $\theta_w$:

$$
n_w\sin\theta_w=\sin\theta_i+\frac{\lambda}{\Lambda}.
$$

Guided propagation requires total internal reflection and $\theta_w<90^\circ$, that is $1<n_w\sin\theta_w<n_w$. A full field of view $\Theta$ spans $\sin\theta_i\in[-\sin\tfrac{\Theta}{2},\,+\sin\tfrac{\Theta}{2}]$, an interval of width $2\sin\tfrac{\Theta}{2}$, which must fit inside a guided interval of width $n_w-1$:

$$
\Theta_{\max}=2\arcsin\!\left(\frac{n_w-1}{2}\right).
$$

| $n_w$ | 1.5 | 1.8 | 2.0 | 2.3 |
|---|---|---|---|---|
| $\Theta_{\max}$ (horizontal, upper bound) | 29.0° | 47.2° | 60.0° | 81.1° |

This is an upper bound **[E, derived]** for a single-plate, single-axis system with air cladding, ignoring total-internal-reflection margins, material dispersion, and the output coupler. Practical values are lower. Waveguide refractive index, not display resolution, sets the first-order ceiling on field of view, which explains the push toward high-index substrates. The reported achromatic metasurface waveguide, for example, operates across a maximum field of view exceeding 45° on a high-index plate [Tian2025]†, consistent with the bound.

### 3.4 Chromatic dispersion and recent work

Because $\lambda/\Lambda$ shifts the guided window with wavelength, red and blue light are guided over different input-angle windows. Neglecting material dispersion, the full-colour field of view of an uncompensated single-layer grating satisfies

$$
2\sin\tfrac{\Theta_{\mathrm{RGB}}}{2}\;\le\;(n_w-1)-\frac{\lambda_R-\lambda_B}{\Lambda}.
$$

Colour channels therefore either need separate plates (mass and loss) or couplers whose deflection is wavelength independent. Recent bench demonstrations address this at different points in the trade space.

- Inverse-designed metasurface couplers on a high-index waveguide reported achromatic behaviour across the waveguide's supported field of view, with a prototype showing improved colour accuracy and uniformity [Tian2025]†.
- A single-layer waveguide using achromatic metagratings diffracts red, green, and blue in the same direction [Moon2025]†.
- Metasurface waveguides combined with learned holographic calibration produced full-colour 3D holographic AR images [Gopakumar2024]†.
- Polarisation volume gratings exploit an off-Bragg polarisation conversion to circumvent the trade-off between in-coupling efficiency and eyebox uniformity; the authors report roughly twofold efficiency and 2.3-fold uniformity gains over conventional couplers [Ding2024]†.

**[I]** These are laboratory demonstrations. None shows a manufacturable, efficient, wide-field full-colour waveguide at eyeglass mass. Known open issues include diffractive artefacts from ambient light passing through the gratings [KressChatterjee2021]†, yield, and cost.

### 3.5 Focus and visual comfort

For stereoscopic displays, the vergence–accommodation conflict is quantified in dioptres:

$$
\Delta D=\left|\frac{1}{d_{\mathrm{vergence}}}-\frac{1}{d_{\mathrm{focal}}}\right| .
$$

A virtual image at fixed focal distance 2 m ($0.5\ \mathrm{D}$) rendered with content at 0.5 m ($2\ \mathrm{D}$) gives $\Delta D=1.5\ \mathrm{D}$. Mismatch of this size is associated with visual fatigue and reduced performance [Hoffman2008], and comfort zones are on the order of a few tenths of a dioptre [Shibata2011]. **[E]**

Two points matter for NEXUS.

1. **[I]** For a *monocular* display there is no binocular disparity cue, so the stereoscopic conflict does not arise as such. The eye must nevertheless re-accommodate between the virtual image plane and the real task plane, and the display's relationship to the other eye's view can itself cause discomfort. Visual comfort remains an open measurement problem, not a solved one.
2. If multiple focal states are provided, a uniform spacing of $\delta=2\varepsilon$ in dioptres keeps the residual mismatch within a tolerance $\varepsilon$. Covering a range $\Delta D_{\mathrm{range}}$ needs $N_f=\lceil\Delta D_{\mathrm{range}}/\delta\rceil+1$ states. For $0$–$4\ \mathrm{D}$ (infinity to 25 cm) at $\delta=0.3\ \mathrm{D}$ this is $N_f=15$.

Candidate architectures include fixed focus, switchable multi-plane, gaze-contingent varifocal, and holographic wavefront control. A polarisation-controlled geometric-phase-lens design demonstrated multi-depth switching from about 24 cm to infinity in the laboratory [Shin2025]†. Gaze-contingent adaptive focus has been shown to improve performance in user studies [Padmanaban2017], and holographic near-eye displays are an active line of work [Maimone2017].

Comfort should be measured, not assumed, using validated instruments: the Simulator Sickness Questionnaire [Kennedy1993] and the Computer Vision Syndrome Questionnaire [Segui2015], alongside task-load measurement (NASA-TLX [Hart1988]).

### 3.6 Retinal and laser projection: a safety-first constraint

Scanned-laser and Maxwellian-type designs can be compact because the image is formed by controlling rays rather than by placing a panel at a focal plane. They concentrate light through a small exit pupil, so they typically need pupil tracking to keep the image visible [Jang2017].

The eye focuses a collimated beam to a small retinal spot, so apparent brightness is not a proxy for retinal hazard **[E]**. Applicable frameworks include IEC 60825-1, ANSI Z136.1, and the ICNIRP guidelines on laser exposure limits [IEC60825], [ANSI136], [ICNIRP2013]. In the United States, laser products fall under the FDA's radiation-control regulations (21 CFR 1040.10), and the FDA maintains a product code for non-medical retinal-image laser displays, currently unclassified [FDAREE]†.

**[I]** Design consequences if a laser engine is pursued:

- the optical-power limit must be enforced by hardware that is independent of the software stack;
- pupil-tracking failure must default to a safe state, because tracking latency becomes safety-relevant;
- a single-fault hazard analysis must precede any high-power prototype.

### 3.7 Objectives

| ID | Objective | Output |
|---|---|---|
| O1 | End-to-end optical budget model for each candidate architecture | Vector $\{\Theta,\,A_{\mathrm{eb}},\,\eta,\,L,\,\mathrm{MTF},\,\text{distortion},\,\Delta u'v',\,T_{\mathrm{world}}\}$; no architecture is accepted on a single metric |
| O2 | Efficiency–FOV frontier at fixed eyebox | Pareto curve of $\eta$ against $\Theta$ |
| O3 | Comfort under realistic use | SSQ, CVS-Q, NASA-TLX over 1 h and 4 h task blocks |
| O4 | Hazard analysis | Completed before any laser-class hardware is built |

---

## 4. Frontier B: non-invasive neural and neuromotor sensing

### 4.1 Measurement physics and what eyewear can reach

A scalp EEG measurement is modelled as

$$
\mathbf x(t)=\mathbf L\,\mathbf j(t)+\mathbf n(t),
$$

where $\mathbf j(t)$ are distributed cortical sources, $\mathbf L$ is the lead-field (volume-conduction) operator, and $\mathbf n(t)$ is noise. Recovering $\mathbf j$ from $\mathbf x$ is an ill-posed inverse problem with non-unique solutions [Baillet2001]. **[E]** In a wearable setting the measurement is further contaminated:

$$
\mathbf x=\mathbf x_{\mathrm{brain}}+\mathbf x_{\mathrm{ocular}}+\mathbf x_{\mathrm{muscle}}+\mathbf x_{\mathrm{motion}}+\mathbf x_{\mathrm{env}}+\mathbf n .
$$

Cortical EEG is microvolt-scale, whereas muscle and eye-movement potentials are larger by orders of magnitude, and electrode-motion artefacts can exceed the biosignal by an order of magnitude [Seok2021]†. **[E]** Improving a classifier does not remove these terms. The levers are the electrode–skin interface, reference channels for the artefact terms, and the decoder's training data.

**An eyewear-specific constraint [I].** Frame-mounted electrodes can reach temporal, frontal-polar, nose-bridge, and post-auricular or ear sites; ear-centred EEG has been demonstrated as a concealed, unobtrusive form factor [Debener2012] [Bleichner2017]. The sensorimotor sites used by motor-imagery paradigms and the occipital sites used by SSVEP paradigms are not reachable without a headband. Results from full-cap laboratory studies therefore do not transfer to eyewear by default. How much cap-level information survives restriction to an eyewear-accessible montage is a direct, testable question (H1, control iv).

### 4.2 The evidence boundary

- **Word decoding from EEG/MEG** [dAscoli2025]†. A single architecture was evaluated on seven public datasets and two new ones, totalling 723 participants, three languages, and about five million words. Decoding was of language *perception* (reading or listening), not production; word-onset times were known; scoring was retrieval among the 250 most frequent words, with top-10 accuracy up to 37% against a 4% chance level. MEG decoded better than EEG, and reading better than listening. Subject-specific layers prevented zero-shot transfer to new participants. Averaging repeated responses raised accuracy substantially, which is not available in real-time use. For a 50-word vocabulary the authors report top-1 accuracy of 20%, against 39.5% for an implanted decoder in an earlier study. The pipeline performed no artefact removal. **[E]**
- **EEG finger-level robotic control** [Ding2025]†. Twenty-one able-bodied, experienced BCI users controlled robotic fingers in real time by motor imagery; reported accuracies were 80.56% (two fingers, chance 50%) and 60.61% (three fingers, chance 33%). **[E]**
- **Wrist sEMG** [Kaifosh2025]†: see §2.1. **[E]**
- **Silent speech.** A 2026 review concludes that on-body systems currently offer the best balance of accuracy and deployability, with in-body approaches giving unmatched neural access where articulation is lost entirely [Tang2026]†. **[E]**
- **Evaluation hygiene.** Data leakage has been observed in many language-decoding papers [Jo2024]†, and the word-decoding study above takes explicit steps to avoid it by splitting on stimulus. **[E]**

**Assessment [I].** Open-vocabulary language *production* from non-invasive EEG is not near deployment. Peripheral channels are the nearer route. For perspective on EEG command bandwidth, the information transfer per decision under the standard Wolpaw formula [Wolpaw2002],

$$
B=\log_2 N+P\log_2 P+(1-P)\log_2\frac{1-P}{N-1}\quad\text{bits per selection},
$$

assuming equiprobable targets and uniformly distributed errors, gives:

| Case | $N$ | $P$ | $B$ (bits/selection) |
|---|---|---|---|
| Two-finger motor imagery [Ding2025] | 2 | 0.8056 | 0.29 |
| Three-finger motor imagery [Ding2025] | 3 | 0.6061 | 0.22 |
| Illustrative four-target interface | 4 | 0.80 | 0.96 |

These are best-case, laboratory, full-cap, experienced-user figures, and $B$ ignores the null state; false-activation rate must be reported separately (§4.6).

### 4.3 Multimodal intent estimation

Under conditional independence of modalities given $y_t$ and $c_t$,

$$
p(y_t\mid o_t,c_t)\;\propto\;p(y_t\mid c_t)\prod_{k}p\big(o^{(k)}_t\mid y_t,c_t\big).
$$

**[E]** The independence assumption is false for artefact-coupled channels: EEG is contaminated by EOG and EMG. A learned fusion model with explicit artefact covariates is required in place of the naive product.

| Channel | What it informs | Principal failure mode |
|---|---|---|
| Gaze and vision | Target of attention | Unintended selection ("Midas touch") [Jacob1990] |
| EOG | Eye-movement events at low power | Drift; coarse spatial resolution |
| Wrist sEMG | Discrete gestures, continuous control, handwriting | Vocabulary limits; user variability |
| Facial/jaw EMG | Silent commands | Repositioning across sessions |
| Voice | Open-vocabulary content | Social acceptability; noise |
| EEG | State (attention, workload); possibly error detection | Low SNR; limited access on eyewear; artefact leakage |
| IMU | Head motion; artefact reference | None critical |

Two roles for EEG deserve explicit tests. One is passive state monitoring, as in passive BCI [Zander2011]. The other is error-related potentials, which could flag that the system's action was wrong and trigger correction [Chavarriaga2010]. **[H]** Both are more plausible than command entry on the current evidence, and neither requires the sensorimotor montage.

The interface object between the neural/physiological stack and the runtime should be a distribution, not a command:

```json
{
  "type": "IntentHypothesis",
  "timestamp": "2026-10-01T09:30:00.250Z",
  "valid_for_ms": 300,
  "posterior": {"select": 0.62, "dismiss": 0.05, "none": 0.33},
  "target": {"id": "obj_17", "p": 0.81, "source": "gaze+vision"},
  "modalities": {"emg_wrist": "ok", "eog": "ok", "eeg": "degraded"},
  "calibration": {"decoder": "d_2026_09_28", "ece_last_session": 0.04}
}
```

All values are illustrative. The posterior sums to one and includes the null class; quality flags travel with the hypothesis.

### 4.4 Measuring the information EEG adds

By the chain rule,

$$
I(Y;O\mid C)=I\big(Y;O_{-\mathrm{eeg}}\mid C\big)+I\big(Y;O_{\mathrm{eeg}}\mid O_{-\mathrm{eeg}},C\big).
$$

The EEG increment is the conditional mutual information in the second term. Two issues follow.

1. Conditional mutual information is non-negative, and estimators are positively biased in finite samples, so testing "increment $>0$" is not informative. A minimum effect size $\delta_1$, fixed before data collection, is required.
2. In practice the increment is estimated from held-out log-likelihood:

$$
\widehat{\Delta\ell}=\frac1N\sum_{i=1}^{N}\Big[\log_2 q_{\mathrm{full}}(y_i\mid x_i)-\log_2 q_{\mathrm{base}}(y_i\mid x^{-\mathrm{eeg}}_i)\Big].
$$

Each term lower-bounds the corresponding mutual information up to $H(Y)$, so their difference is **not** a bound on the true increment. It is a decoder-relative gain: keep the model class and capacity identical across arms, block the cross-validation by session, and report confidence intervals.

**Controls required to attribute any gain to cortical activity [E, design requirement].**

1. EEG after regression of EOG, EMG, and IMU reference channels;
2. frontal-polar-only versus temporal/ear-only EEG (the former is ocular-dominated);
3. time-shuffled and label-shuffled EEG surrogates;
4. full-cap montage versus eyewear-accessible montage.

If control (i) eliminates the gain, the gain was not neural.

### 4.5 Calibration, drift, and personalisation

Electrode position, physiology, fatigue, and attention change between sessions, so the class-conditional distribution drifts: $p_t(x\mid y)\neq p_{t+\Delta}(x\mid y)$ [Shenoy2006]. **[E]** A generic decoder with a regularised personal adaptation is a natural structure:

$$
\theta_u=\theta_0+\Delta_u,\qquad
\Delta_u^{*}=\arg\min_{\Delta}\;\mathcal L_u(\theta_0+\Delta)+\lambda\,\lVert\Delta\rVert_2^2 ,
$$

where $\theta_0$ are generic weights, $\mathcal L_u$ is a loss on the user's recent data, and $\lambda$ trades plasticity against stability so that adaptation does not drift without bound. The sEMG result supports this structure: generic models generalise across people, and personalisation adds a further 16% on handwriting [Kaifosh2025]†. For EEG, subject-specific layers prevented zero-shot transfer in the largest word-decoding study [dAscoli2025]†. Pretrained EEG foundation models are an emerging direction [Jiang2024]; **[I]** evidence on their naturalistic cross-session robustness is thin.

### 4.6 Action thresholds and false activations

Let the system choose among *execute*, *ask*, and *ignore*. With $p$ the posterior of the intended action, $B$ the benefit of a correct execution, $C$ the cost of an erroneous one (including undo), and $c_q$ the cost of a confirmation:

$$
U_{\mathrm{exec}}=pB-(1-p)C,\qquad U_{\mathrm{ask}}=pB-c_q,\qquad U_{\mathrm{ignore}}=0 .
$$

Execute beats ask when $p>1-c_q/C$, and ask beats ignore when $p>c_q/B$. **[E, derived]** The consequence is that **the authorisation threshold depends on $C$, not only on the classifier**: confidence is never sufficient by itself. This is the decision-theoretic content of mixed-initiative interaction [Horvitz1999]. It also requires probabilities that are calibrated, so expected calibration error [Guo2017] and strictly proper scores [Gneiting2007] must be reported with accuracy.

Always-on gaze or intent systems face the Midas-touch problem: looking at something is not requesting it [Jacob1990]. The relevant performance measure is the false-activation rate per hour, FAR. With zero observed false activations over $T$ hours, the 95% upper confidence bound on FAR is approximately $3/T$ (the rule of three), so demonstrating $\mathrm{FAR}\le0.1\ \mathrm{h^{-1}}$ requires about 30 hours of null-class operation per condition.

---

## 5. Frontier C: state estimation, world models, and agency

### 5.1 Latent state and predictive dynamics

A joint-embedding predictive approach learns a latent state $z_t=f_\theta(o_{\le t},a_{<t})$ and predictive dynamics

$$
\hat z_{t+1}=g_\phi(z_t,a_t),
$$

trained against stop-gradient target embeddings rather than reconstructing pixels [LeCun2022]. V-JEPA 2 shows that self-supervised video pretraining supports visual understanding, anticipation, and, with an action-conditioned stage, planning on tabletop robot manipulation [Assran2025]. **[E]**

**[I]** Two limits apply to NEXUS. First, a robot's actions are observable and its world is largely physical, whereas the user's internal state $u_t$ and goal $g_t$ are latent with no ground truth except behaviour and feedback. Second, manipulation demonstrations do not establish long-horizon prediction of human context in open environments. Evaluation should use strictly proper scoring rules and calibration, not accuracy alone [Gneiting2007].

### 5.2 Active perception under a power budget

Sensing is itself an action. Active perception [Bajcsy1988] chooses what to measure by expected information per unit cost:

$$
a^{*}_{\mathrm{sense}}=\arg\max_{a}\;\frac{\mathbb E\big[\mathrm{IG}(a)\big]}{E(a)}\quad\text{subject to latency and thermal limits},
$$

with $\mathrm{IG}$ the expected information gain about the quantities that matter for the current task and $E(a)$ the energy cost. Practical forms include raising camera resolution only when object identity is uncertain, and invoking higher-cost compute only when local confidence is low. **[I]** At the power levels in §7.2, duty-cycled active perception is a requirement, not an optimisation.

### 5.3 Memory and the user model

Memory is separated by timescale: working, episodic, semantic, spatial, procedural, and a slowly updated user model. The requirement is a compact state that supports prediction, not retention of everything. **[I]** Memory is also the largest privacy surface: it should be on-device by default, with explicit retention limits and deletion semantics. The user model holds probabilistic hypotheses about preferences and context, never claims about private mental states.

### 5.4 Failure compounding

If an interaction passes through $n$ stages with success probabilities $p_i$ that are approximately independent, then $P_{\mathrm{success}}\approx\prod_i p_i$ **[I]**. Five stages at 0.95 give 0.77, and ten give 0.60. To reach 0.95 end to end over five stages, each stage needs about 0.99. A wrong gaze target propagates into a wrong intent, tool, and action; confirmations interrupt that chain at a cost $c_q$ (§4.6). This is why end-to-end task success, not component accuracy, is the primary measure (§8).

---

## 6. Frontier D: runtime and compute placement

### 6.1 Layering

The NEXUS runtime sits above existing kernels (Linux, Android), so the abstraction can be tested without a new operating system.

```text
Hardware → Linux / Android / firmware → NEXUS runtime → agents and applications
```

The runtime manages objects that conventional operating systems do not: observations, context, intent hypotheses, capabilities, policies, models, and compute resources, in addition to processes, files, and devices.

### 6.2 Control plane and data plane

| Plane | Content | Mechanism class | Reason |
|---|---|---|---|
| Control | Discovery, intents, permissions, scheduling, status | Compact RPC/IPC (e.g. Binder/AIDL on Android) | Low volume, latency-sensitive, needs schema and authorisation |
| Data | Camera frames, audio, sensor buffers, tensors, rendered frames | Shared memory, DMA-capable buffers, compressed streams | High volume; serialising it as messages is wasteful |

A rough scale argument shows why they must be separated. Uncompressed 1080p video at 30 frames per second in YUV 4:2:0 is $1920\times1080\times1.5\times30\approx93\ \mathrm{MB/s}$ (about $0.75\ \mathrm{Gbit/s}$), whereas Bluetooth LE's 2M PHY has a nominal rate of 2 Mbit/s. **[E, arithmetic]** Raw video cannot cross the low-power link; compression or on-device reduction is mandatory.

### 6.3 Placement as constrained optimisation

A user request decomposes into a directed acyclic task graph $\mathcal G=(V,E)$. Given devices $\mathcal D$, a placement is a map $\phi:V\to\mathcal D$:

$$
\min_{\phi}\;\sum_{v\in V}E_v\big(\phi(v)\big)+\sum_{(u,v)\in E}E^{\mathrm{comm}}_{uv}\big(\phi(u),\phi(v)\big)
$$

$$
\text{s.t.}\quad T_{\mathrm{make}}(\phi)\le\tau_{\max},\qquad
\phi(v)\in\mathcal D^{\mathrm{priv}}_v\ \ \forall v,\qquad
\bar P_d(\phi)\le P_d^{\max}\ \ \forall d\in\mathcal D .
$$

The three constraint families are the interaction-class deadline, per-task privacy eligibility, and per-device power or thermal limits. Version 1.0 of this document used a weighted sum of latency, energy, cost, bandwidth, and risk. That form is rejected here: the terms have incommensurable units, the weights have no principled values, and a scalarisation hides the Pareto structure. Task scheduling on heterogeneous processors is NP-hard in general [Ullman1975]; list-scheduling heuristics such as HEFT [Topcuoglu2002] are the baselines that any learned or online policy must beat **[H]**.

For a single compute-bound task of $C$ cycles and $B$ bytes, offloading saves glasses-side energy only if

$$
P_c\,\frac{C}{S_L}\;>\;P_{\mathrm{tx}}\,\frac{B}{R}+P_{\mathrm{idle}}\,\frac{C}{S_R},
$$

where $S_L$ and $S_R$ are local and remote speeds, $R$ is the link rate, and $P_c$, $P_{\mathrm{tx}}$, $P_{\mathrm{idle}}$ are the compute, transmit, and waiting powers [Cuervo2010]. **[E]** The benefit of the "compute reservoir" is therefore an empirical question, tested in H4, not an assumption.

### 6.4 Model routing

A router selects among a set of models by task, context, and budget: a tiny local model for wake and intent detection, a local vision model for scene interpretation, a nearby-GPU model for heavy multimodal reasoning, and a remote model for rare expensive tasks. Cascades with calibrated early exit are the standard construction. The objective is useful intelligence per unit of energy, latency, and risk, which is again a constrained problem, not a model-size contest.

### 6.5 Capabilities and consequence classes

Authorisation uses scoped, expiring capabilities with attenuation-only delegation [DennisVanHorn1966] [Birgisson2014]:

$$
\kappa=(\text{holder},\ \text{resource},\ \text{operation},\ \text{scope},\ t_{\mathrm{exp}}).
$$

Each operation also carries a consequence class that sets the minimum posterior and confirmation mode, connecting directly to the thresholds of §4.6:

| Class | Example | Rule |
|---|---|---|
| Reversible, low cost | Dismiss a notification | Execute above a low posterior threshold |
| Reversible, costly | Send a drafted message | Confirm with one low-effort gesture |
| Irreversible or external | Payment; deleting data | Explicit multi-modal confirmation; short-lived capability |

---

## 7. Integration: coupled-loop budgets

Each subsystem can be optimised in isolation. The system must satisfy all of them at once, and three budgets couple the layers.

### 7.1 Latency

The loop latency is the sum of its stages:

$$
\tau_{\mathrm{loop}}=\tau_{\mathrm{sense}}+\tau_{\mathrm{decode}}+\tau_{\mathrm{fuse}}+\tau_{\mathrm{plan}}+\tau_{\mathrm{place}}+\tau_{\mathrm{compute}}+\tau_{\mathrm{render}} .
$$

Human-factors heuristics put the thresholds near 0.1 s (feels instantaneous), 1 s (preserves flow), and 10 s (limit of attention) [Nielsen1993]. The allocations below are **assumed design targets** to be replaced by measurement, not findings:

| Interaction class | Example | Target | Illustrative allocation |
|---|---|---|---|
| Reflex | Gesture to visual feedback | ≤ 100 ms | sensing window and link 30 ms; decode 10 ms; fusion and policy 10 ms; render to photons 25 ms; margin 25 ms |
| Conversational | Spoken or gestured query answered with offloaded inference | ≤ 1 s | link round trip 30–100 ms; remote inference 300–800 ms; render |

### 7.2 Power and thermal

Mean power over a use period is bounded by the energy available:

$$
\bar P=\frac1{T_{\mathrm{op}}}\int_0^{T_{\mathrm{op}}}P(t)\,\mathrm{d}t\;\le\;\frac{E_{\mathrm{batt}}}{T_{\mathrm{op}}} .
$$

With an assumed 1 Wh battery and a 10 h target the mean budget is 100 mW, shared among display, sensing, compute, and radio. The only comparable commercial device is vendor-reported at about six hours of mixed use [Meta2025], so the 10 h target is not demonstrated **[H]**. **[I]** Face-worn devices also face skin-contact temperature limits that cap sustained dissipation, which couples compute and display power to comfort. All subsystem power figures must be measured, not assumed.

### 7.3 The three principal couplings

1. **Optical efficiency → source power → thermal and battery budget → compute and sensing duty cycle.** A low-efficiency optical path reduces what every other layer can use **[I]**.
2. **The eye tracker has three roles at once**: an input channel (gaze), an optical requirement (pupil steering, gaze-contingent focus), and a safety component (laser shutdown). It is therefore a single point of dependence and deserves its own reliability analysis **[I]**.
3. **Placement trades glasses-side energy against latency and privacy exposure.** Offloading changes what data leaves the device (§6.3) **[I]**.

---

## 8. A falsifiable research program

### 8.1 Hypotheses

Threshold values are **placeholders proposed for discussion**. They must be fixed and pre-registered before data collection.

| ID | Hypothesis | Primary measurement | Refuted if |
|---|---|---|---|
| **H1** | In an eyewear-accessible montage, under natural motion and on held-out sessions, EEG adds information about intent beyond all other modalities: $I(Y;O_{\mathrm{eeg}}\mid O_{-\mathrm{eeg}},C)\ge\delta_1$ | Held-out log-likelihood gain with controls (i)–(iv) of §4.4 | The control-adjusted gain has a 95% interval that includes 0 or lies below $\delta_1$ (placeholder: the gain that lowers expected selection error by 10% relative) |
| **H2** | Context lowers the signal quality needed for a target accuracy: $\mathrm{SNR}^{*}_{+c}(A^{\star})\le\mathrm{SNR}^{*}_{-c}(A^{\star})-\Delta_{\mathrm{dB}}$ | Inject calibrated noise into biosignal channels; find the noise level at which accuracy $A^{\star}$ is lost, with and without context | The interval for $\Delta_{\mathrm{dB}}$ includes 0 (placeholder target: $\ge 3$ dB) |
| **H3** | With bounded recalibration, decoding is stable across days: accuracy on days 2–7 is at least $\rho$ of day-1 within-session accuracy at fixed FAR | Accuracy, calibration error, and FAR against recalibration time $T_{\mathrm{cal}}$ | The lower bound falls below $\rho$ (placeholders: $T_{\mathrm{cal}}=2$ min, $\rho=0.9$) |
| **H4** | Adaptive placement lowers glasses-side energy per completed task by at least $\gamma$ versus all-local execution without breaching the p95 deadline | Energy per task; latency percentiles on recorded network traces | The saving is below $\gamma$ or the p95 deadline is breached (placeholder: $\gamma=30\%$) |
| **H5** | In a pre-specified task suite the closed loop beats both a smartphone baseline and a voice-only baseline on time to completion, at equal or lower correction rate and $\mathrm{FAR}\le f$ per hour, with no worse SSQ/CVS-Q | Task success, time, corrections, FAR, energy per task, NASA-TLX, SSQ, CVS-Q | It does not beat the strongest baseline |

H2 is the quantitative form of the central idea in v1.0, that context reduces the inference space. H5 insists on real baselines, since "better than nothing" is not a result.

### 8.2 Phases and gates

| Phase | Content | Tests | Gate |
|---|---|---|---|
| 0 | Formal model, runtime skeleton, simulated sensors. **Optical budget model (O1, O2) starts here as a parallel track** | — | Closed loop runs in simulation; first power and thermal envelope from the optical model |
| 1 | Non-neural multimodal baseline: gaze, EOG, wrist sEMG, IMU, voice, vision; synchronised dataset | Baseline posterior; FAR | Calibrated baseline posterior; measured FAR |
| 2 | EEG increment with artefact controls and montage ablation | H1, H2 | If H1 is refuted, NEXUS proceeds on non-EEG channels; EEG remains only for passive-state or error-potential studies |
| 3 | Adaptation and naturalistic motion | H3 | Stability criterion met |
| 4 | Optical integration, placement, closed loop | H4, H5, O3 | H5 met against the strongest baseline |

**[I]** The optical model starts in Phase 0, not last as in v1.0, because it sets the power and thermal envelope for every other decision and has the longest lead time.

### 8.3 Evaluation protocol

- Block cross-validation by session; use leave-one-subject-out for any cross-subject claim.
- Split by stimulus or sequence so that repeated content never appears on both sides of a split [Jo2024]† [dAscoli2025]†.
- Pre-register hypotheses, thresholds, and exclusion rules; report negative results.
- Report calibration (ECE, Brier or log score) alongside accuracy.
- Report FAR per hour with exact Poisson intervals; apply the rule of three when no events occur (§4.6).
- Compare against baselines of equal capacity and equal tuning effort.
- Evaluate during walking, speaking, ordinary head motion, and changing light, not only in static sessions.

### 8.4 Scorecard

| Layer | Metrics |
|---|---|
| Optics | $\Theta$, $A_{\mathrm{eb}}$, $\eta$, luminance, MTF, distortion, $\Delta u'v'$, focus range, SSQ, CVS-Q |
| Neural/neuromotor | Held-out log-likelihood gain, ECE, ITR, FAR per hour, latency, cross-session retention, calibration time |
| State and agency | Strictly proper score of predictions, planning success, memory-retrieval accuracy |
| Runtime | Scheduling latency, energy per task, node-recovery time, deadline misses |
| System | Task success, time per task, correction rate, FAR per hour, energy per task, NASA-TLX |

The headline measure is useful interactions completed per unit of energy, time, and user effort, compared against the strongest baseline.

---

## 9. Risks, ethics, and regulation

### 9.1 Technical risks

| Risk | Consequence | Mitigation |
|---|---|---|
| EEG adds nothing beyond other channels (H1 refuted) | The "neural" differentiator disappears | Architecture works without EEG; report the null result |
| Artefact leakage mimics neural gain | False positive for H1 | Controls (i)–(iv) of §4.4 |
| Optical efficiency too low at target FOV | Brightness–power–thermal infeasible | Optical budget model from Phase 0; smaller eyebox with pupil steering; narrower FOV |
| Context inference acts on a wrong goal | Incorrect action | Calibrated posteriors; consequence-class thresholds (§4.6, §6.5) |
| Offload leaks sensitive data or misses deadlines | Privacy and reliability failure | Local-first defaults; privacy-eligibility constraint in placement (§6.3) |

### 9.2 Neural data, privacy, and governance

Neural and neuro-adjacent signals are among the most sensitive personal data. Proposals for neuro-specific rights, including mental privacy, predate current products [Ienca2017]. In November 2025 UNESCO's member states adopted a Recommendation on the Ethics of Neurotechnology that treats neural data as particularly sensitive [UNESCO2025]†, and 2024 amendments to U.S. state privacy laws, including California's, extended sensitive-data protections to neural data. **[E]** Design consequences **[I]**:

- raw biosignals stay on-device by default; only derived intent hypotheses leave, with explicit consent and retention limits;
- no inference of mental or emotional state for advertising or manipulation;
- head-worn cameras raise bystander-privacy and social-acceptability issues that need visible recording indicators and local processing.

### 9.3 Regulatory note

**[I]** Intended-use claims drive regulatory classification. Health or fatigue claims derived from EEG can move a product toward medical-device regulation, and retinal-scanning laser displays carry product-specific laser regulation (§3.6). This is a design input, not legal advice; formal legal review is required before any claim is made externally.

---

## 10. Conclusion

NEXUS is a coherent systems-research question, not a single technology bet: whether weak, noisy, partly redundant signals can be turned into reliable interaction by modelling context and by deciding, with calibrated uncertainty, when to act. The evidence reviewed here supports three working positions. Peripheral neuromotor and ocular channels are the dependable inputs, and scalp EEG is the least-supported component, which makes it the most informative one to test. Optical efficiency, power, and latency couple the layers, so the optical budget model must come first, not last. Context-aware, consequence-sensitive authorisation is well grounded in decision theory and prior work, and its value in a wearable setting is measurable.

The program is built so that each of these positions can fail in public. If H1 fails, NEXUS continues without EEG. If H4 or H5 fail, the compute-fabric and closed-loop claims do not stand. A result in either direction advances the work.

---

## References

Entries marked † were re-checked against publisher, repository, or index records on 1 October 2026 (for several optics entries via a citing record's reference list). Remaining entries are cited from standard literature and **should be DOI-checked before external circulation**. Standards and regulatory entries are cited by name and should be confirmed against current editions.

**Optics and visual comfort**

- **[ANSI136]** American National Standards Institute / Laser Institute of America. *ANSI Z136.1: Safe Use of Lasers.*
- **[Ding2023]†** Ding, Y. et al. Waveguide-based augmented reality displays: perspectives and challenges. *eLight* 3, 24 (2023). doi:10.1186/s43593-023-00057-z
- **[Ding2024]†** Ding, Y., Gu, Y., Yang, Q., Yang, Z., Huang, Y., Weng, Y., Zhang, Y., Wu, S.-T. Breaking the in-coupling efficiency limit in waveguide-based AR displays with polarization volume gratings. *Light: Science & Applications* 13, 185 (2024). doi:10.1038/s41377-024-01537-8
- **[FDAREE]†** U.S. Food and Drug Administration. Product classification: laser visual display, display retinal image, non-medical (product code REE; not classified). Accessed 1 October 2026.
- **[Gopakumar2024]†** Gopakumar, M. et al. Full-colour 3D holographic augmented-reality displays with metasurface waveguides. *Nature* 629, 791–797 (2024). doi:10.1038/s41586-024-07386-0
- **[Hart1988]** Hart, S.G., Staveland, L.E. Development of NASA-TLX: results of empirical and theoretical research. In *Human Mental Workload*, 139–183 (1988).
- **[Hoffman2008]** Hoffman, D.M., Girshick, A.R., Akeley, K., Banks, M.S. Vergence–accommodation conflicts hinder visual performance and cause visual fatigue. *Journal of Vision* 8(3), 33 (2008).
- **[ICNIRP2013]** ICNIRP. Guidelines on limits of exposure to laser radiation of wavelengths between 180 nm and 1,000 µm. *Health Physics* 105(3), 271–295 (2013).
- **[IEC60825]** International Electrotechnical Commission. *IEC 60825-1: Safety of laser products, Part 1: Equipment classification and requirements.*
- **[Jang2017]** Jang, C., Bang, K., Moon, S., Kim, J., Lee, S., Lee, B. Retinal 3D: augmented reality near-eye display via pupil-tracked light field projection on retina. *ACM Transactions on Graphics* 36(6), 190 (2017).
- **[Kennedy1993]** Kennedy, R.S., Lane, N.E., Berbaum, K.S., Lilienthal, M.G. Simulator sickness questionnaire: an enhanced method for quantifying simulator sickness. *International Journal of Aviation Psychology* 3(3), 203–220 (1993).
- **[KressChatterjee2021]†** Kress, B.C., Chatterjee, I. Waveguide combiners for mixed reality headsets: a nanophotonics design perspective. *Nanophotonics* 10, 41–74 (2021).
- **[Maimone2017]** Maimone, A., Georgiou, A., Kollin, J.S. Holographic near-eye displays for virtual and augmented reality. *ACM Transactions on Graphics* 36(4), 85 (2017).
- **[Moon2025]†** Moon, S., Kim, S., Kim, J., Lee, C.-K., Rho, J. Single-layer waveguide displays using achromatic metagratings for full-colour augmented reality. *Nature Nanotechnology* 20, 747–754 (2025).
- **[Padmanaban2017]** Padmanaban, N., Konrad, R., Stramer, T., Cooper, E.A., Wetzstein, G. Optimizing virtual reality for all users through gaze-contingent and adaptive focus displays. *PNAS* 114(9), 2183–2188 (2017).
- **[Segui2015]** Seguí, M.M., Cabrero-García, J., Crespo, A., Verdú, J., Ronda, E. A reliable and valid questionnaire was developed to measure computer vision syndrome at the workplace. *Journal of Clinical Epidemiology* 68(6), 662–673 (2015).
- **[Shibata2011]** Shibata, T., Kim, J., Hoffman, D.M., Banks, M.S. The zone of comfort: predicting visual discomfort with stereo displays. *Journal of Vision* 11(8), 11 (2011).
- **[Shin2025]†** Shin, J.-Y. et al. Multi-depth switching by triple wavefront modulation of quarter-waveplate geometric phase lenses for vergence-accommodation-matching extended reality. *Light: Science & Applications* 14, 333 (2025). doi:10.1038/s41377-025-02026-2
- **[Tian2025]†** Tian, Z., Zhu, X., Surman, P.A., Chen, Z., Sun, X.W. An achromatic metasurface waveguide for augmented reality displays. *Light: Science & Applications* 14, 94 (2025). doi:10.1038/s41377-025-01761-w

**Neural, neuromotor, and BCI**

- **[Baillet2001]** Baillet, S., Mosher, J.C., Leahy, R.M. Electromagnetic brain mapping. *IEEE Signal Processing Magazine* 18(6), 14–30 (2001).
- **[Bleichner2017]** Bleichner, M.G., Debener, S. Concealed, unobtrusive ear-centered EEG acquisition: cEEGrids for transparent EEG. *Frontiers in Human Neuroscience* 11, 163 (2017).
- **[Chavarriaga2010]** Chavarriaga, R., Millán, J.d.R. Learning from EEG error-related potentials in noninvasive brain–computer interfaces. *IEEE Transactions on Neural Systems and Rehabilitation Engineering* 18(4), 381–388 (2010).
- **[dAscoli2025]†** d'Ascoli, S., Bel, C., Rapin, J., Banville, H., Benchetrit, Y., Pallier, C., King, J.-R. Towards decoding individual words from non-invasive brain recordings. *Nature Communications* 16, 10521 (2025). doi:10.1038/s41467-025-65499-0
- **[Debener2012]** Debener, S., Minow, F., Emkes, R., Gandras, K., de Vos, M. How about taking a low-cost, small, and wireless EEG for a walk? *Psychophysiology* 49(11), 1617–1621 (2012).
- **[Ding2025]†** Ding, Y., Udompanyawit, C., Zhang, Y., He, B. EEG-based brain–computer interface enables real-time robotic hand control at individual finger level. *Nature Communications* 16, 5401 (2025). doi:10.1038/s41467-025-61064-x
- **[Gneiting2007]** Gneiting, T., Raftery, A.E. Strictly proper scoring rules, prediction, and estimation. *Journal of the American Statistical Association* 102(477), 359–378 (2007).
- **[Guo2017]** Guo, C., Pleiss, G., Sun, Y., Weinberger, K.Q. On calibration of modern neural networks. *Proc. ICML*, PMLR 70, 1321–1330 (2017).
- **[Jacob1990]** Jacob, R.J.K. What you look at is what you get: eye movement-based interaction techniques. *Proc. CHI '90*, 11–18 (1990).
- **[Jiang2024]** Jiang, W.-B., Zhao, L.-M., Lu, B.-L. Large brain model for learning generic representations with tremendous EEG data in BCI. *Proc. ICLR* (2024).
- **[Jo2024]†** Jo, H. et al. Are EEG-to-text models working? Preprint, arXiv:2405.06459 (2024).
- **[Kaifosh2025]†** Kaifosh, P., Reardon, T.R., CTRL-labs at Reality Labs. A generic non-invasive neuromotor interface for human–computer interaction. *Nature* 645(8081), 702–711 (2025). doi:10.1038/s41586-025-09255-w
- **[Metzger2023]†** Metzger, S.L. et al. A high-performance neuroprosthesis for speech decoding and avatar control. *Nature* 620, 1037–1046 (2023). doi:10.1038/s41586-023-06443-4
- **[Millan2004]** Millán, J.d.R., Renkens, F., Mouriño, J., Gerstner, W. Noninvasive brain-actuated control of a mobile robot by human EEG. *IEEE Transactions on Biomedical Engineering* 51(6), 1026–1033 (2004).
- **[Pfurtscheller2010]** Pfurtscheller, G. et al. The hybrid BCI. *Frontiers in Neuroscience* 4, 30 (2010).
- **[Seok2021]†** Seok, D. et al. Motion artifact removal techniques for wearable EEG and PPG sensor systems. *Frontiers in Electronics* 2, 685513 (2021). doi:10.3389/felec.2021.685513
- **[Shenoy2006]** Shenoy, P., Krauledat, M., Blankertz, B., Rao, R.P.N., Müller, K.-R. Towards adaptive classification for BCI. *Journal of Neural Engineering* 3(1), R13–R23 (2006).
- **[Tang2026]†** Tang, C., Qi, L., Gao, S. et al. Sensing technologies for silent speech interfaces. *Nature Sensors* 1, 16–26 (2026).
- **[Willett2023]†** Willett, F.R. et al. A high-performance speech neuroprosthesis. *Nature* 620, 1031–1036 (2023). doi:10.1038/s41586-023-06377-x
- **[Wolpaw2002]** Wolpaw, J.R., Birbaumer, N., McFarland, D.J., Pfurtscheller, G., Vaughan, T.M. Brain–computer interfaces for communication and control. *Clinical Neurophysiology* 113(6), 767–791 (2002).
- **[Zander2011]** Zander, T.O., Kothe, C. Towards passive brain–computer interfaces: applying brain–computer interface technology to human–machine systems in general. *Journal of Neural Engineering* 8(2), 025005 (2011).

**State estimation, systems, and governance**

- **[Assran2025]** Assran, M. et al. V-JEPA 2: self-supervised video models enable understanding, prediction and planning. Preprint, arXiv:2506.09985 (2025).
- **[Bajcsy1988]** Bajcsy, R. Active perception. *Proceedings of the IEEE* 76(8), 966–1005 (1988).
- **[Birgisson2014]** Birgisson, A. et al. Macaroons: cookies with contextual caveats for decentralized authorization in the cloud. *Proc. NDSS* (2014).
- **[Cuervo2010]** Cuervo, E. et al. MAUI: making smartphones last longer with code offload. *Proc. MobiSys '10*, 49–62 (2010).
- **[DennisVanHorn1966]** Dennis, J.B., Van Horn, E.C. Programming semantics for multiprogrammed computations. *Communications of the ACM* 9(3), 143–155 (1966).
- **[Dey2001]** Dey, A.K. Understanding and using context. *Personal and Ubiquitous Computing* 5(1), 4–7 (2001).
- **[Ha2014]** Ha, K., Chen, Z., Hu, W., Richter, W., Pillai, P., Satyanarayanan, M. Towards wearable cognitive assistance. *Proc. MobiSys '14*, 68–81 (2014).
- **[Horvitz1999]** Horvitz, E. Principles of mixed-initiative user interfaces. *Proc. CHI '99*, 159–166 (1999).
- **[Ienca2017]** Ienca, M., Andorno, R. Towards new human rights in the age of neuroscience and neurotechnology. *Life Sciences, Society and Policy* 13, 5 (2017).
- **[Kaelbling1998]** Kaelbling, L.P., Littman, M.L., Cassandra, A.R. Planning and acting in partially observable stochastic domains. *Artificial Intelligence* 101(1–2), 99–134 (1998).
- **[LeCun2022]** LeCun, Y. A path towards autonomous machine intelligence, version 0.9.2. OpenReview (2022).
- **[Meta2025]** Meta. Meta Ray-Ban Display and Meta Neural Band product announcement (Meta Connect, September 2025). Specifications as reported in launch coverage; vendor-reported.
- **[Nielsen1993]** Nielsen, J. *Usability Engineering.* Academic Press (1993).
- **[Satyanarayanan2009]** Satyanarayanan, M., Bahl, P., Cáceres, R., Davies, N. The case for VM-based cloudlets in mobile computing. *IEEE Pervasive Computing* 8(4), 14–23 (2009).
- **[Topcuoglu2002]** Topcuoglu, H., Hariri, S., Wu, M.-Y. Performance-effective and low-complexity task scheduling for heterogeneous computing. *IEEE Transactions on Parallel and Distributed Systems* 13(3), 260–274 (2002).
- **[UNESCO2025]†** UNESCO. *Recommendation on the Ethics of Neurotechnology.* Adopted by Member States, November 2025.
- **[Ullman1975]** Ullman, J.D. NP-complete scheduling problems. *Journal of Computer and System Sciences* 10(3), 384–393 (1975).
- **[Weiser1991]** Weiser, M. The computer for the 21st century. *Scientific American* 265(3), 94–104 (1991).
