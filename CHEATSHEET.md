# MRI Physics Cheat Sheet

Quick-reference tables for the ABDN MRI Physics lecture. 

\---

## The Three Magnetic Fields

|Field|Symbol|Strength|Role|Always On?|
|-|-|-|-|-|
|Main static|B₀|1.5–7 T|Aligns spins, sets Larmor frequency|Yes|
|Radiofrequency|B₁|\~μT|Tips M into transverse plane (flip angle)|No — pulsed|
|Gradients|Gₓ, Gᵧ, Gz|\~mT/m|Encode position: slice, phase, frequency|No — switched|

\---

## Larmor Frequency

$$\\omega = \\gamma B\_0$$

|B₀|ω (¹H)|Use|
|-|-|-|
|1.5 T|\~63.9 MHz|Routine clinical|
|3.0 T|\~127.7 MHz|High-res neuro, fMRI|
|7.0 T|\~298.1 MHz|Research, cortical layers|

\---

## Relaxation Times (Brain at 3T, approximate)

|Tissue|T₁ (ms)|T₂ (ms)|T1w|T2w|
|-|-|-|-|-|
|Fat|\~260|\~80|Bright|Intermediate|
|White matter|\~790|\~80|Brighter than grey|Darker than grey|
|Grey matter|\~920|\~90|Intermediate|Intermediate|
|CSF|\~4200|\~2200|Dark|Very bright|
|Muscle|\~900|\~45|Intermediate|Dark|

**Remember:** T₂ ≤ T₁ always. T₂\* ≤ T₂ always.

\---

## Contrast Matrix: TR vs TE

||Short TE (<30 ms)|Long TE (>80 ms)|
|-|-|-|
|**Short TR (<1000 ms)**|**T1-weighted** — anatomy, fat bright, fluid dark|Mixed — not used|
|**Long TR (>2000 ms)**|**PD-weighted** — structural detail|**T2-weighted** — pathology, fluid bright|

\---

## Sequence Families

|Sequence|Echo Mechanism|k-space/TR|Weighting|Speed|Key Weakness|
|-|-|-|-|-|-|
|**SE**|180° RF refocusing|1 line|T1, T2, PD|Slow|—|
|**TSE/FSE**|Train of 180° pulses|ETL lines|T2 (routine)|Fast|Blurring; fat bright|
|**GRE**|Gradient reversal|1 line|T2\*, T1|Fast|Susceptibility artefacts|
|**EPI**|Oscillating gradient|Whole grid (single shot)|T2\*, DWI|Fastest|Distortion, ghosting, dropout|
|**IR**|180° prep + readout|Varies|T1-controlled|Slow|TI must match T₁|

\---

## Special Sequences

|Sequence|Physics|What It Does|Clinical Use|
|-|-|-|-|
|**FLAIR**|IR nulls CSF (TI \~2900 ms)|CSF black, lesions bright|Periventricular lesions, MS|
|**STIR**|IR nulls fat (TI \~180 ms)|Fat black, oedema bright|Marrow oedema, MSK|
|**DWI**|Diffusion gradients (b-values)|Bright = restricted water|Acute stroke, abscess, tumour|
|**ADC**|Calculated from multi-b DWI|Dark = true restriction|Confirms DWI; rules out T2 shine-through|
|**SWI**|Long-TE 3D GRE + phase mask|Black = blood, iron, veins|Microbleeds, haemorrhage, cavernoma|

\---

## DWI Interpretation Rule

|DWI|ADC|Meaning|
|-|-|-|
|Bright|Dark|**True restricted diffusion** — acute infarct, abscess, highly cellular tumour|
|Bright|Bright|**T2 shine-through** — artefact, not restriction|
|Dark|Bright|Facilitated diffusion — vasogenic oedema, chronic gliosis|

**Never call restricted diffusion without checking the ADC map.**

\---

## fMRI Essentials

|Concept|Fact|
|-|-|
|What fMRI measures|BOLD signal — blood flow overshoot, not neurons|
|Sequence|T₂\*-weighted single-shot GRE-EPI|
|Peak response|\~5–6 seconds after neural event|
|Temporal resolution|Seconds (limited by HRF, not scanner)|
|Key artefacts|Susceptibility dropout, geometric distortion, motion, drift|
|Why EPI?|Only readout fast enough for whole-brain coverage every 1–2 s|

\---

## k-Space Rules

|Region|Contains|If Lost|
|-|-|-|
|**Centre**|Low spatial frequencies — contrast, signal|Faint outline, no brightness|
|**Periphery**|High spatial frequencies — edges, detail|Blurry blob, correct contrast|

**Motion corrupts k-space, not pixels.** Phase-encode direction ghosts across the image.

\---

## Field Strength Trade-offs

|Property|1.5T → 3T → 7T|
|-|-|
|SNR|↑ Linear increase|
|T₁|↑ Lengthens|
|T₂\* effects|↑ Stronger|
|SAR (heating)|↑ Binds harder|
|B₁ uniformity|↓ Worsens (shading at 7T)|
|Chemical shift|↑ Larger fat/water misregistration|

**Multi-center rule:** Field strength is a study variable, not a scanner spec. 1.5T and 3T images are not interchangeable inputs.

\---

## The Chain of MRI (One Line)

B₀ aligns → B₁ tips → T₁/T₂/T₂\* signatures → gradients encode position → k-space accumulates → FFT reconstructs → TR/TE/TI/b choose contrast → sequence executes → BOLD fMRI watches blood.

\---

## Glossary (Ultra-Short)

|Term|Meaning|
|-|-|
|B₀|Main static field (1.5–7 T)|
|B₁|RF excitation field|
|TR|Repetition time — between excitations|
|TE|Echo time — excitation to readout|
|TI|Inversion time — for IR sequences|
|α|Flip angle — how far B₁ tips M|
|γ|Gyromagnetic ratio — 42.58 MHz/T for ¹H|
|b-value|Diffusion weighting strength (s/mm²)|
|ADC|Apparent diffusion coefficient|
|ETL|Echo train length (TSE/FSE)|
|SAR|Specific absorption rate (RF heating limit)|
|SNR|Signal-to-noise ratio|
|HRF|Haemodynamic response function|
|BOLD|Blood-oxygen-level-dependent contrast|
|FID|Free induction decay|

\---

*"When a dataset behaves strangely — a model that fails on a new scanner, a lesion that appears on DWI but not ADC, an activation cluster beside the sinuses — walk the chain and ask at which link physics, not biology, is speaking."*

