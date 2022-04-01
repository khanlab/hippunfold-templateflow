# hippunfold-templateflow

Hippocampus & dentate gyrus surfaces and subfield segmentations, by running HippUnfold on templates in templateflow.

All surfaces have corresponding vertices, and come in three different densities (0p5mm, 1mm, 2mm) which relate to their approximate vertex spacing.


Potential use cases:
 1. Sampling volumetric data to hippocampal surfaces (`wb_command -volume-to-surface-mapping`)
 2. Mapping hippocampal surface data (metric/label) to a standard template space (`wb_command -{metric/label}-to-volume-mapping`)

Note: you have the choice of interpolation with `wb_command` and can perform ribbon-style mapping by making use of the inner and outer surfaces.

Volumetric ROIs of the subfields are also provided in each space (`_dseg.nii.gz`). These relate to the `res-1` or `res-01` T1w images.



## How were these files created?

These steps were carried out:
 1. pull template T1w images from template flow into `template_images/tpl-{subject}_res-1_T1w.nii.gz`
 2. rename res-01 to res-1 to make naming consistent and life easier
 3. run hippunfold on those templates:
    hippunfold - hippunfold-templates participant -p --keep-work --modality T1w --path_T1w=template_images/tpl-{subject}_res-1_T1w.nii.gz --cores all  --use-singularity  --output-density 1mm 2mm 0p5mm
 4. copy out dseg and surf.gii files in space-T1w:
    cp  hippunfold-templates/hippunfold/sub-*/anat/*space-T1w_*dseg.nii.gz hippunfold-templates/hippunfold/sub-*/surf/*space-T1w*label-{hipp,dentate}_{inner,outer,midthickness}.surf.gii .
 5. rename sub- to tpl-, and put back in tpl- subfolders


## Acknowledgements:

This work is entirely dependent on templateflow and hippunfold:

TemplateFlow: a community archive of imaging templates and atlases for improved consistency in neuroimaging R Ciric, R Lorenz, WH Thompson, M Goncalves, E MacNicol, CJ Markiewicz, YO Halchenko, SS Ghosh, KJ Gorgolewski, RA Poldrack, O Esteban bioRxiv 2021.02.10.430678; doi: 10.1101/2021.02.10.430678 

HippUnfold: Automated hippocampal unfolding, morphometry, and subfield segmentation
Jordan DeKraker, Roy AM Haast, Mohamed D Yousif, Bradley Karat, Stefan Köhler, Ali R Khan
bioRxiv 2021.12.03.471134; doi: https://doi.org/10.1101/2021.12.03.471134 


See www.templateflow.org and https://github.com/khanlab/hippunfold for more information.
