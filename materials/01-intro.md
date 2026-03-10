# Introduction to protein structure prediction

:::{.callout-tip}
#### Learning objectives

- Define the concept of protein folding and the levels of protein structure (primary to quaternary).
- Explain why structure prediction is a central area of research in biological and life sciences.
- Describe the historical context of protein structure prediction and the challenges in the field.
- Recognise the transformative impact of deep learning approaches, particularly AlphaFold.
- Summarise the current state of the field and ongoing challenges.
:::

## Overview 

Proteins do most of the work in a cell. 
They build structures, catalyse reactions, transmit signals, and control gene expression. 
To do these jobs, proteins must adopt precise three-dimensional shapes.

A protein's **structure determines its function**. 
Change the shape, and the function often changes too.

This simple idea drives a large area of research: **predicting protein structure from its sequence**.

In these materials we will explore how modern computational methods predict protein structures and complexes. 
We will also examine how researchers analyse and interpret these models.

Before we begin, we need a few key ideas.

## Protein folding and levels of structure

Proteins are chains of amino acids. 
The order of those amino acids forms the **primary structure**.

Cells build this chain on the ribosome. 
At first it looks like a loose thread, but the chain does not stay that way for long, as it eventually folds.

Folding happens because amino acids interact with each other and with water: some attract, some repel, some form bonds. 
These interactions pull the chain into a stable shape.

We usually describe protein structure at four levels.

### Primary structure

The **primary structure** is simply the amino acid sequence.
Example:

```
Met-Ala-Leu-Gly-Lys...
```

This sequence contains all the information needed to fold the protein.

### Secondary structure

Short stretches of the chain fold into regular patterns called **secondary structures**.

The two most common are:

- **alpha helices** - spiral structures stabilised by hydrogen bonds
- **beta sheets** - extended strands that pair to form sheets

These elements form the scaffolding of many proteins.

### Tertiary structure

The **tertiary structure** describes the full three-dimensional fold of a single protein chain.

At this level we see:

- how helices and sheets pack together
- where pockets and binding sites appear
- how the protein interacts with ligands or other molecules

This is the level most often shown in molecular graphics programs such as ChimeraX.

### Quaternary structure

Many proteins work as **complexes** of several chains.
The arrangement of these chains forms the **quaternary structure**.

Examples include:

- enzyme complexes
- antibody assemblies
- transcription factor dimers
- membrane channels

In these materials we will often examine these assemblies. 
They reveal how proteins cooperate to perform biological functions.

## Why predict protein structures?

Biologists can determine protein structures experimentally. 
The most common methods include:

- **X-ray crystallography**
- **NMR spectroscopy**
- **cryo-electron microscopy**

These techniques produce detailed structures, but they require specialised equipment and time.
As a result, we know the sequences of **hundreds of millions of proteins**, but we have solved structures for only a small fraction of them.

This gap creates a clear problem: We know the letters of the sequence, but we do not know the shape.

Structure prediction aims to close that gap.
Accurate structure models help researchers:

- understand how proteins work
- identify binding sites for drugs or ligands
- interpret disease mutations
- study protein evolution
- design new proteins and enzymes

In short, structure helps us turn sequence data into biological insight.

## A long-standing challenge

Predicting protein structure from sequence has challenged scientists for decades.
The core problem seems simple:

> Given an amino acid sequence, predict the structure it will fold into.

In practice, the problem is extremely hard.
A protein chain can adopt an enormous number of possible shapes. 
Each shape corresponds to a different arrangement of atoms, however, only a few of these shapes are stable.

Predicting the correct fold means finding the **lowest-energy structure** among a vast number of possibilities.

## Early computational approaches

Early methods relied on two main ideas.

### Physics-based models

Some approaches attempted to simulate the physics of folding. 
They calculated forces between atoms and searched for the lowest-energy structure.

In theory this approach should work. 
In practice it demands enormous computing power.
Even today, simulating full protein folding remains difficult.

### Comparative modelling

Another approach used **homology**.

Proteins that share similar sequences often share similar structures. 
If researchers already solved the structure of a related protein, they could model the new one by comparison.

This method works well when a close structural relative exists. 
But it struggles with proteins that lack known homologues.

## The CASP experiments

To measure progress, the community created a regular blind test called **CASP** (Critical Assessment of Structure Prediction).
In CASP:

1. Experimental groups determine new protein structures but keep them secret.
2. Prediction groups attempt to model those proteins from sequence alone.
3. Organisers compare the predictions with the experimental structures.

CASP provides an objective way to measure how well prediction methods perform.
For many years, progress was steady but slow.

Then the field changed.

## Deep learning and AlphaFold

In recent years, machine learning methods have improved many areas of science. 
Protein structure prediction is one example.

Deep learning models analyse large datasets of known protein structures and sequences. 
From these data they learn patterns that relate sequence to structure.

One system in particular drew wide attention: **AlphaFold**, developed by DeepMind.

AlphaFold uses deep neural networks to predict:

- distances between residues
- angles within the protein backbone
- overall structural geometry

It combines these predictions into a final three-dimensional model.

In CASP14 (2020), AlphaFold achieved a level of accuracy close to many experimental structures. 
This result marked a major advance in the field.

Since then, researchers have applied AlphaFold and related tools to predict structures across entire proteomes.
Large public resources now exist, including the **AlphaFold Protein Structure Database**, which contains predicted structures for millions of proteins.

## The current state of the field

Today, structure prediction plays a central role in molecular biology.
Researchers routinely use predicted models to:

- explore protein function
- analyse mutation effects
- design experiments
- study protein complexes

Tools continue to improve. New systems can predict:

- protein multimers
- protein–DNA interactions
- protein–ligand complexes

These capabilities allow us to model increasingly realistic biological systems.

However, challenges still remain, as prediction methods still struggle with:

- highly flexible or disordered regions
- large multi-component assemblies
- transient interactions
- subtle conformational changes

Researchers must also learn how to **interpret prediction confidence** and recognise when a model may be unreliable.

## What you will learn

These materials focus on practical analysis of predicted protein structures.

You will learn how to:

- search structural databases such as UniProt and AlphaFoldDB
- predict protein structures using modern tools
- compare predicted and experimental structures
- analyse complexes and interfaces
- interpret prediction confidence scores
- visualise and analyse structures in ChimeraX
