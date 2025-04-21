---
title: Multimer prediction
---

::: {.callout-tip}
#### Learning Objectives

- Bulleted list of learning objectives
:::


## Section

Headings for material sections start at level 2. 

More guidelines for content available here: https://cambiotraining.github.io/quarto-course-template/materials/02-content_guidelines.html


## Exercise

In exercise 4.2 in the File Format section, we managed to polish our sequence input by removing spaces and other unwanted characters. You were asked to save its output as “numb_delta_human_complex.fasta” in the directory “data/04_multimers/”. You are now required to:

- perform multimer predictions with ColabFold and AlphaFold3 to model interaction between DELTA homolog1 and NUMB homolog proteins. You are free to choose any flag options in your modelling, but it is good practice to note these flag values down for reproducibility.

- Record the confidence score of the model from each tool.

- Load each model into ChimeraX and observe their structures by rotating the models in different ways. Note any differences.


## Exercise

Model the protein target in FASTA file “data/06_chimerax/rcsb_pdb_1ERE.fasta” using ColabFold and AlphaFold3. Save the output in the folder “results/04_multimers/<tool_name>”.

- Note the score from each model.

- Which model has the highest score in pTM?

- Which fold/model looks similar to the reference PDB structure (1ERE)? 

::: {.callout-tip}
## Hint

Try to load 1ERE.pdb and structure model into ChimeraX and superimpose them. If you can’t do at the moment, we will cover later in the ChimeraX session.

:::

## Summary

::: {.callout-tip}
#### Key Points

- Last section of the page is a bulleted summary of the key points
:::
