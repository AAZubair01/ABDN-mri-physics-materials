# MRI Physics for the Imaging Scientist

**Magnetic Fields · Image Formation · Contrast Weightings · Pulse Sequences · fMRI Physics**

\---

**PREPARED BY**  
Abdulrazaq A. Zubair, B.Rad, MSc  
PhD Candidate, Bayero University Kano  
Federal University of Health Sciences, Azare

**PREPARED FOR**  
Fellows of the ABDN Programme (Radiography · Computer Science · Medicine · Neuroscience · Anatomy · Engineering)

**LEVEL**  
Conceptual to intermediate — MRI/fMRI cohort

**VERSION**  
July to November 2026

\---

## Contents

* [How to Use These Notes](#how-to-use-these-notes)
* [Module 1 — The MRI Machine and Its Magnetic Fields](#module-1--the-mri-machine-and-its-magnetic-fields)
* [Module 2 — Image Formation: From Spins to Pixels](#module-2--image-formation-from-spins-to-pixels)
* [Module 3 — Contrast Weightings: Reading the Images](#module-3--contrast-weightings-reading-the-images)
* [Module 4 — Pulse Sequences: The Engine Underneath](#module-4--pulse-sequences-the-engine-underneath)
* [Module 5 — fMRI Physics: Imaging the Working Brain](#module-5--fmri-physics-imaging-the-working-brain)
* [Summary — The Chain of MRI](#summary--the-chain-of-mri)
* [Cheat Sheet \& Glossary](#cheat-sheet--glossary)
* [Further Reading](#further-reading)
* [Figure Credits](#figure-credits)

\---

## How to Use These Notes

These notes accompany the ABDN fellows' lecture on MRI physics. They are written for an audience that uses MRI data rather than acquires it: computer scientists training models, neuroscientists mapping brain circuits, anatomists measuring structures, engineers building reconstruction pipelines, and clinicians interpreting scans.

The philosophy is simple. You will probably never stand at the scanner console and choose a repetition time. But every dataset you will ever touch was shaped by someone who did, and the choices they made live inside your pixels. A model trained without knowing what TE does will fail on a new scanner and you will not know why. An fMRI result interpreted without knowing what T2\* is may be an artefact rather than a finding. These notes exist to make you literate in the language of the machine that generates your data.

Each module follows the same rhythm: a plain-language picture first (an analogy you can repeat at a dinner table), then the physics stated precisely (with the one or two equations that genuinely matter), then worked examples and real clinical images, and finally a short "why you should care" box tied to research practice. Nothing here requires more than secondary-school mathematics to read; everything here is stated accurately enough to defend in a viva.

### The five questions these notes answer

1. What are the three magnetic fields inside the scanner, and what does each one do? (Module 1)
2. How does a signal from a spinning proton become a pixel with an address? (Module 2)
3. Why does the same brain look completely different on T1w, T2w, FLAIR, DWI and SWI? (Module 3)
4. What do the pulse-sequence acronyms (SE, TSE, GRE, EPI, IR) actually mean physically? (Module 4)
5. How can a scanner that sees water protons possibly image a thought? (Module 5)

\---

## Module 1 — The MRI Machine and Its Magnetic Fields

### Learning objectives

* Explain why MRI is built around hydrogen protons.
* Describe what B₀ does to proton populations and why it produces net magnetization.
* Define precession and compute the Larmor frequency at any field strength.
* Distinguish the roles of B₀, B₁ and the three gradient fields.
* State the practical trade-offs of 1.5T vs 3T vs 7T.

### 1.1 What MRI actually measures

MRI does not photograph anatomy. It measures the magnetic behaviour of hydrogen protons — mostly the protons in water and fat — and reconstructs an image from that behaviour. The body is roughly 60–70% water, so every tissue carries an enormous, built-in signal source. No injected tracer, no ionizing radiation: the contrast comes from physics, not from a dye (though contrast agents can be added later).

Three ingredients do all the work: a very strong static magnet (B₀), radio waves (B₁ pulses), and switchable gradient fields (Gₓ, Gᵧ, Gz). A computer orchestrates them and turns the returning radio signals into images. Everything in these five modules is a consequence of how those three magnetic fields interact with spinning protons.

> \*\*Figure 1.1.\*\* The bore of the scanner. Three magnetic systems surround the patient: the superconducting B₀ magnet (always on), the gradient coils (x, y, z), and the RF transmit/receive coil that produces B₁ and listens for the returning signal.

### 1.2 Protons: the tiny magnets inside us

The nucleus of hydrogen is a single proton. It has a positive charge and a quantum property called spin, and a moving charge generates a magnetic field — so each proton behaves like a microscopic bar magnet with a north and a south pole. Outside a magnetic field, billions of these tiny magnets point in random directions and their fields cancel: no net signal exists to detect.

**Picture it.** A stadium full of football fans, each waving a flag in a random direction. From the commentary box you see chaos, no pattern. The fans are your protons before the magnet is switched on. MRI begins the moment someone gives the crowd a reason to face the same way.

**Why hydrogen and not another nucleus?** Two reasons, both decisive. First, hydrogen is the most abundant nucleus in the body (water and fat are everywhere). Second, it has the largest gyromagnetic ratio of the common biological nuclei (γ = 42.58 MHz/T, compared with 17.24 for ³¹P, 11.26 for ²³Na and 10.71 for ¹³C), so its signal is intrinsically loud. Abundant and loud: hydrogen wins on both counts.

### 1.3 B₀: the main magnetic field and alignment

When a patient slides into the bore, B₀ — typically 1.5 or 3 tesla, roughly 30,000–60,000 times the Earth's magnetic field acts on every proton at once. Protons settle into one of two energy states: parallel (with the field, lower energy) or anti-parallel (against it, higher energy). Slightly more choose parallel; at body temperature the excess is only a few protons per million, but across the trillions of protons in a voxel that small excess adds up to a measurable net magnetization vector, **M**, pointing along B₀.

> \*\*Figure 1.2.\*\* Left: outside the scanner, proton moments point randomly and cancel. Right: inside B₀, protons align parallel or anti-parallel; the slight parallel excess creates the net magnetization M that all MRI signal ultimately comes from.

**Why B₀ alone produces no image.** M points along B₀ and just sits there. A receiver coil detects only changing magnetic flux (Faraday induction), and a static vector changes nothing. B₀ prepares the spins; it cannot interrogate them. For that we need B₁ — Section 1.5 — and to know where the signal came from we need the gradients — Section 1.6.

### 1.4 Precession and the Larmor frequency

Aligned protons do not sit still. Because they are spinning, the torque from B₀ makes them precess wobble around the field direction, exactly like a spinning top wobbling around gravity. The rate of that wobble is not arbitrary. It is set by one equation, the most important in all of MRI:

$$\\omega = \\gamma B\_0$$

where ω is the Larmor (precession) frequency, γ is the gyromagnetic ratio of the nucleus (42.58 MHz/T for hydrogen), and B₀ is the main field strength.

**Worked examples:**

|Field strength|Calculation|Larmor frequency|Typical use|
|-|-|-|-|
|1.5 T|42.58 × 1.5|\~63.9 MHz|Routine clinical imaging|
|3.0 T|42.58 × 3.0|\~127.7 MHz|High-resolution neuroimaging, fMRI|
|7.0 T|42.58 × 7.0|\~298.1 MHz|Research: cortical layers, spectroscopy|

> \*\*Figure 1.3.\*\* Left: a proton precesses around B₀ like a wobbling top. Right: the Larmor frequency scales linearly with field strength, protons at 3T precess exactly twice as fast as at 1.5T.

**Picture it.** Every nucleus is a radio station broadcasting at its own fixed frequency, and γ is what sets the dial. Hydrogen in a 1.5T scanner broadcasts at 63.9 MHz, always, predictably. To talk to the protons, the scanner must transmit on exactly their station. Slightly off-frequency, and the protons simply do not answer. That selectivity is called resonance, and it is why the technique is called magnetic resonance imaging.

### 1.5 B₁: the radiofrequency field and resonance

B₁ is a second, much weaker magnetic field, switched on in brief bursts by the RF coil and oscillating at the Larmor frequency. Because it matches the protons' precession frequency, energy transfers efficiently (resonance) and the net magnetization tips away from B₀ by an angle called the **flip angle (α)**. A 90° pulse tips M fully into the transverse (xy) plane; smaller angles are used in fast imaging. When B₁ switches off, M is left rotating in the transverse plane — and a rotating magnetic vector is precisely what a receiver coil can detect. The B₁ pulse is therefore the moment the invisible becomes measurable.

**Picture it.** B₀ is the parade-ground general who keeps the soldiers standing at attention. B₁ is the visiting officer who arrives, shouts one sharp order "turn!" and leaves. The soldiers (protons) break ranks together, in step, and it is that synchronized movement the scanner records. When the order fades, they drift back to attention — and how fast each group drifts back is the contrast you will meet in Module 2.

### 1.6 The gradients: Gₓ, Gᵧ, Gz

Gradient coils are electromagnets that add a small, controlled, position-dependent tilt to the main field. With a gradient on, the field — and therefore the Larmor frequency — varies smoothly from one side of the patient to the other. Since frequency and phase now encode position, the scanner can work out where every signal originated. There are three sets of gradient coils, one per axis, and each has a specific job in image formation:

|Gradient|Axis|Job|When it acts|
|-|-|-|-|
|Gz — slice select|Z|Only one slice resonates with the RF pulse|During RF excitation|
|Gᵧ — phase encode|Y|Gives each row a distinct phase twist|Briefly before readout|
|Gₓ — frequency encode (readout)|X|Gives each column a distinct frequency|During signal collection|

**Picture it.** Gradients are GPS for protons. B₀ gets everyone facing north; B₁ makes everyone sing; the gradients make each row of the choir sing at a different pitch and each column start on a different beat. Listen to the chord and you can tell exactly who sang — and where they were standing.

### 1.7 Field strength: 1.5T, 3T, 7T and safety

Doubling B₀ does not simply "double the image quality." The gains and the costs scale together:

|Property|Higher field →|Practical consequence|
|-|-|-|
|Signal (SNR)|Rises linearly|Finer resolution or faster scans|
|T₁ of tissues|Lengthens|Contrast behaviour changes; protocols must be re-tuned|
|Susceptibility effects|Stronger|More distortion near air/bone/implants — but better SWI and BOLD|
|RF energy (SAR)|Rises|Heating limits bind harder at 3T and above|
|B₁ uniformity|Worsens|Shading artefacts, especially at 7T|
|Chemical shift|Larger|Fat/water misregistration increases|

**For a researcher pooling data across sites, the message is that field strength is a study variable, not a scanner spec.** A 1.5T image and a 3T image of the same brain are not interchangeable inputs for an analysis pipeline or a trained model — their contrast, resolution and artefact profiles differ systematically.

**Module 1 take-home.** B₀ aligns the spins and sets their precession frequency (ω = γB₀). B₁, tuned to exactly that frequency, tips them into the measurable plane. The gradients stamp each signal with a positional address. Every image, artefact and contrast you will ever meet is these three fields, choreographed in time.

\---

## Module 2 — Image Formation: From Spins to Pixels

### Learning objectives

* Explain what a 90° RF pulse makes measurable, and what the flip angle controls.
* Define T₁, T₂ and T₂\* precisely and state their typical tissue values.
* Describe how slice selection, phase encoding and frequency encoding localize signal.
* Explain what k-space is, what its centre and periphery contain, and how the image is recovered from it.

### 2.1 Tipping the magnetization: the flip angle

At equilibrium M lies along B₀, invisible to the coil. A 90° B₁ pulse rotates it fully into the transverse plane, where it precesses at the Larmor frequency and induces an oscillating voltage the MR signal. The flip angle is a genuine design choice: a full 90° gives maximum signal per excitation, while smaller angles (10–40°) let you repeat excitations rapidly without waiting for full recovery, which is how fast gradient-echo imaging works. Nothing about the anatomy changed; only the geometry of the magnetization changed and that alone created the signal.

> \*\*Figure 2.1.\*\* The three moments of excitation: M aligned with B₀ (undetectable), the 90° B₁ pulse tipping it into the transverse plane, and the rotating transverse magnetization inducing signal in the receiver coil.

### 2.2 Relaxation: T₁, T₂ and T₂\*

The instant the RF pulse stops, two independent processes begin simultaneously. They are the origin of all MRI contrast, so they deserve careful definitions.

#### T₁ — spin-lattice relaxation (recovery)

T₁ describes how fast M regrows along B₀ as protons hand their excitation energy to the surrounding tissue (the "lattice"). It is an exponential recovery:

$$M\_z(t) = M\_0(1 - e^{-t/T\_1})$$

After one T₁, 63% of the magnetization has recovered. T₁ is short where molecular motion is well matched to the Larmor frequency (fat, ≈260 ms at 3T) and long where water tumbles too fast to exchange energy efficiently (CSF, ≈4200 ms). T₁ lengthens with field strength — one of the reasons 1.5T and 3T images of the same tissue look different.

> \*\*Figure 2.2.\*\* T₁ recovery for four tissues (values at 3T). Fat recovers quickly; CSF barely recovers within seconds. At a short repetition time, fat has regrown most of its signal and CSF almost none that difference is T₁ contrast.

#### T₂ — spin-spin relaxation (decay)

T₂ describes how fast the transverse signal dies as protons exchange energy with each other and drift out of phase. No energy is lost to the lattice; coherence is lost among neighbours:

$$M\_{xy}(t) = M\_0 e^{-t/T\_2}$$

After one T₂, only 37% of the signal remains. CSF keeps phase coherence for a long time (T₂ ≈ 2000 ms), so it is still "singing in unison" long after muscle (T₂ ≈ 45 ms) has gone silent. Dephasing is always faster than energy recovery, so in every tissue T₂ ≤ T₁ — usually much less.

> \*\*Figure 2.3.\*\* T₂ decay. At a late echo time, long-T₂ fluid still has signal while short-T₂ muscle has decayed — that difference is T₂ contrast.

#### T₂\* — the honest decay (tissue + machine)

In a real scanner the main field is never perfectly uniform, and every air–tissue or bone–tissue interface adds its own local distortion. These extra dephasing sources accelerate the decay beyond true T₂:

$$\\frac{1}{T\_2^\*} = \\frac{1}{T\_2} + \\frac{1}{T\_2'}$$

where T₂′ captures everything the tissue is not responsible for field inhomogeneity, susceptibility interfaces, iron, blood products. T₂\* is always shorter than T₂. Two things you will meet constantly depend on T₂\*: gradient-echo imaging (Module 4.3), which cannot rewind T₂′ effects, and BOLD fMRI (Module 5), which exploits them.

> \*\*Figure 2.4.\*\* The free induction decay. The oscillating grey line is what the coil actually measures; it dies inside the T₂\* envelope, which always decays faster than the true tissue T₂ envelope.

**Representative relaxation times in brain at 3T** (approximate; values vary by tissue state and field strength):

|Tissue|T₁ (ms)|T₂ (ms)|Bright on T1w?|Bright on T2w?|
|-|-|-|-|-|
|Fat / subcutaneous|\~260|\~80|Yes|Intermediate|
|White matter|\~790|\~80|Brighter than grey|Darker than grey|
|Grey matter|\~920|\~90|Intermediate|Intermediate|
|CSF|\~4200|\~2200|No (dark)|Yes (very bright)|
|Acute haematoma (deoxy-Hb)|\~isointense|short|No|No (dark)|

### 2.3 Spatial encoding with gradients

Relaxation tells us what the tissue is doing; it says nothing about where. Localization is the gradients' job, and it happens in three coordinated steps, each exploiting ω = γB.

#### Step 1 — Slice selection (Gz)

Switch Gz on during the RF pulse. Now the Larmor frequency varies linearly along z, so a B₁ pulse with a narrow bandwidth excites only the thin slab whose frequency matches. Everyone outside the slice simply does not resonate. Slice position is chosen by the RF centre frequency; slice thickness by the bandwidth and gradient strength.

#### Step 2 — Phase encoding (Gᵧ)

Apply Gᵧ briefly, then switch it off. While it was on, spins at different y-positions precessed at different speeds; when it stops they return to a common frequency but remember how far ahead or behind they fell as a position-dependent phase. This step encodes only one line of data, so it must be repeated with many different Gᵧ strengths. Phase encoding is why scans take minutes, not seconds.

#### Step 3 — Frequency encoding (Gₓ)

During signal collection, Gₓ makes precession frequency vary along x. The measured signal is a chord of many frequencies; a Fourier transform separates them, and each frequency maps to an x-position. The pitch tells you the column; the phase memory tells you the row.

> \*\*Figure 2.5.\*\* The three-step localization scheme: Gz selects the slice, Gᵧ stamps each row with a phase, Gₓ stamps each column with a frequency, and the Fourier transform turns the coded signal back into positions.

**Why this matters for your research.** Slice thickness and slice gap are acquisition choices, not anatomy. A 4 mm lesion in a 5 mm slice with a 1 mm gap can vanish into partial volume averaging. In-plane resolution and through-plane resolution are different numbers with different consequences check both before trusting a measurement taken from someone else's scan.

### 2.4 k-space: the raw data of MRI

The scanner never stores an image directly. It stores a matrix of spatial-frequency measurements called **k-space**: each data point contains signal from the whole slice, encoded at one particular combination of phase and frequency. The image and k-space are a Fourier pair one is the 2D Fourier transform of the other:

$$I(x,y) = \\mathcal{F}^{-1}{S(k\_x, k\_y)}$$

**The geography of k-space** is one of the most useful facts in practical MRI:

* **Centre (low spatial frequencies):** carries the bulk of the signal overall brightness and contrast. Lose the centre and the image is a faint outline.
* **Periphery (high spatial frequencies):** carries edges and fine detail resolution. Lose the periphery and the image is a blurry blob with correct contrast.

> \*\*Figure 2.6.\*\* A real Fourier demonstration on a standard test phantom. Reconstructing from the centre of k-space alone preserves contrast but destroys detail; reconstructing from the periphery alone preserves edges but destroys contrast. The full image needs both.

**Three consequences follow immediately:**

1. **Motion corrupts k-space, not pixels:** a patient who moves between two phase-encoding steps creates structured ghosting across the whole image — which is why motion correction is a Fourier-domain problem, not a "sharpen the blurry photo" problem.
2. **You can skip lines** (partial Fourier, parallel imaging, compressed sensing) to go faster, paying with SNR or reconstruction artefacts the entire field of deep-learning reconstruction lives in this trade.
3. **Sequence families differ in how they walk through k-space**, which is exactly the subject of Module 4.

**Module 2 take-home.** RF tips M into the measurable plane; T₁ recovery and T₂/T₂\* decay give each tissue a signature; the gradients encode position as frequency and phase; k-space accumulates the encoded signal; the inverse FFT turns it into the image on your screen. Pixels are the end of the story, not the beginning.

\---

## Module 3 — Contrast Weightings: Reading the Images

### Learning objectives

* Define TR and TE and predict contrast from their values.
* Recognize T1w, T2w, PD, FLAIR, STIR, DWI/ADC and SWI/T2\*w images at a glance.
* State what each weighting is for, and what can go wrong when it is misused.

### 3.1 TR and TE: the two dials of contrast

Every MRI sequence repeats a basic cycle: excite, encode, listen, repeat. Two timing parameters dominate how the result looks:

* **TR (repetition time):** how long the scanner waits between excitations. A short TR catches tissues mid-recovery, so their T₁ differences show. A long TR lets everyone recover, erasing T₁ differences.
* **TE (echo time):** how long the scanner waits after excitation before listening. A long TE lets fast-decaying tissues go quiet first, so their T₂ differences show. A short TE listens before decay separates tissues.

> \*\*Figure 3.1.\*\* The contrast matrix. Choose TR and TE and you have chosen your weighting: short/short = T1w, long/long = T2w, long/short = PD. "Weighting" simply means which tissue property dominates the image.

**Picture it.** TR is how long you let the choir rest between songs; TE is how long you wait before judging who is still singing. Judge too soon (short TE) and everyone sounds equal. Let them rest fully (long TR) and the T₁ differences vanish. The conductor's two choices decide which choir members stand out and the radiographer's two choices decide which tissues stand out.

### 3.2 T1w, T2w and PD

|Weighting|TR|TE|Fat|Fluid (CSF)|Main use|
|-|-|-|-|-|-|
|T1-weighted|Short (<1000 ms)|Short (<30 ms)|Bright|Dark|Anatomy, grey/white differentiation, post-contrast|
|T2-weighted|Long (>2000 ms)|Long (>80 ms)|Intermediate|Bright|Pathology: oedema, inflammation, most lesions|
|PD-weighted|Long (>2000 ms)|Short (<30 ms)|Intermediate|Intermediate|Fine structural detail (MSK)|

The mnemonic worth memorizing: **T₁ = fat bright (anatomy); T₂ = water bright (pathology).** Pathology almost always involves extra water — oedema, inflammation, demyelination, many tumours, so T2w is the workhorse "where is the disease?" sequence, while T1w is the "where exactly is the anatomy?" sequence, and the pair together triangulates most diagnoses.

> \*\*Figure 3.2.\*\* The same brain imaged three ways. On T1w, CSF in the ventricles is black and white matter is brighter than grey; on T2w the CSF is brilliant and the grey/white relationship inverts; PD sits between the two. Nothing about the patient changed only TR and TE.

**Reading habit.** Find the CSF first. If the ventricles are black, you are looking at T1w (or FLAIR); if they are white, T2w; if grey, PD. This two-second check orients you before you read a single finding — and it is also a sanity check on whether a dataset's labels match its content.

### 3.3 FLAIR and STIR: making a tissue disappear

Sometimes one tissue is so bright it hides its neighbours. Periventricular MS plaques sit beside brilliantly bright CSF on T2w; bone-marrow oedema hides inside bright fat. The solution is an inversion recovery preparation (physics in Module 4.5): invert all magnetization with a 180° pulse, then wait exactly until the offending tissue's magnetization crosses zero as it recovers and image at that instant. That tissue contributes no signal and vanishes:

* **FLAIR (fluid-attenuated inversion recovery):** TI chosen to null CSF (≈2900 ms at 3T). CSF goes black, so lesions next to ventricles and sulci finally stand out. The single most important neuro sequence after T2w itself.
* **STIR (short-tau inversion recovery):** TI chosen to null fat (≈180 ms at 3T). Fat goes black, so oedema and marrow lesions shine through. Works at any field strength, unlike frequency-selective fat suppression.

> \*\*Figure 3.3.\*\* T1w, T2w and FLAIR of the same slice. FLAIR is a T2w image with the CSF signal switched off: brain lesions keep their T2 brightness while the ventricles go dark.

> \*\*Figure 3.4.\*\* The physics behind FLAIR and STIR: after a 180° inversion pulse each tissue's magnetization recovers through zero at its own time (TI ≈ T₁ × ln 2). Image at fat's zero-crossing and you have STIR; image at CSF's zero-crossing and you have FLAIR.

### 3.4 DWI and ADC: imaging water motion

Diffusion-weighted imaging adds a pair of strong, matched gradients whose combined effect is to make signal sensitive to random microscopic water motion (Brownian motion). Water that moves freely between the two gradients loses phase coherence and goes dark; water that is trapped inside swollen cells, in viscous pus, in densely packed tumour — keeps its signal and stays bright. The strength of the diffusion weighting is the **b-value** (s/mm²); routine brain DWI uses b ≈ 0 and b ≈ 1000.

$$S(b) = S\_0 e^{-b \\cdot ADC}$$

Acquire at two or more b-values and the equation can be solved voxel-by-voxel for the **apparent diffusion coefficient (ADC)** — a quantitative map of how freely water actually moves. This matters because DWI brightness alone is ambiguous: DWI is a T2-weighted image with diffusion gradients bolted on, so a bright lesion might be truly restricted water or simply long-T₂ fluid ("T2 shine-through"). The ADC map settles it:

**The rule of DWI interpretation.** DWI bright and ADC dark = true restricted diffusion (acute infarct, abscess, highly cellular tumour). DWI bright but ADC bright = T2 shine-through, an artefact, not restriction. **Never report DWI without checking the ADC map.**

> \*\*Figure 3.5.\*\* Left: signal falls with b-value according to each tissue's ADC — restricted water (red) keeps signal at high b. Right: the interpretation rule linking DWI and the ADC map.

### 3.5 SWI and T2\*w: imaging field disturbances

Blood degradation products (deoxyhaemoglobin, methaemoglobin, haemosiderin), iron deposits and calcification are all magnetically different from brain tissue. Each one creates a small local field disturbance that accelerates T₂\* decay in its neighbourhood — and a T₂\*-weighted gradient-echo sequence turns those disturbances into signal voids. Susceptibility-weighted imaging (SWI) pushes this furthest: a high-resolution 3D T2\*w acquisition whose phase information is used to amplify the contrast, making microbleeds, venous structures and iron visible as striking black dots.

> \*\*Figure 3.7.\*\* Why haemorrhage goes black on T2\*w/SWI: local field disturbances destroy phase coherence rapidly, hollowing out the signal around blood products and iron.

> \*\*Figure 3.8.\*\* The same patient on conventional T2\*w GRE (A) and SWI (B): SWI reveals dramatically more microbleeds (black dots) and delineates the basal-ganglia haemorrhage more clearly the clinical value of engineering a sequence around T₂\*.

**Module 3 take-home.** Contrast is not colour; it is a deliberate choice of which physical property to amplify. The same brain yields anatomy (T1w), pathology (T2w), lesion visibility (FLAIR), fat-free oedema (STIR), cellular water motion (DWI/ADC) and blood products (SWI) all from the same protons, asked different questions.

\---

## Module 4 — Pulse Sequences: The Engine Underneath

### Learning objectives

* Read a basic pulse-sequence timing diagram (RF, gradients, signal).
* Explain how SE, TSE/FSE, GRE, EPI and IR generate echoes and fill k-space.
* Match each sequence family to its strengths, speed and characteristic artefacts.

A pulse sequence is the exact timetable of RF pulses and gradient switches that produces an image. Module 3 told you what contrast each weighting shows; this module tells you how the machine manufactures it. One concept unlocks the whole family tree: **how the echo is formed**.

### 4.1 Spin echo (SE): the honest rewind

After a 90° pulse, spins dephase by both true T₂ and reversible T₂′ effects. SE inserts a 180° pulse at TE/2, which flips the dephasing spins so the fast ones are now behind and the slow ones ahead they re-converge into an echo at TE. Field inhomogeneity is rewound; only true tissue T₂ remains. SE is therefore the reference standard for T₂ contrast but one excitation fills one k-space line, so it is slow.

**Picture it.** Runners of different speeds start together and spread out around a track. At the halfway signal (the 180° pulse) everyone turns and runs back the way they came. Fast runners have further to return, slow runners less — and everyone crosses the start line at the same instant. That reunion is the echo.

> \*\*Figure 4.1.\*\* SE timing: 90° excitation with slice-select gradient, phase-encoding blip, 180° refocusing at TE/2, echo collected at TE during the readout gradient.

### 4.2 Turbo / fast spin echo (TSE/FSE): the production line

TSE applies a whole train of 180° pulses after one excitation, harvesting several echoes and several k-space lines — per TR. The echo train length (ETL) sets the speed-up: ETL 8 means roughly 8× faster than SE. The cost is subtle: echoes collected at different effective TEs are mixed into one image, giving mild blurring and altered fat contrast (fat stays bright on TSE T2 — a common trap when comparing with SE T2). Virtually all clinical T2w imaging today is TSE/FSE.

> \*\*Figure 4.2.\*\* TSE/FSE: multiple 180° refocusing pulses after a single 90° excitation, each echo filling a different k-space line.

### 4.3 Gradient echo (GRE): speed without the rewind

GRE abandons the 180° pulse entirely. A readout gradient is first applied in reverse (dephase lobe), then flipped (rephase lobe): spins swept apart by the gradient are swept back together into an echo. Nothing rewinds the T₂′ effects from field inhomogeneity, blood products or iron — so GRE is T₂\*-weighted, fast (small flip angles, short TR), 3D-capable, and exquisitely sensitive to susceptibility. Haemorrhage, calcification and air–tissue interfaces bloom dark. The same sensitivity that makes GRE the basis of SWI and BOLD fMRI also makes it vulnerable to distortion near sinuses, mastoids and skull base.

> \*\*Figure 4.3.\*\* GRE timing: one RF pulse, a dephase/rephase gradient pair forming the echo, no 180° pulse — so T₂\* decay proceeds unchecked.

### 4.4 Echo planar imaging (EPI): the speed demon

EPI asks the extreme question: how much of k-space can be filled after one excitation? Answer — all of it. The readout gradient oscillates rapidly back and forth, sweeping a zigzag trajectory through k-space in a single shot lasting tens of milliseconds. A whole slice per shot, a whole brain per second or two: this is what makes DWI, DTI and fMRI practical. The price is that T₂\* decay continues during the readout, so EPI images suffer geometric distortion (worst near air–tissue interfaces), signal dropout, ghosting and blurring. Every fMRI preprocessing pipeline (distortion correction, motion correction) exists because of these artefacts.

> \*\*Figure 4.4.\*\* Left: the single-shot EPI trajectory through k-space. Right: signal decays during the long readout — the origin of EPI's blurring, distortion and ghosting.

### 4.5 Inversion recovery (IR): contrast by subtraction

IR prepends a 180° inversion pulse before the imaging sequence, then waits a chosen inversion time (TI) while magnetizations recover through zero. Image at a tissue's zero-crossing and that tissue is erased: FLAIR (TI ≈ 2900 ms) erases CSF, STIR (TI ≈ 180 ms) erases fat. A third family member, MP-RAGE/MP2RAGE, combines an inversion preparation with fast 3D spoiled GRE to produce the high-resolution T1w anatomical volumes that neuroimaging pipelines (FreeSurfer, SPM, FSL) expect as their structural input.

**Sequence families at a glance:**

|Family|Echo formed by|k-space per excitation|Weighting|Strength|Weakness / artefact|
|-|-|-|-|-|-|
|SE|180° RF pulse|1 line|T1, T2, PD|Robust, true T2|Slow|
|TSE/FSE|Train of 180° pulses|ETL lines|T2 (routine)|Fast clinical T2|Blurring; fat stays bright|
|GRE|Gradient reversal|1 line|T2\*, T1 (spoiled)|Fast, 3D, susceptibility|Distortion, signal voids|
|EPI|Oscillating gradient|Whole grid (single shot)|T2\*, DWI|Fastest; DWI/fMRI|Ghosting, dropout, distortion|
|IR|180° prep + SE/GRE readout|Varies|T1-controlled|Nulls one tissue (FLAIR, STIR)|Long; TI must match T₁|

**Module 4 take-home.** SE rewinds with RF and is honest but slow; TSE rewinds many times and is the clinical workhorse; GRE rewinds with gradients and trades fidelity for speed and susceptibility sensitivity; EPI fills everything at once and pays with artefacts; IR deletes a chosen tissue. "Sequence" and "weighting" are not synonyms — a weighting is a contrast goal, a sequence is the machinery that reaches it.

\---

## Module 5 — fMRI Physics: Imaging the Working Brain

### Learning objectives

* Explain the BOLD mechanism from neuron to pixel.
* Describe the haemodynamic response function and its consequences for experimental design.
* Explain why fMRI is built on EPI and what its characteristic artefacts are.

### 5.1 BOLD: blood as an endogenous contrast agent

Functional MRI measures no neurons directly. It exploits a lucky piece of physiology and one piece of physics you already know —

**The physiology:** when a brain region becomes active, local blood flow increases far more than oxygen consumption does — an overshoot called neurovascular coupling.

**The physics:** haemoglobin's magnetism depends on whether it carries oxygen. Deoxyhaemoglobin is paramagnetic (it disturbs the local field and accelerates T₂\* decay); oxyhaemoglobin is diamagnetic (near-invisible to the field). When activity flushes a region with oxygenated blood, deoxyhaemoglobin is diluted, the local field calms, T₂\* lengthens slightly, and the T₂\*-weighted signal rises by a few percent. That faint rise is the **BOLD signal** — blood-oxygen-level-dependent contrast.

> \*\*Figure 5.1.\*\* Left: rest vs activation oxygen-rich blood displaces paramagnetic deoxyhaemoglobin, calming local field disturbances and raising T₂\* signal. Right: the response peaks 5–6 s after the neural event and is followed by an undershoot.

**Keep the chain honest.** BOLD = neural activity → neurovascular coupling → oxy/deoxy ratio → local field uniformity → T₂\* → GRE-EPI signal. fMRI conclusions inherit every link in that chain — including vascular disease, caffeine, breathing and scanner artefacts, none of which are neuroscience.

### 5.2 The haemodynamic response and its lag

The blood-flow response is slow: after a brief neural event, BOLD signal rises over ≈2 s, peaks at 5–6 s, and settles over 15–20 s, often with a post-stimulus undershoot. This **haemodynamic response function (HRF)** acts like a smearing filter between neural activity and measurement. Three practical consequences:

1. **Temporal resolution is seconds,** no matter how fast the scanner is, fMRI cannot order millisecond-scale neural events.
2. **Designs must be convolved with the HRF:** analysis asks which voxels' time series match the task-timing pattern after blurring by the HRF.
3. **Events too close together overlap:** block designs (20–40 s ON/OFF) maximize detectability; event-related designs trade power for trial-level resolution.

> \*\*Figure 5.2.\*\* A block design (top) and the voxel time series it predicts after convolution with the HRF (bottom). Statistical parametric mapping is, at heart, a correlation between each voxel's measured signal and this expected waveform.

### 5.3 Why fMRI needs EPI

BOLD changes are a few percent of signal, so fMRI must sample the whole brain every 1–2 s for several minutes and squeeze statistics out of the time series. Only EPI (Module 4.4) is fast enough: single-shot T₂\*-weighted GRE-EPI is the default fMRI acquisition. Every property of fMRI data, the distortion, the dropout near the orbitofrontal cortex and temporal poles, the sensitivity to head motion is an EPI property wearing a neuroscience hat.

> \*\*Figure 5.3.\*\* BOLD activation maps (warm colours = activation, cool = deactivation) overlaid on structural MRI during a motor task — the end product of the entire chain in this module.

### 5.4 Artefacts every fMRI scientist must know

|Artefact|Physical origin|Looks like|Standard mitigation|
|-|-|-|-|
|Susceptibility dropout|T₂\* destroyed near air-tissue interfaces|Signal voids: orbitofrontal, anterior temporal|Shorter TE, shimming, distortion correction|
|Geometric distortion|EPI readout|Warping near temporal lobes|Field-map or reverse-polarity (topup) correction|
|Head motion|k-space corruption between volumes|Spurious activation, group differences|Motion regressors, scrubbing|
|Physiological noise|Cardiac/respiratory alias into BOLD|Fluctuation at vessel/CSF locations|RETROICOR, ICA-based denoising|
|Scanner drift|Gradual instabilities|False long-term "activity"|Detrending|

**Module 5 take-home.** fMRI = T₂\* + neurovascular overshoot + EPI speed + statistics. The scanner sees blood, not thoughts; the HRF smears seconds; EPI supplies the speed and the artefacts. Understanding this module is the difference between analysing brain function and analysing a magnetic weather pattern.

\---

## Summary — The Chain of MRI

**B₀ aligns the spins** → ω = γB₀ **sets the frequency** → **B₁ tips M into the transverse plane** → *T₁/T₂/T₂ give tissues their signatures*\* → **gradients encode position as frequency and phase** → **k-space accumulates the coded signal** → **FFT reconstructs the image** → **TR/TE/TI/b choose the contrast** → **the sequence (SE/TSE/GRE/EPI/IR) is the timetable that makes it happen** → *BOLD fMRI exploits T₂ and blood flow to watch the brain work.*\*

Every term in that chain is now in your vocabulary. When a dataset behaves strangely — a model that fails on a new scanner, a lesion that appears on DWI but not ADC, an activation cluster beside the sinuses — walk the chain and ask at which link physics, not biology, is speaking.

\---

## Cheat Sheet \& Glossary

### One-line answers to the questions fellows actually ask

|Question|One-line answer|
|-|-|
|Why hydrogen?|Most abundant nucleus in the body and the loudest (γ = 42.58 MHz/T).|
|What does B₀ do?|Aligns spins, creates net M, sets precession frequency. Always on.|
|What does B₁ do?|Tuned RF pulse that tips M into the measurable plane (flip angle).|
|What do gradients do?|Make frequency/phase position-dependent: slice (z), phase (y), frequency (x).|
|T₁ vs T₂ in one line?|T₁ = regrowth along the field (energy out); T₂ = loss of sync across it (coherence gone). T₂ ≤ T₁.|
|What is T₂\*?|T₂ plus field-inhomogeneity decay; what GRE and EPI actually see; BOLD's currency.|
|What is k-space?|The raw frequency/phase data matrix; image = its inverse FFT. Centre = contrast, edge = detail.|
|Short TR + short TE?|T1-weighted (anatomy; fat bright, fluid dark).|
|Long TR + long TE?|T2-weighted (pathology; fluid bright).|
|How to kill CSF signal?|FLAIR: invert, image at TI \~ 2900 ms (CSF zero-crossing).|
|How to kill fat signal?|STIR: invert, image at TI \~ 180 ms (fat zero-crossing).|
|DWI bright = stroke?|Only if ADC is dark. DWI bright + ADC bright = T2 shine-through.|
|What is SWI for?|Blood products, iron, microbleeds, veins — anything that disturbs the local field.|
|Why is EPI everywhere?|Fills all of k-space in one shot; the only readout fast enough for DWI and fMRI.|
|What does fMRI measure?|Not neurons — the BOLD blood-flow overshoot via T₂\*, delayed \~5–6 s.|

### Glossary (selected)

|Term|Meaning|
|-|-|
|B₀|Main static magnetic field (1.5–7 T), produced by the superconducting magnet|
|B₁|Oscillating radiofrequency field used to excite spins|
|Flip angle (α)|Angle by which RF tips the net magnetization away from B₀|
|Larmor frequency|Spin precession frequency, ω = γB₀; the resonance condition for RF|
|Gyromagnetic ratio (γ)|Nucleus-specific constant linking field strength to precession frequency|
|TR / TE / TI|Repetition time / echo time / inversion time — the timing dials of contrast|
|b-value|Strength of diffusion weighting in DWI (s/mm²)|
|ADC|Apparent diffusion coefficient; quantitative map of water mobility computed from multi-b DWI|
|FID|Free induction decay — the raw decaying signal after one excitation|
|SAR|Specific absorption rate — RF power deposited in tissue (W/kg); safety-limited|
|BOLD|Blood-oxygen-level-dependent contrast — the basis of fMRI|
|HRF|Haemodynamic response function — the slow BOLD waveform following neural activity|
|Partial volume|Averaging of multiple tissues within one voxel, blurring borders and small lesions|
|SNR|Signal-to-noise ratio; rises with field strength, voxel size and averaging|

\---

## Further Reading

1. McRobbie, D. W., Moore, E. A., Graves, M. J., \& Prince, M. R. (2017). *MRI from Picture to Proton* (3rd ed.). Cambridge University Press. — The clearest intuition-builder; start here.
2. Westbrook, C., \& Talbot, J. (2018). *MRI in Practice* (5th ed.). Wiley-Blackwell. — The clinical workhorse text.
3. Haacke, E. M., Brown, R. W., Thompson, M. R., \& Venkatesan, R. (1999). *Magnetic Resonance Imaging: Physical Principles and Sequence Design*. Wiley-Liss. — The rigorous reference.
4. Bernstein, M. A., King, K. F., \& Zhou, X. J. (2004). *Handbook of MRI Pulse Sequences*. Elsevier Academic Press. — For Module 4 in full depth.
5. Nishimura, D. G. (2010). *Principles of Magnetic Resonance Imaging*. Lulu. — Superb on encoding and reconstruction.
6. Huettel, S. A., Song, A. W., \& McCarthy, G. (2014). *Functional Magnetic Resonance Imaging* (3rd ed.). Sinauer Associates. — The standard fMRI text for Module 5.
7. Prince, J. L., \& Links, J. M. (2014). *Medical Imaging Signals and Systems* (2nd ed.). Pearson. — The signal-processing perspective.

\---

## Figure Credits

Figures 1.1–1.3, 2.1–2.6, 3.1, 3.4, 3.5, 3.7, 4.1–4.4, 5.1–5.2 were generated specifically for these notes from the governing equations (Larmor equation, Bloch-equation relaxation, Fourier transform of a standard test phantom, canonical haemodynamic response model).

* Figure 3.2: Wikimedia Commons, file "T1t2PD.jpg".
* Figure 3.3: Case Western Reserve University, School of Medicine (Neurology), "MRI Basics".
* Figure 3.6: Stroke Manual, "MR-DWI in the acute stroke diagnosis".
* Figure 3.8: Stroke / American Heart Association Journals, figure from "Susceptibility-Weighted Imaging is More Reliable Than T2\*-Weighted Gradient-Recalled Echo MRI for Detecting Microbleeds".
* Figure 5.3: PLOS ONE, "Brain activation by a VR-based motor imagery and observation task: an fMRI study", published under a Creative Commons Attribution (CC BY) licence.

Clinical images are reproduced for non-commercial teaching within the ABDN fellowship programme; all rights remain with the original publishers. Please retain these credits if the notes are redistributed.

