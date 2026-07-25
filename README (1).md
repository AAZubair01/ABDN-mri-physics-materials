# MRI Physics for the Imaging Scientist

**Magnetic Fields · Image Formation · Contrast Weightings · Pulse Sequences · fMRI Physics**

\---

## About This Repository

This repository contains the complete lecture materials for the **ABDN Fellowship Teaching Series** on MRI physics, prepared for an interdisciplinary audience of imaging scientists who use MRI data but do not necessarily acquire it.

### Prepared by

**Abdulrazaq A. Zubair**, B.Rad, MSc  
PhD Candidate, Bayero University Kano  
Federal University of Health Sciences, Azare  
*Affiliated with AKTH / Medserve, Kano, Nigeria \& African Institute for Research advancement \& Innovation* 

*(AIRA AFRICA)*

### Prepared for

Fellows of the **ABDN Programme**

* Radiography
* Computer Science
* Medicine
* Neuroscience
* Anatomy
* Engineering

### Level

Conceptual to intermediate — MRI/fMRI cohort

### Version

July to November 2026

\---

## Why These Notes Exist

You will probably never stand at the scanner console and choose a repetition time. But every dataset you will ever touch was shaped by someone who did — and the choices they made live inside your pixels.

* A model trained without knowing what **TE** does will fail on a new scanner and you will not know why.
* An fMRI result interpreted without knowing what **T₂**\* is may be an artefact rather than a finding.
* A multi-center study that pools 1.5T and 3T images without accounting for field strength will publish phantom biology.

These notes exist to make you **literate in the language of the machine that generates your data**.

\---

## Repository Structure

```
.
├── README.md                          # This file
├── mri\_physics\_lecture.md             # Full lecture notes (Modules 1–5)
├── opening\_questions.md               # Six discipline-specific opening questions
├── CHEATSHEET.md                      # Quick-reference tables and glossary
└── slides/                            # PDF slide deck (if uploaded)
    └── mri\_physics\_slides.pdf
```

\---

## The Five Questions These Notes Answer

1. What are the three magnetic fields inside the scanner, and what does each one do? *(Module 1)*
2. How does a signal from a spinning proton become a pixel with an address? *(Module 2)*
3. Why does the same brain look completely different on T1w, T2w, FLAIR, DWI and SWI? *(Module 3)*
4. What do the pulse-sequence acronyms (SE, TSE, GRE, EPI, IR) actually mean physically? *(Module 4)*
5. How can a scanner that sees water protons possibly image a thought? *(Module 5)*

\---

## How to Use These Notes

Each module follows the same rhythm:

1. **Plain-language picture first** — an analogy you can repeat at a dinner table.
2. **Physics stated precisely** — with the one or two equations that genuinely matter.
3. **Worked examples and clinical images** — bridging theory to practice.
4. **"Why you should care"** — tied directly to research and engineering practice.

Nothing here requires more than secondary-school mathematics to read. Everything here is stated accurately enough to defend in a viva.

\---

## Core Philosophy

> \*\*MRI physics is not optional metadata. It is the experimental condition of every neuroimaging and AI study.\*\*

Whether you write Python, design experiments, trace structures, or interpret scans, your raw material is k-space data transformed by human decisions:

* **TR** decides how much T₁ weighting leaks into your T₂ image.
* **Bandwidth** decides how much chemical shift misregistration distorts your segmentation.
* **Parallel imaging** introduces noise amplification that looks like biological heterogeneity.
* **Field strength** (1.5T vs 3T vs 7T) changes everything: SNR, SAR, susceptibility, RF penetration.

These are not "scanner settings." They are **the experimental conditions of your study**.

\---

## Citation

If you use these materials in your research or teaching, please cite:

```
Zubair, A. A. (2026). MRI Physics for the Imaging Scientist: 
Magnetic Fields, Image Formation, Contrast Weightings, 
Pulse Sequences, and fMRI Physics. ABDN Fellowship Teaching Series.
```

\---

## Licence

These notes are prepared for non-commercial teaching within the ABDN fellowship programme. Clinical images retain the rights of their original publishers; see `mri\_physics\_lecture.md` for full figure credits.

\---

## Contact

For questions, corrections, or collaborations:  
**Abdulrazaq A. Zubair** — aazubair01@gmail.com

*"When a dataset behaves strangely, walk the chain and ask at which link physics, not biology, is speaking."*

