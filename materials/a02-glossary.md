# Glossary

## Structural biology terms

**Amino acid** - The basic building block of proteins; 20 standard amino acids form peptide chains through covalent peptide bonds.

**Polypeptide / Chain** - A single continuous sequence of amino acids connected by peptide bonds. In PDB files, each chain is labelled with a unique identifier (e.g. chain A, chain B).

**Protein** - A functional biological molecule composed of one or more polypeptide chains, folded into a specific three-dimensional structure.

**Residue** - A single amino acid within a polypeptide chain (after incorporation into the chain).

**Fold** - The overall 3D arrangement of secondary structure elements (helices, sheets, loops) that defines a protein’s topology and shape.

**Domain** - A compact, independently folding unit within a protein, often associated with a specific function. A protein may have one or multiple domains.

**Subunit** - A single polypeptide chain within a larger multi-chain assembly. For example, each chain in a dimer or tetramer is a subunit.

**Complex** - An assembly of two or more protein subunits or different biomolecules (e.g. DNA, RNA, ligands) that interact to perform a function.

**Oligomer** - A complex of multiple subunits (can be identical or different).

**Homo-oligomer** - A complex composed of **identical** subunits (e.g. a homodimer or homotetramer).

**Hetero-oligomer** - A complex composed of **different** subunits (e.g. a heterodimer).

**Quaternary structure** - The arrangement and interactions of multiple subunits within a protein complex.

**Ligand** - A small molecule, ion, or cofactor that binds to a protein and may regulate or stabilise its function.

**Cofactor / Prosthetic group** - A non-protein chemical compound (e.g. metal ion, heme, or [Fe-S] cluster) essential for the protein’s biological activity.

**Binding site** - A region on a protein surface where a ligand, cofactor, or another protein binds specifically.

**Conformation** - The spatial arrangement of atoms in a protein at a given time; proteins may adopt multiple conformations during function.

**Interface** - The region where two protein subunits (or proteins) make contact within a complex.

**Model** - A computational or experimentally determined representation of a protein's 3D structure.


## Databases

**PDB (Protein Data Bank)** - A public database of experimentally determined 3D structures of proteins and nucleic acids.

**UniProt** - A comprehensive database of protein sequence and functional information.


## Computational prediction terms

**Predicted model** - A computationally generated structure (e.g. from AlphaFold or Rosetta), not determined experimentally.

**Template** - A known structure used as a reference or starting point for modelling a related protein.

**RMSD (Root Mean Square Deviation)** - A measure of the average distance between atoms (usually backbone atoms) of two superimposed protein structures, used to assess structural similarity.

**pLDDT (predicted Local Distance Difference Test)** - A confidence score provided by AlphaFold for each residue in a predicted protein structure, indicating the reliability of the prediction.

**pTM (predicted Template Modelling score)** - A confidence score provided by AlphaFold-Multimer for predicted protein complexes, indicating the reliability of the overall quaternary structure prediction.

**ipTM (interface predicted Template Modelling score)** - A confidence score provided by AlphaFold-Multimer that specifically assesses the quality of the predicted interfaces between subunits in a protein complex.

**PAE (Predicted Aligned Error)** - A matrix provided by AlphaFold that estimates the expected positional error between pairs of residues in a predicted structure, useful for assessing confidence in relative domain positions.
