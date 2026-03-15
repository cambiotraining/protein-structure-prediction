# Alcohol dehydrogenase {.unnumbered}

You are researching the salt-loving archaeon _Halorubrum lacusprofundi_, which survives in highly saline Antarctic lakes. 
To better understand its metabolism, you decide to compare the structure of a alcohol dehydrogenase from _H. lacusprofundi_ (UniProt ID: **B9LTJ0**) to a homologous structure from a non-halophilic organism.

Alcohol dehydrogenases are widespread metabolic enzymes used to break down alcohols that are toxic to the cell. 
They use NAD+ as a cofactor, which gets reduced to NADH during the reaction.

:::{.callout-exercise}
#### Novel structure comparison

Your tasks are to: 

1. Import and assess the structure prediction quality for the _H. lacusprofundi_ alcohol dehydrogenase in **ChimeraX**.
   - We provide pre-processed predictions for AlphaFold2 in `<TODO: folder>`. Optionally, you can run a new prediction using AlphaFold Server.

2. Search for homologous structures in the PDB using **FoldSeek**, and choose a solved structure complexed with cofactor NADH to compare it to.

3. Open the predicted structure and the chosen PDB structure in ChimeraX, and align the two structures to each other. 

4. Examine if there is good alignment in the region containing the NADH binding site of the homolog, considering both the RMSD alignment score and the pLDDT prediction score. 

:::{.callout-answer}

1. We import the predicted structure into ChimeraX:

    ```bash
    close
    cd ~/Course_Materials/TODO
    open B9LTJ0_af3/fold_B9LTJ0_af3_model_0.cif
    alphafold pae #1 palette paegreen file B9LTJ0_af3/fold_B9LTJ0_af3_full_data_0.json
    ```

  - TODO: remarks about the structure quality

2. We submitted the PDB file to FoldSeek for homologous structures and sorted the results by E-value.
   The top hit was the structure of an alcohol dehydrogenase from _E. coli_: "**4ilk-assembly1_A**".
   This corresponds to chain A of the PDB structure 4ilk, which is a structure of an alcohol dehydrogenase complexed with NADH.

3. We can open the chosen PDB structure in ChimeraX, and align the two structures to each other.

    ```bash
    open 4ilk
    delete #2/B
    hide :ZI :MN
    hide #2 atoms
    mm #1 to #2 showAlignment true
    ```

With the alignment done, we generate several views of the protein to assess the region around the NADH binding site.

First, we consider the RMSD of the aligned residues, to see if the alignment quality is high in the region of interest:

```bash
hide #2 cartoon
colour #1 white
colour byattribute r:seq_rmsd #1 target csab palette RdYlBu key true
surface :NAI color cyan transparency 50
```

We can see that the region around the NADH binding site has a low RMSD.
This is a sign that the predicted structure around the NADH binding pocket is consistent with the solved _E. coli_ structure.

In another view, we colour by the AlphaFold confidence score (B-factor attribute) to see if the model is confident in the region of interest:

```bash
colour byattribute bfactor #1 palette alphafold key true
```

We can see that, in general, the model is confident in the region around the NADH binding site, with pLDDT scores above 0.7.

Finally, we create a view with the protein surface shown to see if the NADH molecule fits in a pocket within the predicted structure: 

```bash
surface hide
surface #1 color white transparency 80
show :NAI atoms
style sphere
```

Visually examining the structure, we can see that the NADH molecule fits within a pocket in the predicted structure, which again is a sign that the pocket is conserved in the predicted structure.
We would expect this to be the case, as alcohol dehydrogenases are a well-studied family of enzymes, and the NADH binding pocket is likely to be conserved across species.

Downstream of this visual analysis, we could formally predict binding pockets in our structure using a tool such as `fpocket`, and then dock NADH to the predicted pocket to see if it can bind in a similar pose to the experimentally solved structure in _E. coli_.

:::
:::
