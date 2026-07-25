# Opening Questions: The Physics Blind Spot

**Instructions:** Find the question that hurts the most. That is your entry point. We will return to it at the end of the lecture.

\---

## For the Biochemists

1. You incubate a tissue sample, scan it ex vivo, and compare its "T1 relaxation time" to in vivo literature values. They do not match. Is your fixation protocol wrong, or did you confuse chemical environment with magnetic field coupling?
2. You see bright enhancement after gadolinium injection and conclude "increased vascular permeability." Could it instead be T1 shortening in a static pool of contrast with no blood flow at all? What does your sequence actually measure permeability or relaxivity?
3. You quantify metabolite concentrations from MR spectroscopy peaks. You ignore the B₀ inhomogeneity across the voxel. How much of your "biochemical gradient" is actually magnetic field distortion?

\---

## For the Neuroscientists

4. Your fMRI shows "activation" in the orbitofrontal cortex and anterior temporal lobes. You publish it as a cognitive finding. Did you verify whether those voxels lie in a susceptibility void where T₂\* signal is artifactually lost and regained? Is your "neural signal" just an air-tissue interface ghost?
5. Your DTI tractography resolves a beautiful arcuate fasciculus. But your b-value was 700 s/mm² and you acquired 12 directions. In a region with crossing fibers, did you measure diffusion anisotropy, or did you measure the limits of your gradient encoding?
6. You compare resting-state networks between a 3T and a 7T dataset. The 7T shows "stronger connectivity." Is that neuroscience, or is it RF penetration and flip angle variation across the cortex biasing your BOLD sensitivity?

\---

## For the Computer Scientists \& Engineers

7. You train a segmentation model on T2-weighted images from Scanner A (TE = 90 ms). You deploy on Scanner B (TE = 120 ms). CSF brightness shifts. Your model now labels ventricles as "pathology." You call it a domain adaptation problem. Is it, or is it a physics literacy problem?
8. You build a motion-correction network. You treat motion as a rigid 2D warp on image pixels, like a shaky photograph. But MRI fills k-space line by line over time. If the patient moves between phase-encoding steps, what is the mathematical structure of the corruption? Can a convolutional network ever learn to invert Fourier-domain phase errors without knowing where the lines came from?
9. You curate a "clean" dataset for MICCAI by excluding "low-quality" scans. You remove images with "dark bands" near the skull base. Those bands were flow artifacts from the carotid arteries — predictable, physiological, and removable with sequence knowledge. You have just excluded valid data and biased your benchmark. How would you know?

\---

## For the Clinicians

10. A stroke patient presents within 3 hours. The DWI is bright, the ADC is dark. You call it "acute infarct." But do you know why restricted diffusion appears bright on DWI? If the scanner uses a low b-value or a high b-value with poor SNR, could your "restricted diffusion" be T2 shine-through or noise?
11. You see a T1-hyperintense lesion and list "subacute haemorrhage, fat, melanin, protein" as your differential. You chose the sequence because it shows "anatomy." But T1 hyperintensity is not anatomy; it is short longitudinal relaxation. Does your differential change if you learn the sequence was acquired with an inversion time designed to null fat?
12. You order a "routine MRI brain" and miss a small cortical lesion. The radiographer used a 5 mm slice thickness with a 1 mm gap. The lesion is 4 mm. Did you miss it because of biology, or because of partial volume averaging and slice gap?

\---

## For the Anatomists \& Physiologists

13. You measure hippocampal volume from a T1-weighted atlas and compare it across species. But T1 contrast depends on field strength, hydration, and lipid content. Are you measuring anatomy, or are you measuring hydrogen proton relaxation in different magnetic environments?
14. You trace a muscle boundary on a 2D axial slice and extrapolate its 3D volume. Your in-plane resolution is 0.5 × 0.5 mm, but your slice thickness is 4 mm. You report the volume to two decimal places. How much of that precision is interpolation fantasy versus through-plane resolution reality?
15. You observe that "gray matter appears darker than white matter" on your T1 image and use it to define a structural atlas. But at 7T, the gray-white contrast inverts in some cortical layers due to myelin and iron content. Is your atlas describing neuroanatomy, or is it describing magnetic susceptibility at a specific frequency?

\---

## The Cross-Disciplinary Trap (For Everyone)

16. You collaborate on a multi-center study. Scanner A uses a spin-echo T2; Scanner B uses a turbo spin-echo T2 with a variable flip angle train. You pool the images and run a group analysis. You find a "significant regional difference." Is it biology, or is it blurring from stimulated echoes and k-space filtering that one scanner applies and the other does not?

\---

## Answer Key Map

|Question|Physics Concept|Where Answered|
|-|-|-|
|1, 5|T1 relaxation, spin-lattice, chemical exchange|Module 2.2|
|2|T1 shortening, contrast mechanisms|Module 3.5|
|3|B₀ inhomogeneity, shimming, chemical shift|Module 1.2 / 2.3|
|4|T₂\*, susceptibility artifacts, BOLD fMRI|Module 2.2 / 3.3|
|5|DWI/DTI, b-values, diffusion tensors|Module 3.6|
|6|RF penetration, flip angle, SAR, field strength|Module 1.2 / 3.3|
|7|TE, T2 weighting, contrast, domain shift|Module 2.2 / 3.1|
|8|k-space, phase encoding, motion in Fourier domain|Module 2.4|
|9|Flow artifacts, sequence parameters, data curation|Module 2.3 / 3.2|
|10|DWI, ADC, T2 shine-through, b-values|Module 3.6|
|11|T1 hyperintensity, inversion recovery, fat nulling|Module 3.5|
|12|Slice thickness, partial volume, slice gap|Module 2.3|
|13|T1 dependence on field strength, tissue composition|Module 2.2|
|14|In-plane vs. through-plane resolution, voxel anisotropy|Module 2.3|
|15|T1 contrast inversion, susceptibility, iron/myelin|Module 2.2 / 3.3|
|16|Spin echo vs. turbo spin echo, k-space filtering, blurring|Module 3.2|



