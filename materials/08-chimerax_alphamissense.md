# Mapping mutation effects onto structures

:::{.callout-tip}
#### Learning Objectives

- Visualise predicted protein structures from AlphaFold Database.
- Inspect model confidence using pLDDT and PAE scores.
- Load and explore protein annotations from UniProt.
- Map AlphaMissense mutation scores onto protein structures.
- Interpret how structural context influences mutation sensitivity.
:::

## Functional mutation consequences

Mutations can affect proteins in many different ways.

A **missense mutation** replaces one amino acid with another.
The impact of such mutations depends strongly on **where they occur in the protein structure**.

For example, mutations may disrupt:

- **ligand-binding sites**
- **protein-protein interfaces**
- **DNA-binding regions**
- the **structural core** of the protein

Residues involved in these functions are often **less tolerant to mutation**, because changes can destabilise the structure or disrupt key interactions.

Mapping mutation sensitivity onto protein structures therefore helps identify **functionally important regions**.

## Preparing the structure

Following from the [model comparison](05-model_comparison.md) chapter, we will use the human protein **SLC52A2** (UniProt ID: **Q9HAB3**).
As a reminder, this is a **membrane transporter for vitamin B2 (riboflavin)**, and mutations in this protein can cause a childhood-onset neurological disorder called Brown-Vialetto-Van Laere syndrome.

We begin by loading the predicted structure from the AlphaFold Database:

```chimerax
alphafold fetch Q9HAB3 version 6
```

{{< mol-afdb Q9HAB3 >}}

In this command we explicitly specify the **model version**.
This ensures that we retrieve the correct version of the prediction, but you should always check what the latest version available is.

We can also load the corresponding PAE matrix:

```chimerax
alphafold pae #1 palette paegreen uniprotId Q9HAB3 version 6
```

### Adding UniProt annotations

Structural information is often easier to interpret alongside **functional annotations**.
ChimeraX can retrieve UniProt annotations directly:

```chimerax
open Q9HAB3 from uniprot format uniprot
```

This opens a **sequence annotation panel** showing features such as:

- functional domains
- transmembrane regions
- experimentally observed variants
- binding sites

Clicking a feature in the panel will highlight the corresponding residues in the structure.

Combining structural and functional annotations helps identify **regions likely to be sensitive to mutation**.

## AlphaMissense mutation scores

The **AlphaMissense** model predicts the likely functional impact of every possible amino acid substitution in a protein.
For each possible mutation, the model assigns a **score between 0 and 1**:

- values near **0** suggest the mutation is likely **benign**
- values near **1** suggest the mutation is likely **deleterious**

These scores can be loaded directly in ChimeraX.

```chimerax
open Q9HAB3 from alpha_missense format amiss
```

The scores are loaded and a histogram showing the **distribution of mutation impact scores** opens on the side. 

We can add a label to each residue displaying the mutation impact score for every possible mutation:

```chimerax
mutationscores label #1 amiss height 3 palette bluered
```

This view can be useful to look at the impact of specific mutations in each residue.
However, it doesn't give a good overall view of the mutational sensitivity across the protein.

We can remove the labels with:

```chimerax
label delete
```

### Summarising scores

Because each residue can mutate to many different amino acids, it is often useful to summarise these predictions.
For example, we can compute the **average predicted mutation effect per residue**:

```chimerax
mutationscores define avg fromScore amiss setAttribute true combine mean mutationSet Q9HAB3
```

This creates a new residue attribute called `avg`.
We can then map these scores onto the structure:

```chimerax
color byattribute r:avg palette bluered key true range 0,1
cartoon byattribute r:avg
```

- **blue** residues with a **thinner** cartoon representation are more tolerant to mutation
- **red** residues with a **thicker** cartoon representation are predicted to be highly sensitive to mutation

Mapping mutation sensitivity onto the structure provides insight into which regions of the protein are **structurally or functionally constrained**.
These regions often correspond to:

- catalytic residues
- ligand-binding pockets
- protein interaction interfaces
- structurally critical elements of the fold

:::{.callout-warning}
#### Closing mutation scores

When working with multiple AlphaMissense scores, it's important to either close ChimeraX or close the mutation scores explicitly to avoid confusion between different datasets.

```chimerax
mutationscores close
```

:::

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

    ```chimerax
    close
    alphafold fetch P03372 version 6
    alphafold pae #1 palette paegreen uniprotId P03372 version 6
    ```

2. Next we load the UniProt annotation and the AlphaMissense mutation scores:

    ```chimerax
    open P03372 from uniprot format uniprot
    open P03372 from alpha_missense format amiss
    ```

3. Each residue has many possible amino acid substitutions.
   We compute the **average predicted mutation effect per residue**:

    ```chimerax
    mutationscores define avg fromScore amiss setAttribute true combine mean mutationSet P03372
    ```

4. Finally, we map these scores onto the protein structure:

    ```chimerax
    color byattribute r:avg palette bluered key true
    cartoon byattribute r:avg
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

:::{.callout-tip}
#### Key points

- The functional impact of mutations often depends on their structural location.
  - Variants occurring in conserved structural regions are more likely to affect protein stability or function.

- Computational tools such as AlphaMissense can help prioritise mutations for further study.

- AlphaMissense scores can be imported into ChimeraX using: `open <UniProt_ID> from alpha_missense format amiss`.

- Scores can be summarised using `mutationscores define` and mapped onto the structure using `color byattribute` and `cartoon byattribute`.
:::
