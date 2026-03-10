# Additional exercises

## whiB6 transcriptional regulator

In this set of exercises, you will work with the **whiB6 protein**, which is a transcriptional regulator in _Mycobacterium tuberculosis_:

- Part of the WhiB-like family of proteins, which are small redox-sensing proteins important for _M. tuberculosis_ survival and pathogenesis.
- Interacts with RNA polymerase and sigma factors to regulate gene expression.
- Predicted to bind an iron–sulfur ([4Fe–4S]) cluster.

In this exercise, you will:

- Generate a structural model of whiB6 with AlphaFold Server (and optionally ColabFold).
- Assess model quality.
- Compare models generated with different parameters and AlphaFold versions.

:::{.callout-exercise}
#### Sequence retrieval

1. Find the UniProt entry for **whiB6 from _Mycobacterium tuberculosis_** protein.

**Questions:**

- What is the length of the protein?
- Does the annotation mention any domains or cofactors?
- Can you identify any functional sites or motifs?

:::{.callout-answer}

We can find this protein in UniProt with the accession number P9WF37.

- In the **Sequence** section we can see the protein is 116 amino acids long. 
  We could use the **Download** button to obtain the sequence in FASTA format:

    ```
    >sp|P9WF37|WHIB6_MYCTU Probable transcriptional regulator WhiB6 OS=Mycobacterium tuberculosis (strain ATCC 25618 / H37Rv) OX=83332 GN=whiB6 PE=1 SV=1
    MRYAFAAEATTCNAFWRNVDMTVTALYEVPLGVCTQDPDRWTTTPDDEAKTLCRACPRRW
    LCARDAVESAGAEGLWAGVVIPESGRARAFALGQLRSLAERNGYPVRDHRVSAQSA
    ```

- In the **Family & Domains** section we can see there is an annotated domain called 	"4Fe-4S Wbl-type" in positions 33-86.
- In the **Function** section we can see that this protein is a transcriptional regulator that binds an iron–sulfur ([4Fe–4S]) cluster, with binding sites at positions 12, 53, 56, 62.

:::
:::

:::{.callout-exercise}
#### Structure prediction with AlphaFold Server (AF3)

1. Submit the whiB6 sequence to the <a href="https://alphafoldserver.com" target="_blank">AlphaFold Server (AlphaFold3)</a> to predict its structure.

Questions:

- What is the overall confidence of the model?
- Are there low-confidence regions? Where are they located?
- Do these regions correspond to any of the domains or functional sites you identified in the previous exercise?

:::{.callout-answer}

After running the prediction we can see that: 

- The predicted template modeling (pTM) score was 0.63. As this is above 0.5, we may conclude the overall confidence of the model is good.
- The pLDDT score is low (< 70) at the N-terminus (around residues 1-30) and the C-terminus (around residues 109-116), while the core of the protein has a much higher pLDDT score (> 70).
- As the annotated domain is between residues 33-86, we can see that this region corresponds to the structured core of the protein, which has a high confidence score.

:::
:::

:::{.callout-exercise}
#### Structure prediction with ColabFold (AF2)

ColabFold is a user-friendly implementation of AlphaFold2 that runs on Google Colab. 
You can access it at <a href="https://colab.research.google.com/github/sokrypton/ColabFold/blob/main/AlphaFold2.ipynb" target="_blank">this Colab notebook</a>.

As running predictions with the free ColabFold can take some time, for this exercise, we will explore pre-processed results from previous runs:

- [Run 1](https://colab.research.google.com/drive/11ED7YCFc6uA_lwFUtqdEIFCjNGe8cMT3?usp=sharing) - used default settings
- [Run 2](https://colab.research.google.com/drive/1L5Pruy_Xx819s-1bdeEmnVxGqYZs7g5x?usp=sharing) - changed `num_relax`, `template_mode` and `num_seeds`

**Questions:**

1. Identify the parameter values that changed between the two runs. 
2. Based on the quality scores for both predictions (pLDDT, PAE and pTM), which do you think gives best results?
3. Looking at the results for run 2, what can you conclude about the predictions from the five AlphaFold2 models?
4. Optional: open the top-ranked model from run 1 in ChimeraX and explore the confidence scores and PAE matrix.

:::{.callout-answer}

1. Run 1 used default settings, while run 2 used the following: 
  - `num_relax` = 1 (relax only the top-ranked model)
  - `template_mode` = pdb100 (uses the PDB100 database to find structural templates based on sequence similarity)
  - `num_seeds` = 4 (to generate a few models and choose the one with highest quality)

2. The top-ranking predictions of each run had:
  - Run 1 (`rank_001_alphafold2_ptm_model_4_seed_000`): pLDDT=75.8; pTM=0.627
  - Run 2 (`rank_001_alphafold2_ptm_model_4_seed_000`): pLDDT=78.5 pTM=0.661

These are both very similar, but run 2 has slightly higher confidence scores, suggesting it may be the better prediction.

3. For run 2 the top-ranked models all come from model 4 (with different seeds), suggesting this is the best-performing model for this protein.

4. We can open the model in ChimeraX with the following commands:

    ```bash
    close
    cd ~/Course_Materials/whiB6_mtb/whiB6_monomer_run1_af2/
    open whiB6_monomer_run1_af2_f46cc_unrelaxed_rank_001_alphafold2_ptm_model_4_seed_000.pdb
    colour byattribute bfactor palette alphafold
    alphafold pae #1 palette paegreen file whiB6_monomer_run1_af2_f46cc_scores_rank_001_alphafold2_ptm_model_4_seed_000.json
    ```

- We initiate a fresh session using `close`.
- We navigate to the folder where the results are stored using `cd`.
- We open the top-ranked model (`.pdb` format) using `open`.
- We colour the structure by pLDDT score using `colour byattribute bfactor palette alphafold`.
- We open the PAE matrix using `alphafold pae` and colour it with a green palette.
:::
:::

:::{.callout-exercise}
#### Comparing two model predictions

In the [monomer prediction exercises](04-monomer.md), we predicted the structure of the _Mycobacterium tuberculosis_ transcriptional regulator whiB6 using AlphaFold2 and AlphaFold3.
We will now compare the best-ranked models from each of them, by aligning them to each other in ChimeraX.

Run the following code in ChimeraX, which will open and align these models:

```bash
close
cd ~/Course_Materials/whiB6_mtb/
open whiB6_monomer_run1_af2/whiB6_monomer_run1_af2_f46cc_unrelaxed_rank_001_alphafold2_ptm_model_4_seed_000.pdb
open whiB6_monomer_af3/fold_whib6_monomer_af3_model_0.cif
mm #2 to #1
```

**Questions:**

1. Do their overall folds look similar?
2. Which model prediction would you choose to work with and why?
3. Bonus: To make the visual assessment of the alignment easier, can you display the cartoon only for the high-confidence (pLDDT) region of the protein predictions?

:::{.callout-answer}

1. Comparing these two predictions, we can conclude that: 

   - The core of the protein is very similar across the two models, with a similar fold and secondary structure elements.
   - The N-terminus and C-terminus are variable across the two models:
     - The AF3 model predicted a secondary structure element (α-helix) in the N-terminus, which the AF2 model did not.
     - Conversely, the AF2 model predicted a secondary structure element (α-helix) in the C-terminus, which the AF3 model did not.

2. The differences between the two models are mainly in regions with low pLDDT and PAE scores, while the core of the protein is very consistent between the two. 
   The pTM scores for the two models are also identical (0.63), suggesting similar overall confidence in the predicted fold.
   Therefore, in this case, it probably does not matter which model we choose to work with.

3. We can confirm that the regions with high pLDDT score are very consistent across the two models:

    ```bash
    hide cartoon
    cartoon :31-108
    ```

   - We hide the full cartoon
   - We then show the cartoon only for the high-confidence region (residues 31-108), which also includes the annotated domain.

:::
:::

:::{.callout-exercise}
#### Comparing multiple models

Continuing from the previous exercises on the whiB6 protein, we can also compare more than 2 model predictions.

In the second run of the AlphaFold2 predictions, we generated multiple models using different random seeds, which can be used to explore the consistency of the predictions across different runs.

Here is the code to open multiple sequences and align them to each other with MatchMaker:

```bash
close
cd ~/Course_Materials/whiB6_mtb/whiB6_monomer_run2_af2/
open *unrelaxed*model_4*.pdb
mm #2-4 to #1
```

- We `close` the current session to start fresh.
- We `cd` into the directory where the models are located.
- We `open` all the models that correspond to model 4 (with different seeds) using the `*` wildcard.
- We use `mm` (MatchMaker) to align several models `#2-4` to model 1 (`to #1`), which we use as a reference.

Questions: 

1. What can you conclude about the prediction consistency across the different seeds?
2. Following a similar code as above, start a new session to compare the outputs of the five AlphaFold2 models (1-5) for seed 000.

:::{.callout-answer}

1. Running the code given, which compares the four random seed predictions for `model_4`, we can see that: 

   - The main source of variation is in the N-terminus, which might be disordered and thus does not align well across the different seeds. 
   - The C-terminus, although having low pLDDT scores, seems to be more consistent across the different seeds. 

2. We can adapt the code to compare the top-ranked models from each of the five AlphaFold2 models (with seed 000):

    ```bash
    close
    cd ~/Course_Materials/whiB6_mtb/whiB6_monomer_run2_af2/
    open *unrelaxed*seed_000.pdb
    mm #2-5 to #1
    ```

  - From this comparison, we can see that:
     - The core of the protein is very consistent across models
     - The N-terminus is very variable across the different models, with some predicting a secondary structure element in this regions, which the best-ranked model (number 4) did not.
     - The C-terminus is more consistent across the different models, although it has low pLDDT scores. This could suggest that the α-helix predicted for this region may be a real feature. However, from our previous comparison with the AlphaFold3 model, we know this is not the case.

:::
:::


:::{.callout-exercise}
#### whiB6 bound to sigAr4-RNAP

On PDB entry [8D5V](https://www.rcsb.org/structure/8D5V), **whiB6** is available as a complex with **sigAr4-RNAP**: a fusion protein of SigmaA region 4 (sigAr4) and the tip of RNA polymerase (RNAP). 
This complex is used to study whiB6's role in transcription.

These proteins were used in the CASP15 competition to test multimer prediction algorithms, and we will try to recreate the predictions for this complex using ColabFold and AlphaFold3.

- Use <a href="https://alphafoldserver.com" target="_blank">AlphaFold Server (AlphaFold3)</a> to create a prediction for this heterodimer. Assess its quality based on global (pTM, ipTM) and local (plDDT, PAE) scores.
  - You can obtain the sequences from the [PDB entry here](https://www.rcsb.org/fasta/entry/8D5V/display).
- Compare the result with <a href="https://colab.research.google.com/drive/12U_p6XMyQg4_-zFv382jr437LE6-tRJq?usp=sharing" target="_blank">this preprocessed ColabFold prediction</a> in terms of global scores, but also local regions where each model may have lower confidence. 
- Load the experimental structure (8D5V) and the two models into ChimeraX. Compare the predicted interfaces with the experimental one, and note any differences.

:::{.callout-answer}

- For the AlphaFold3 model:
  - pTM = 0.76 indicating a high confidence in the overall fold of the complex.
  - ipTM = 0.78 indicating a high confidence in the interaction between the two chains. 

- For the ColabFold model: pTM = 0.766 and ipTM=0.792, which is very similar to the AlphaFold3 confidence scores.

- We load the structures into ChimeraX with the following code: 

```bash
close
cd ~/Course_Materials/whiB6_mtb/
open 8D5V
hide atoms
delete #1/C-D
open whiB6_heterodimer_af2/whiB6_heterodimer_af2_9518d_relaxed_rank_001_alphafold2_multimer_v3_model_1_seed_000.pdb
open whiB6_heterodimer_af3/fold_whib6_heterodimer_af3_model_0.cif
mm #2-3 to #1
select :31-108
```

:::
:::


## Alcohol dehydrogenase

:::{.callout-exercise}
#### Novel structure comparison

You are researching the salt-loving archaeon _Halorubrum lacusprofundi_, which survives in highly saline Antarctic lakes. 
To better understand its metabolism, you decide to compare the structure of a alcohol dehydrogenase from _H. lacusprofundi_ (UniProt ID: **B9LTJ0**) to a homologous structure from a non-halophilic organism.

Alcohol dehydrogenases are widespread metabolic enzymes used to break down alcohols that are toxic to the cell. 
They use NAD+ as a cofactor, which gets reduced to NADH during the reaction.

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

## gp2 bacteriophage

:::{.callout-exercise}
#### gp2 bacteriophage homodimer

One of the competition proteins in CASP15 was a recently resolved bacteriophage protein (gp2/ChmB). 
It forms part of a “phage nucleus” that protects its DNA from the host’s DNA-targeting immune response. 

Enustun et al. 2023 describe the formation of a homodimer by this protein.

Use AlphaFold3 to create a prediction for this homodimer. Assess its quality based on global (pTM, ipTM) and local (plDDT, PAE) scores.
Visually compare your prediction with the experimental structure on PDB 7UYX. Pay particular attention to the β sheets where confidence is lower - does the prediction match the experimental structure?

```bash
close
cd ~/Course_Materials/gp2_bacteriophage/
open 7UYX
select #1/A-B
delete ~sel
select clear
open gp2_homodimer_af3/fold_gp2_homodimer_af3_model_0.cif
mm #2 to #1
colour byattribute bfactor #2 palette alphafold key true
alphafold pae #2 palette paegreen file gp2_homodimer_af3/fold_gp2_homodimer_af3_full_data_0.json
```

AlphaFold3 predicted a β-sheet in each of the chains, which is absent from the experimental structure.
This is likely because this is a difficult region to resolve experimentally, which may be related to why the _de novo_ prediction models struggle to predict it accurately.
:::

## p53

:::{.callout-exercise}
#### p53 tetramer bound to DNA

p53 is known to form a homotetramer that binds to a DNA element on the CDKN1A promoter (Emamzadah, Tropia & Halazonetis 2011). The experimentally-resolved structure is available on PDB: 3TS8.
We will aim to predict this complex in-silico using AlphaFold3. 
From the sequences FASTA file, add both entities on AlphaFold3 Server: 
>p53_HUMAN residues 90-300: to ease interpretation, we use only the “core” region of p53 (residues 90-300), chosen based on its low PAE from our monomer prediction. 
>CDKN1A p53-response element: DNA element on CDKN1A promoter that p53 binds to.
Set the p53 entity as 4 copies.
Click the ⋮ menu in the DNA entity to create a reverse complement entity.

```bash
close
cd ~/Course_Materials/p53_human/
open p53_homotetramer_with_dna_af3/fold_p53_homotetramer_with_dna_af3_model_0.cif
alphafold pae #1 palette paegreen file p53_homotetramer_with_dna_af3/fold_p53_homotetramer_with_dna_af3_full_data_0.json
hide all
cartoon protein
colour protein ivory
colour nucleic steelblue
alphafold contacts /A-D to /E-F distance 5 palette paegreen
open P04637 from uniprot format uniprot
# region of interest > interaction with DNA
select /A-D:184-191
colour sel red
```

Several of the contacts within 5 Å of the DNA are consistent with the UniProt annotation.

Nice visualisation:

```bash
hide cartoon
show /A-D:184-191 atoms
colour /A-D:184-191 red
show /E-F atoms
style atoms ball
colour /E steelblue
colour /F lightblue
surface protein transparency 80
```

:::

:::{.callout-exercise}
#### p53-MDM2 complex

In the course folder you will find the ColabFold outputs for the p53-MDM2 prediction.
Load the PDB and JSON files for the top-ranked model into ChimeraX.
Zoom in on the region where you think these two proteins are likely to interact (based on the PAE score as well as the diagram in the previous slide).
Compare the structure you obtained with the experimental structure available from PDB: 1YCR
Note: the PDB structure only includes residues MDM2:17-125 and p53:15-29

:::
