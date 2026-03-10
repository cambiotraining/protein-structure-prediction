# Model comparison

In this session we will use a protein called SLC52A2 (UniProt ID: Q9HAB3), a human protein that currently has no experimentally-solved structure.
This is a membrane transporter for vitamin B2/riboflavin. 
Some mutations in SLC52A2 can cause a childhood onset motor neuron disease known as Brown-Vialetto-Van Laere syndrome. 
These mutations are thought to reduce protein expression or reduce riboflavin uptake

```bash
alphafold fetch Q9HAB3 version 6
hide atoms
```

## Searching for homologous structures

We can use FoldSeek to search for homologous structures in the PDB.
This can be done from the FoldSeek webserver, or from within ChimeraX (Tools > Structure Comparison > FoldSeek).

One of the first hits is the structure of `6ob6`. 
We can open this in the same session:

```bash
open 6ob6
label delete
hide atoms
```

We can check how our models and chains are named:

```bash
info chains
```

```
chain id #1/A chain_id A
chain id #2/A chain_id A
chain id #2/B chain_id B
```

We can see that the AlphaFold model is in model #1, and the PDB structure is in model #2, with chains A and B.

We are now ready to align the two structures using the Matchmaker command:

```bash
mm #2 to #1 showAlignment true
```

- The `showAlignment true` option will show the pairwise sequence alignment in the Sequence Viewer.

We can visualise our predicted structure as a cartoon again, and colour it by the RMSD to the experimentally solved structure. 
If you examine the residue attributes (`c`), you will see that there is an attribute called `seq_rmsd`, which gives the RMSD of each residue to the aligned structure.

We can therefore colour by this attribute: 

```bash
select #1
color #1 white
color byattribute r:seq_rmsd #1 target csab palette RdYlBu key true
select clear
```

Knowing the alignment is only to chain B, we can hide the other chain to make it easier to see the differences:

```bash
hide cartoon
cartoon #1 #2/B
```

## Exercises

**Reminder:** 
In this exercise series you are investigating the structure of the amphioxus estrogen receptor and comparing it with experimentally determined human ER structures. 

:::{.callout-exercise}
#### Comparing two model predictions

In the [monomer prediction exercises](04-monomer.md), we predicted the structure of the full amphioxus ERα protein using AlphaFold3. 
We will now align and compare this model with the prediction available in AlphaFoldDB, which was generated using AlphaFold2.

Run the following code in ChimeraX to open and align the two models:

```bash
close
cd ~/Course_Materials/er_amphioxus_full_monomer_af3
open fold_er_amphioxus_full_monomer_af3_model_0.cif
alphafold fetch B3V8B7 version 6
color #2 steelblue
mm #2 to #1
```

**Questions:**

1. What do you observe about the alignment of the two models?
   - Consider both the agreement within individual domains and the relative orientation of the domains.
   - Focus in particular on the DNA-binding (294-370) and ligand-binding (441-682) domains.

2. Given the issues you see when aligning the entire protein, can you think of a different way to approach this comparison?

:::{.callout-answer}

1. Aligning the entire protein produces a suboptimal alignment. 
   The ligand-binding domain aligns well between the two models, but the DNA-binding domain does not.

   This occurs because the two models predict slightly different orientations for the domains relative to each other. 
   The flexible linker between the domains introduces uncertainty in their relative positioning, which makes a global alignment of the entire protein less reliable.

2. A better approach is to align each domain separately.

   - First align the DNA-binding domain:

   ```bash
   hide cartoon
   cartoon :294-370
   mm #2:294-370 to #1:294-370
   ```

   - Then align the ligand-binding domain:

   ```bash
   hide cartoon
   cartoon :441-682
   mm #2:441-682 to #1:441-682
   ```

   When aligning the domains separately, the DNA-binding domain shows very strong agreement between the two models. 
   The ligand-binding domain also aligns well overall, although there are small differences near the start of the domain, where the AlphaFold3 model predicts a slightly longer alpha-helix than the AlphaFold2 model.

   This demonstrates that while the internal structure of each domain is predicted consistently, the relative orientation of domains in flexible multidomain proteins may vary between models.
:::
:::

:::{.callout-exercise}
#### Comparing multiple models

In the [monomer prediction exercises](04-monomer.md), we predicted the structure of the ligand-binding domain (LBD) of the ER protein using ColabFold (AlphaFold2).

In the second run of the AlphaFold2 predictions, we used **four random seeds** for each of the **five AlphaFold2 neural network models**, resulting in a total of **20 structure predictions**.

We can use this ensemble of models to evaluate how consistent the predicted structure is for this domain.

Run the following commands in ChimeraX to open the models and align them:

```bash
close
cd ~/Course_Materials/er_amphioxus_lbd_monomer_af2_run2/
open *unrelaxed*.pdb
mm #2-20 to #1
```

- `close` starts a new ChimeraX session.
- `cd` moves to the directory containing the prediction outputs.
- `open` loads all structure files matching the pattern `*unrelaxed*.pdb` (the `*` is known as a "wildcard" and matches any characters in the file names).
- `mm` (MatchMaker) sequentially aligns models `#2-20` to model `#1`, which is used as the reference structure.

**Questions:**

1. What can you conclude about the consistency of the predicted structures across the different models and random seeds?

:::{.callout-answer}

1. After aligning all 20 models, we observe that:

   - The core of the ligand-binding domain shows very strong structural agreement across all models.
   - The main variation occurs near the N-terminus and C-terminus, which correspond to regions flanking the annotated LBD domain.
   - These terminal regions appear more flexible and are predicted less consistently.

   Overall, the strong agreement in the core of the domain suggests that the predicted fold of the LBD is robust and not sensitive to the choice of AlphaFold model or random seed. 
   Variation at the termini is common in structure predictions and often reflects flexible regions of the protein.

To make our comparisons visually easier, we could colour the structures in different ways: 

- Give each structure its own colour (the default, when importing multiple structures):

    ```bash
    color bymodel
    ```

- Use a sequential colour palette from the N-terminus to the C-terminus:

    ```bash
    rainbow
    ```
:::
:::

:::{.callout-exercise}
#### Comparing predictions with resolved structures

Finally, we will compare our _de novo_ prediction of the LBD region using AlphaFold3 with an experimentally determined human structure that includes the estradiol ligand (PDB: **1ERE**).

The PDB entry contains three assemblies of the receptor as a homodimer (i.e. a total of six chains). 
For simplicity, we will keep only one chain for the comparison:

```bash
close
open 1ERE
delete /B-F
hide atoms
show cartoon
```

Next, open the AlphaFold3 prediction of the amphioxus ligand-binding domain:

```bash
cd ~/Course_Materials/er_amphioxus_lbd_monomer_af3/
open fold_er_amphioxus_lbd_monomer_af3_model_0.cif
```

**Questions:**

1. Align the two structures using the PDB structure as the reference. Colour the amphioxus prediction by RMSD.
2. Display the ligand atoms in stick style, colour them `gold`, and add a ligand surface with 50% transparency.
3. What can you conclude about the alignment of the amphioxus prediction, particularly around the ligand-binding region?
   - Hint: hiding the reference PDB model may help you see the coloured RMSD values.

:::{.callout-answer}

1. We align the structures using the MatchMaker command and colour the prediction by RMSD relative to the experimental structure:

    ```bash
    mm #2 to #1 showalignment true
    color byattribute r:seq_rmsd #2 target csab palette RdYlBu key true
    ```

    This colours residues in the predicted structure according to their structural deviation from the reference.

2. We display the ligand in stick representation:

    ```bash
    show ligand
    style ligand stick
    color ligand gold
    surface ligand color gold transparency 50
    ```

3. After hiding the cartoon representation of the reference structure (`hide #1 cartoon`), we can more easily see the RMSD colouring on the amphioxus prediction.

   We can see that residues forming the ligand-binding pocket show **low RMSD values**, indicating strong structural agreement with the experimentally determined human structure. 
   This suggests that the architecture of the ligand-binding pocket is highly conserved, even between distantly related species such as amphioxus and humans.

TODO: add rotating gif of the expected final result

This analysis illustrates how key functional regions of proteins can remain structurally conserved over long evolutionary timescales.
:::
:::

## Summary

- Structural comparison tools (e.g. FoldSeek or structural alignment methods) can identify related proteins even when sequence similarity is low.
- Conserved protein domains often maintain similar three-dimensional folds across large evolutionary distances.
- Structural alignment allows comparison of predicted models with experimentally determined structures.
- Example: In the ER exercises, structural alignment can reveal similarities between the amphioxus receptor domains and experimentally determined structures from vertebrates.