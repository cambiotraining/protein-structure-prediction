---
title: Structure analysis and visualisation
---

::: {.callout-tip}
#### Learning Objectives

- Bulleted list of learning objectives
:::


## Section

Headings for material sections start at level 2. 

More guidelines for content available here: https://cambiotraining.github.io/quarto-course-template/materials/02-content_guidelines.html

::: {.callout-tip collapse="true"}
#### Estrogen Receptor (PDB: 1ERE)

We will be using the structure of the ligand binding domain of the Estrogen Receptor (PDB: 1ERE).

The Estrogen Receptor is:

- A member of the Nuclear Receptor family of transcription factors

- Implicated in some breast and ovarian cancers

- Activates gene expression upon binding of its ligand, estradiol

- Targeted by several FDA-approved drugs

:::

:::{.callout-exercise}
#### Exercise - Selections using the CLI
Spend 1-2 minutes now trying to select different parts of the protein:

- Select chain B and chain D in the model

- Select the CA atom (alpha carbon) on residue 510 of chain C

- Select everything except chain E

:::

:::{.callout-exercise}
#### Sequence viewer

Consider the sequence below:

![](../course_files/images/chimera_exercise3.png)

Why are some residues in a black border?

:::

:::{.callout-exercise}
#### Structural Analysis - Revealing other interactions

Analysing the protein-ligand interactions of the Estrogen Receptor (1ERE).

Consider the following diagram:

![](../course_files/images/chimera_exercise4.png)

What does this information tell us about potential mutations at these positions?

:::

::: {.callout-exercise}
#### Wildtype-mutant analysis of predicted structures

Analysing mutations in SLC52A2 which can cause a childhood onset motor neuron disease known as Brown-Vialetto-Van Laere syndrome. 

These mutations are thought to reduce protein expression or reduce riboflavin uptake

Drag your mouse across the PAE plot below to highlight the corresponding residues on the protein

![](../course_files/images/chimera_exercise5.png)

What does this PAE plot tell us about the relative positions of the main parts of this protein?

:::

:::{.callout-exercise}
### Fetching information from AlphaMissense

With the following command: `cartoon byattribute r:avg #!1` and the images below:

![](../course_files/images/chimera_exercise6.png)

Which areas of the structure are least tolerant to mutations and why?

:::


## Summary

::: {.callout-tip}
#### Key Points

- Last section of the page is a bulleted summary of the key points
:::
