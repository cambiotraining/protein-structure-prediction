# Missense variants

## Functional mutation consequences

Mutations in proteins can have very different functional effects depending on **where in the structure they occur**.  
Residues that are structurally important - for example those involved in ligand binding, DNA recognition, or dimerisation - are often **less tolerant to mutation**.

## Working with AlphaFold models

You can obtain predicted structures from AlphaFold Database using the `alphafold fetch` command: 

```bash
alphafold fetch Q9HAB3 version 6
```

{{< mol-afdb Q9HAB3 >}}

In this case, we explicitly define the model version we want to download, as this may not always default to the latest available. 
By default the structure is coloured by pLDDT (local structure confidence) score. 

We can obtain the PAE score matrix, using the `alphafold pae` command:

```bash
alphafold pae uniprotId Q9HAB3 version 6
```

This opens a PAE matrix heatmap, that you can zoom-in and out of to higlight the respective regions of the protein.

To colour the protein by pLDDT score again:

```bash
colour byattribute pLDDT_score palette alphafold
```

or: 

```bash
colour bfactor palette alphafold
```

- See the available palettes in the [palettes documentation page](https://www.cgl.ucsf.edu/chimerax/docs/user/commands/palettes.html). 
- See the available residue attributes using the `info resattr` command.

## UniProt annotations

You can add additional features to the AlphaFold model, such as mutations, transmembrane regions, etc., by opening the corresponding files from UniProt:

```bash
open Q9HAB3 from uniprot format uniprot
```

This opens a window with the sequence and annotated features. 
You can click on the features to select the corresponding residues in the structure.

## AlphaMissense mutation scores

The **AlphaMissense** model predicts the likely impact of every possible amino acid substitution in a protein sequence.  
Each mutation receives a score indicating whether it is predicted to be **benign (tolerated)** or **deleterious (damaging)**.



We can obtain AlphaMissense scores:

```bash
open Q9HAB3 from alpha_missense format amiss
```

```bash
mutationscores label #1 amiss height 3 palette bluered
```

```bash
mutationscores define avg fromScore amiss setAttribute true combine mean
label delete
color byattribute r:avg #!1 palette bluered key true
cartoon byattribute r:avg #!1
```

TODO:

- Change the range of the colour palette from 0 - 1

## Exercises

:::{.callout-exercise}
#### Mutation scores for ER

In this exercise we will map predicted mutation sensitivity onto the **human estrogen receptor alpha (ERα)** structure.

**Tasks:**

1. Load the AlphaFold model for human ERα (UniProt: **P03372**) together with the predicted alignment error (PAE).
2. Load the AlphaMissense mutation scores.
3. Compute the **average mutation score per residue** and map it onto the structure.
4. Colour the structure according to the predicted mutation sensitivity and identify regions that appear particularly constrained.

What parts of the protein appear most sensitive to mutation?

:::{.callout-answer}

1. We first load the AlphaFold structure and its associated PAE matrix:

    ```bash
    close
    alphafold fetch P03372 version 6
    alphafold pae uniprotId P03372 version 6
    ```

2. Next we load the UniProt annotation and the AlphaMissense mutation scores:

    ```bash
    open P03372 from uniprot format uniprot
    open P03372 from alpha_missense format amiss
    ```

3. Each residue has many possible amino acid substitutions.
   We compute the **average predicted mutation effect per residue**:

    ```bash
    mutationscores define avg fromScore amiss setAttribute true combine mean
    ```

4. Finally, we map these scores onto the protein structure:

    ```bash
    color byattribute r:avg #!1 palette bluered key true
    cartoon byattribute r:avg #!1
    ```

   - In this colouring scheme:
     - **Blue residues** are more tolerant to mutation.
     - **Red residues** are predicted to be more deleterious when mutated.

We can use the UniProt window to highlight important annotated regions of the protein. 
We observe that mutation-sensitive residues cluster in functionally important regions of the receptor, including:

- the **DNA-binding domain**
- the **ligand-binding pocket**
- the binding sites.

These regions are strongly constrained because mutations there are more likely to disrupt protein function.

Mapping mutation sensitivity onto the structure reveals that **deleterious mutations cluster in functional and structurally critical regions of the protein**, showing how evolutionary and structural constraints shape where mutations can occur.
:::
:::

## Summary

- The functional impact of mutations often depends on their structural location.
- Variants occurring in conserved structural regions are more likely to affect protein stability or function.
- Computational tools such as AlphaMissense can help prioritise mutations for further study.
- Example: In the ER exercises, analysing mutations within the structured domains helps illustrate how structural context informs variant interpretation.