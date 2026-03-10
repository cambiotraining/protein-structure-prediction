# Protein-ligand complexes

## Overview

In this section we will learn to analyse the protein-ligand interactions using ChimeraX. 
As an example, we will continue working with the Estrogen Receptor (PDB: 1ERE), which we have already visualised in the previous section.

There are numerous drugs that are designed to target the Estrogen Receptor as treatment for breast cancer and other reproductive diseases, and all of them interact with the receptor in its ligand binding pocket. 
In this section, we will look at the protein-ligand interactions of the Estrogen Receptor. 

The same tools can be used to analyse predicted interactions between AlphaFold models and ligands.

We start our session with the following commands: 

```bash
close
open 1ERE
select #1/A
delete ~sel
select clear
style protein ball
hide atoms
show cartoon
show :EST atoms
style :EST sphere
colour :EST & C grey
colour :EST & O magenta
cofr :EST
```

Which give us a view of the secondary structure of the Estrogen Receptor, with the EST ligand shown as atoms.
For convenience, we also set the centre of rotation (`cofr`) on the ligand. 

## Hydrogen bonds

To identify the hydrogen bonds between the ligand and the protein, we can use the `hbonds` command.

```bash
hbonds :EST reveal true log true
```

- `reveal true` will show the hydrogen bonds as dashed lines in the structure (cyan by default, but you can change this with `colour hbonds <colour>`).
- `log true` will display the details of the interactions in the log window.

## Other interactions

To reveal other interactions such as Van der Waals, we can use the `contacts` command.

```bash
contacts :EST reveal true log true
```

- `reveal true` will show the contacts as dashed lines in the structure (green by default, but you can change this with `contacts :EST color <colour>`).
- `log true` will display the details of the interactions in the log window.

## Surface representation

To better visualise the binding pocket and the ligand, we can show:

- The ligand in stick style
- A surface representation of the molecular surface of the atoms

```bash
style :EST stick
show :EST surfaces
surface :EST color white transparency 65
```

## Select residues by distance

Let’s hide atoms outside the ligand binding pocket area to see the pocket area clearly. First, deselect the ligand, select the protein, then hide the model. 
Then we only show the residues that are within 5 Ä of the ligand. 

```bash
hide protein cartoon
hide protein atoms
show :EST :<5
```

## Exercises

:::{.callout-exercise}
#### Select atoms involved in interactions

In this exercise we will attempt to colour the protein atoms involved in the interactions between the ligand and the protein.

![Zoomed-in view of the oestrogen molecule bound to the oestrogen receptor. Atoms in orange are involved in hydrogen bonds (cyan lines); atoms in yellow are involved in other contacts (green lines).](images/chimerax_1ere_bonds_exercise.png)

Start your session using the following code, which will show the ligand and the residues within 5 Å of the ligand, as well as the hydrogen bonds and contacts between the ligand and the protein.

```bash
close
open 1ERE
delete #1/B-F
style protein ball
style :EST stick
hide protein
hide solvent
colour :EST & C grey
colour :EST & O magenta
show :EST :<5
cofr :EST
hbonds :EST reveal true log true
contacts :EST reveal true log true
```

Using a combination of `select` and new keywords listed below, try to: 

- Colour the protein atoms involved in hydrogen bonds in `darkorange`
- Colour the protein atoms involved in contacts in `gold`

Here are some useful keywords to select atoms involved in interactions: 

- `hbonds` selects the pseudobonds (dashed lines) representing the hydrogen bonds
- `pbonds` selects all pseudobonds (including both hydrogen bonds and contacts)
- `hbondatoms` selects the atoms as well as pseudobonds involved in the hydrogen bonds 
- `pbondatoms` selects the atoms as well as pseudobonds involved in the contacts

:::{.callout-hint}
Work on your selection incrementally, making use of the `select subtract` command to deselect certain items from your initial selection.
:::

:::{.callout-answer}

We can achieve this in several incremental steps:

```bash
select pbondatoms
select subtract pbonds
select subtract :EST
colour sel gold
```

- `select pbondatoms` selects all atoms and pseudobonds involved in both hydrogen bonds and contacts 
- `select subtract pbonds` deselects the pseudobonds, leaving only the atoms selected 
- `select subtract :EST` deselects the ligand atoms, leaving only the protein atoms selected 
- `colour sel gold` colours the selected atoms gold 

Then we can repeat the same steps for hydrogen bonds: 

```bash
select hbondatoms
select subtract hbonds
select subtract :EST
colour sel darkorange
select clear
```

Finally, we can reset our selection with `select clear`. 

:::
:::

:::{.callout-exercise}
#### Edit the bond representation

You may notice that some contacts overlap with the hydrogen bonds. 
Reading the [documentation for the `size` command](https://www.cgl.ucsf.edu/chimerax/docs/user/commands/size.html), can you find a way to make the hydrogen pseudobonds thicker, so they stand out?

:::{.callout-answer}

We can use the `pseudobondRadius` attribute to change the thickness of the pseudobonds, making sure to apply this only to the hydrogen bonds, using the `hbonds` selector keyword:

```bash
size hbonds pseudobondRadius 0.1
```

:::
:::
