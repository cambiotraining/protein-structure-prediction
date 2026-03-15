# Bacteriophage PA1c gp2

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
