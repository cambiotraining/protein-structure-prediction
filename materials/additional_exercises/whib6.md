# Transcriptional regulator WhiB6

In this set of exercises, you will explore the structure and function of **WhiB6**, a transcriptional regulator from the bacterium _Mycobacterium tuberculosis_, the causative agent of tuberculosis.

WhiB6 belongs to the **WhiB-like (Wbl) family of proteins**, a group of small iron–sulfur proteins that act as **redox-sensitive transcriptional regulators**. These proteins play important roles in bacterial stress responses and pathogenesis. Members of the Wbl family typically coordinate an **iron–sulfur ([4Fe–4S]) cluster** using conserved cysteine residues, which allows them to sense changes in cellular redox conditions.

In _M. tuberculosis_, WhiB6 regulates gene expression by interacting with **RNA polymerase and sigma factors**, helping the bacterium adapt to environmental stresses encountered during infection.

In this exercise series, you will:

- Retrieve sequence information from protein databases.
- Predict the structure of WhiB6 using AlphaFold.
- Assess model confidence using AlphaFold quality metrics.
- Compare models generated with different methods and parameters.
- Analyse a predicted protein complex and compare it with an experimental structure.

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
#### Structure prediction with AlphaFold Server (AlphaFold3)

We will now predict the three-dimensional structure of WhiB6 using AlphaFold.

- Submit the whiB6 sequence to the <a href="https://alphafoldserver.com" target="_blank">AlphaFold Server (AlphaFold3)</a> to predict its structure.

Questions:

1. What is the overall confidence of the model?
2. Are there low-confidence regions? Where are they located?
3. Do these regions correspond to any of the domains or functional sites you identified in the previous exercise?

:::{.callout-answer}

After running the prediction we can see that: 

1. The predicted template modeling (pTM) score was 0.63. As this is above 0.5, we may conclude the overall confidence of the model is good.

2. The pLDDT score is low (< 70) at the N-terminus (around residues 1-30) and the C-terminus (around residues 109-116), while the core of the protein has a much higher pLDDT score (> 70).

3. As the annotated domain is between residues 33-86, we can see that this region corresponds to the structured core of the protein, which has a high confidence score.

:::
:::

:::{.callout-exercise}
#### Structure prediction with ColabFold (AlphaFold2)

AlphaFold3 predictions are convenient to run using the AlphaFold Server. 
However, many researchers still use **AlphaFold2 implementations such as ColabFold**, which allow more control over prediction parameters.

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

    ```chimerax
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

In the previous exercises we generated predictions for WhiB6 using both AlphaFold2 and AlphaFold3. 
We will now compare the best-ranked models from each method by aligning them in ChimeraX.

Run the following code in ChimeraX, which will open and align these models:

```chimerax
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
     - The AlphaFold3 model predicted a secondary structure element (α-helix) in the N-terminus, which the AlphaFold2 model did not.
     - Conversely, the AlphaFold2 model predicted a secondary structure element (α-helix) in the C-terminus, which the AlphaFold3 model did not.

2. The differences between the two models are mainly in regions with low pLDDT and PAE scores, while the core of the protein is very consistent between the two. 
   The pTM scores for the two models are also identical (0.63), suggesting similar overall confidence in the predicted fold.
   Therefore, in this case, it probably does not matter which model we choose to work with.

3. We can confirm that the regions with high pLDDT score are very consistent across the two models:

    ```chimerax
    hide cartoon
    cartoon :31-108
    ```

   - We hide the full cartoon
   - We then show the cartoon only for the high-confidence region (residues 31-108), which also includes the annotated domain.

:::
:::

:::{.callout-exercise}
#### Comparing multiple models

AlphaFold2 generates multiple predictions for each sequence using different neural network models and random seeds. 
Comparing these predictions can help assess the **robustness of the predicted structure**.

In this exercise we will compare several predictions generated with different random seeds.

In the second run of the AlphaFold2 predictions, we generated multiple models using different random seeds, which can be used to explore the consistency of the predictions across different runs.

Here is the code to open multiple sequences and align them to each other with MatchMaker:

```chimerax
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

    ```chimerax
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

So far we have analysed WhiB6 as an isolated protein. 
However, transcriptional regulators typically function as part of **larger molecular complexes**.

In this exercise we will analyse WhiB6 in complex with RNA polymerase and a sigma factor using both experimental and predicted structures.

On PDB entry [8D5V](https://www.rcsb.org/structure/8D5V), **whiB6** is available as a complex with **sigAr4-RNAP**: a fusion protein of SigmaA region 4 (sigAr4) and the tip of RNA polymerase (RNAP). 
This complex is used to study whiB6's role in transcription.

These proteins were used in the CASP15 competition to test multimer prediction algorithms, and we will try to recreate the predictions for this complex using ColabFold and AlphaFold3.

- Use <a href="https://alphafoldserver.com" target="_blank">AlphaFold Server (AlphaFold3)</a> to create a prediction for this heterodimer. Assess its quality based on global (pTM, ipTM) and local (pLDDT, PAE) scores.
  - You can obtain the sequences from the [PDB entry here](https://www.rcsb.org/fasta/entry/8D5V/display).
- Compare the result with <a href="https://colab.research.google.com/drive/12U_p6XMyQg4_-zFv382jr437LE6-tRJq?usp=sharing" target="_blank">this preprocessed ColabFold prediction</a> in terms of global scores, but also local regions where each model may have lower confidence. 
- Load the experimental structure (8D5V) and the two models into ChimeraX. Compare the predicted interfaces with the experimental one, and note any differences.

:::{.callout-answer}

- For the AlphaFold3 model:
  - pTM = 0.76 indicating a high confidence in the overall fold of the complex.
  - ipTM = 0.78 indicating a high confidence in the interaction between the two chains. 

- For the ColabFold model: pTM = 0.766 and ipTM=0.792, which is very similar to the AlphaFold3 confidence scores.

- We load the structures into ChimeraX with the following code: 

```chimerax
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
