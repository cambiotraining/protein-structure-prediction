# Protein structure databases

:::{.callout-tip}
#### Learning Objectives

- Describe the main databases that store information about protein structure (UniProt, PDB, CATH, AlphaFoldDB, InterPro)
- Understand which database is most suitable for different tasks
- Find and retrieve data from major structure databases
- Interpret the main components of structural files (PDB and PDBx/mmCIF), including atomic coordinates, secondary structure annotations, and metadata.
- Convert between PDB and mmCIF formats
:::

## Overview

Modern structural biology generates a huge amount of data.
Researchers solve thousands of new protein structures each year. Computational tools now add millions more predicted structures.

These data only become useful if we can **store, organise, and retrieve them efficiently**. For this reason, several public databases collect information about protein sequences, structures, and domains.

## Why databases matter

A single protein structure contains a large amount of information.
For example, a typical structural file includes:

- the amino acid sequence
- the 3D coordinates of every atom
- chain organisation
- ligands and cofactors
- experimental details
- references to the scientific literature

Without databases, each research group would store these data in its own format. 
That would make sharing and analysis very difficult.

Instead, structural biology relies on **standardised repositories**. 
These resources allow researchers to:

- deposit newly solved structures
- search existing structures
- retrieve data in standard formats
- connect structures to sequence and functional annotations

Several databases now form the core infrastructure of structural bioinformatics.

## Core databases

### UniProt

**UniProt** is the central database for protein sequence information.
Each protein entry in UniProt includes:

- the amino acid sequence
- protein name and function
- organism
- domain annotations
- cross-references to other databases
- known variants and mutations

UniProt serves as the **hub** that links many biological resources together.
For example, a UniProt entry may link to:

- experimental structures in the Protein Data Bank
- predicted models in AlphaFoldDB
- domain annotations from InterPro
- structural classifications such as CATH

When working with proteins, UniProt often provides the best **starting point**.

A typical workflow begins by:

1. Searching for a protein in UniProt.
2. Inspecting its annotations.
3. Following links to structural databases.

### The Protein Data Bank (PDB)

The **Protein Data Bank (PDB)** stores experimentally determined macromolecular structures.

Researchers deposit structures into the PDB after solving them using methods such as X-ray crystallography, NMR spectroscopy or cryo-electron microscopy.
Each entry contains:

- atomic coordinates
- experimental metadata
- structural annotations
- information about ligands and cofactors

The PDB currently contains **hundreds of thousands of structures**. 
These include proteins, nucleic acids, and large molecular complexes.

Each entry receives a unique **four-character identifier**.
Examples include:

- `1A52` - estrogen receptor ligand-binding domain
- `1HCQ` - estrogen receptor DNA-binding domain bound to DNA

Researchers commonly refer to structures by these codes.

### AlphaFoldDB

Experimental structures remain limited compared with the number of known protein sequences. 
This gap motivated large-scale prediction efforts.

**AlphaFoldDB** provides predicted structures generated using AlphaFold.
The database contains models for:

- entire proteomes
- millions of proteins across many organisms

Each AlphaFoldDB entry links directly to a **UniProt identifier**.
These predicted models include useful information such as prediction confidence scores, which will be [discussed later](04-monomer.md).

Predicted structures can often guide experiments, particularly when no experimental structure exists.
However, they remain **computational models**, not experimental observations. 
Researchers must always interpret them with care.

### InterPro

Proteins often contain **domains** - conserved structural units that perform specific functions.
**InterPro** collects domain annotations from many specialised databases (e.g. PFAM and CATH), integrating them into one place. 

InterPro helps researchers identify:

- conserved domains
- functional motifs
- protein families

For example, a transcription factor might contain:

- a DNA-binding domain
- a ligand-binding domain
- regulatory regions

### CATH

**CATH** classifies proteins based on **three-dimensional structure**.
CATH groups proteins into hierarchical categories:

- **Class** - overall secondary structure composition
- **Architecture** - general spatial arrangement
- **Topology** - fold connectivity
- **Homologous superfamily** - proteins with shared ancestry

This classification helps researchers study:

- structural evolution
- recurring protein folds
- relationships between distant proteins

In other words, CATH helps answer the question: _Which proteins share similar structures, even if their sequences differ?_

## Choosing the right database

Each database answers different questions.

A useful rule of thumb:

| Task                             | Best database |
| -------------------------------- | ------------- |
| Find basic protein information   | UniProt       |
| Retrieve experimental structures | PDB           |
| Explore predicted structures     | AlphaFoldDB   |
| Identify domains and motifs      | InterPro      |
| Study structural classification  | CATH          |

In practice, researchers often move between several databases during analysis.

For example:

1. Start with **UniProt** to find the protein sequence.
2. Check **PDB** for experimental structures.
3. Use **AlphaFoldDB** if no structure exists.
4. Examine **InterPro** to identify domains.
5. Use **CATH** to look for structural relationships.

## File formats

Once we find a structure, we usually download it as a **coordinate file**.

Two formats dominate structural biology:

- **PDB format**
- **PDBx/mmCIF format**

Both store the same core information. 
They differ mainly in structure and flexibility.

### The PDB format

The original PDB format dates back to the 1970s.
It uses fixed-width text lines to describe atoms and metadata.

A typical line looks like this:

```
ATOM   1523  CA  LEU A 203      12.456  18.233   9.612
```

This contains several pieces of information:

- record type (`ATOM`)
- atom number
- atom name
- residue name
- chain identifier
- residue number
- x, y, z coordinates

These coordinates define the atom's position in three-dimensional space.

Software such as ChimeraX reads these coordinates and reconstructs the molecular structure.

### The PDBx/mmCIF format

As structures became larger and more complex, the original PDB format showed its limits.
The **PDBx/mmCIF format** replaced it as the official standard.

mmCIF files store the same information but use a more flexible table-like structure. 
They can represent:

- very large complexes
- extensive metadata
- detailed experimental information

Most modern PDB entries now appear primarily in **mmCIF format**.
Fortunately, most molecular visualisation tools can read both formats.

Sometimes there is a need to convert structures between formats, for example for compatibility with different software tools.
Many programs support conversion, including ChimeraX, which we will introduce in the [next section](03-chimerax_basics.md).

In practice, the **mmCIF format** has become the preferred standard, while PDB files remain common for legacy workflows.

### Key components of structural files

Regardless of format, structural files contain several important types of information.

- **Atomic coordinates:** These lines define the position of every atom in the structure.
  Visualisation software uses these coordinates to build the 3D model.

- **Chain and residue information:** Structures often contain multiple chains. For example: protein chains, DNA strands, ligands, cofactors.
  Chain identifiers allow software to separate these components.

- **Secondary structure annotations:** Many files include annotations that describe secondary structures such as helices and beta strands.
  These annotations help visualisation tools display the protein in cartoon form.

- **Metadata:** Structural files also store experimental information. This includes the method used to solve the structure, the resolution, authors and publication details.

### The FASTA format

FASTA is a simple text format for storing protein or nucleotide sequences.
Each entry has two parts:

- **Header line** - starts with > and usually contains an identifier and brief description.
- **Sequence lines** - the sequence itself, written in standard one-letter codes.

Example:

```
>sp|P9WF37|WHIB6_MYCTU Probable transcriptional regulator WhiB6 OS=Mycobacterium tuberculosis
MRYAFAAEATTCNAFWRNVDMTVTALYEVPLGVCTQDPDRWTTTPDDEAKTLCRACPRRW
LCARDAVESAGAEGLWAGVVIPESGRARAFALGQLRSLAERNGYPVRDHRVSAQSA
```

FASTA is widely used because it is **human-readable, compact, and compatible** with most bioinformatics tools. 
UniProt allows you to download protein sequences directly in FASTA format for further analysis.

## Exercises

:::{.callout-exercise}
#### Exploring the PDB

- Go to the <a href="https://www.rcsb.org/" target="_blank">RCSB PDB portal</a>
- Search for "**vitamin D receptor**", or directly by the PDB ID: "**1DB1**"
  - Note: use the CSM toggle when you also want to include high-accuracy AlphaFold2 models (not experimentally determined)

**Questions:**

- What expression system and method was used to determine this structure?
- Does this protein have any known domains?

:::{.callout-answer}

Looking at the [PDB entry for 1DB1](https://www.rcsb.org/structure/1DB1), we can see:

- From the **Structure Summary** tab: The structure was determined using X-ray crystallography, and the expression system used was _Escherichia coli_.
- From the **Annotations** tab: We can see the "Vitamin D nuclear receptor" domain is annotated for this protein.

We could find further details for the protein, namely specific binding sites from [UniProt entry P11473](https://www.uniprot.org/uniprotkb/P11473/entry), which is linked from the "Structure Summary" tab of the PDB entry.

:::
:::

## Summary

:::{.callout-tip}
#### Key Points

- Structural biology relies on a network of databases that store and organise protein information.

- Some of the main resources include:

  - **UniProt** - protein sequence and functional annotation [https://www.uniprot.org/](https://www.uniprot.org/)
  - **PDB** - experimentally determined structures [https://www.rcsb.org/](https://www.rcsb.org/)
  - **AlphaFoldDB** - predicted protein structures [https://alphafold.ebi.ac.uk/](https://alphafold.ebi.ac.uk/)
  - **InterPro** - domain and motif annotations [https://www.ebi.ac.uk/interpro/](https://www.ebi.ac.uk/interpro/)
  - **CATH** - structural classification of protein folds [https://cathdb.github.io/](https://cathdb.github.io/)


- Two main file formats are used to store protein structures:
  - **PDB** - the original file format definition to store three-dimensional structures, which is still used by many applications.
  - **PDBx/mmCIF** - a modern format that is now widely adopted, allowing storing larger structures and more complex metadata.

- Protein sequences are stored in FASTA format.
:::
