---
title: File formats
---

::: {.callout-tip}
#### Learning Objectives

- Bulleted list of learning objectives
:::


## Section

Headings for material sections start at level 2. 

More guidelines for content available here: https://cambiotraining.github.io/quarto-course-template/materials/02-content_guidelines.html

:::{.callout-exercise}
#### Cleaning FASTA sequence file

Some bioinformatics tools that are used for predicting 3D structure model require to reformat or polishing your sequences (removing space, or other characters) before you start to run the tools.
In this exercise you are given a text file with the amino acid sequences of two proteins (multimers). These sequences have spaces, line breaks and other non-protein characters. Use the [Sequence Manipulation Site (SMS)](https://www.bioinformatics.org/sms2/filter_protein.html) tool to remove these characters. 

The file is in the directory “data/01_file_formats/numb_delta_human_complex.txt”. When the tools finished to run, copy the sequences to the text file using text editor of your choice and save the file in FASTA format as “numb_delta_human_complex.fasta”. Use terminal to copy this file to the directory “data/04_multimers/”

:::

:::{.callout-exercise}
#### Converting cif file to PDB

Convert the .cif file into .pdb (PDBx/mmCIF) format by using the [mmCIF to PDB converter](https://project-gemmi.github.io/wasm/convert/cif2pdb.html) tool. The file is “data/01_file_formats/af3_fold_phage_model_0.cif” Once the conversion is complete, download and copy the file to “results/03_monomers/exercise2/af3_fold_phage_model_0.pdb” directory.

:::

## Summary

::: {.callout-tip}
#### Key Points

- Last section of the page is a bulleted summary of the key points
:::
