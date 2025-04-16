---
title: Monomer prediction
---

::: {.callout-tip}
#### Learning Objectives

- Bulleted list of learning objectives
:::


## Section

Headings for material sections start at level 2. 

More guidelines for content available here: https://cambiotraining.github.io/quarto-course-template/materials/02-content_guidelines.html



## Exercise

1. Use default ColabFold flags and AlphaFold3 to predict the model for protein in “data/03_monomers/H1151_MTB_s2_116res.fasta”. Save results in the results folder.

2. Tweak the following ColabFold flags and remodel the MTB protein:-

  + num_seeds : 2
  + num_relax : 1
  + template_mode : pdb100
  + Leave other flags as default

3. Is there any improvement in the score (< PAE, > pLDDT) after tweaking flags parameters? Which model has higher pLDDT or smaller PAE?


## Exercise

In exercise 3 in the structural database question we attempted to look for the 3D structure of our unknown phage sequence (data/02_structural_databases/T1113_phage.fasta) using AlphaFoldDB. But we could not find the related structure for our sequence. Now you are asked to use ColabFold, and AlphaFold3 to model the structure of the sequence. In the results directory, create two folders for each of the individual software outputs. Remember to note down all parameter flags you used for each tool.

- Observe the plots and scores from each tool

- You can use spreadsheet tool to write down the best scores and their associated metric from each individual tools. 

- + Are there any notable differences in the scores between ColabFold and AlphaFold3 models?



## Summary

::: {.callout-tip}
#### Key Points

- Last section of the page is a bulleted summary of the key points
:::
