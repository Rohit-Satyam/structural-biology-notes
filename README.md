# structural-biology-notes
## Structure prediction 
[Robetta](https://robetta.bakerlab.org/) can model dimer/oligomer. Not suitable for peptides.

Model Archive: Modelled structure repository: https://www.modelarchive.org/

QC: Molprobity, Qmean (https://swissmodel.expasy.org/qmean/) (Use QMEANDisCo for most of the protein and QmeanBrane for membrane protein)

Basic Docking Steps
1. Check Structure Completness: Check structures for structure anomalies using [MDWeb](http://mmb.irbbarcelona.org/MDWeb/index.php). If structure has gaps, try looking into PDBRedo [server](https://pdb-redo.eu/)
2. Install the following in linux:
Read more on [prepare_ligand.py](http://autodock.scripps.edu/faqs-help/how-to/how-to-prepare-a-ligand-file-for-autodock4) and [prepare_receptor4.py](http://autodock.scripps.edu/faqs-help/how-to/how-to-prepare-a-receptor-file-for-autodock4)
```bash
conda install -c bioconda mgltools
## We can now use some mgltools python script to prepare ligands and pdb files: prepare_ligand4.py and prepare_receptor4.py
prepare_receptor4.py -r 2vf5.pdb -A 'checkhydrogens'

```
3. 

Brownie points
1. Autodock vina is available as a conda package [here](https://anaconda.org/bioconda/autodock-vina). Post installation use vina or vina_split
2. Important points from [manual of autodock](http://vina.scripps.edu/manual.html): 

> AutoDock and AutoGrid parameter files (GPF, DPF) and grid map files
> are not needed. 
> Vina inherits some of the ideas and approaches of
> AutoDock 4, such as treating docking as a stochastic global
> opimization of the scoring function, precalculating grid maps (Vina
> does that internally), and some other implementation tricks, such as
> precalculating the interaction between every atom type pair at every
> distance. The source code, the scoring funcion and the actual algorithms used are brand new, so it's more correct to think of AutoDock Vina as a new "generation" rather than "version" of AutoDock.

#### Hydrated Docking is docking in water: 

Nature Protocol: Section F Docking with explicit waters: [Source1](https://www.nature.com/articles/nprot.2016.051), [Source 2](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0172743), [Source 3](https://www.mdpi.com/1420-3049/21/11/1604/htm), [source 4](https://www.ccdc.cam.ac.uk/support-and-resources/ccdcresources/2fb0c5283e924c23a6914826c4470181.pdf), [source 5](http://autodock.scripps.edu/resources/autodock-hydrated-docking)

**How big should the search space be?**

> As small as possible, but not smaller. The smaller the search space, the easier it is for the docking algorithm to explore it. On the other hand, it will not explore ligand and flexible side chain atom positions outside the search space. You should probably avoid search spaces bigger than `30 x 30 x 30` Angstrom, unless you also increase "`--exhaustiveness`". Note:exhaustiveness  parameter determine  number  of search to find best conformation. For exhaustiveness, the default value is 8. You can keep a value of 16 or may be even 24..But it wont differ too much between these values. [Source](http://autodock.1369657.n2.nabble.com/ADL-vina-exhaustiveness-td7577507.html) & [Source2](https://www.researchgate.net/post/Which_parameters_of_Autodock_Vina_to_redocking_some_molecules)

PDBQT format is very similar to PDB format but it includes partial charges ('Q') and AutoDock 4 (AD4) atom types ('T'). There is one line for each atom in the ligand, plus special keywords indicating which atoms, if any, are to be flexible during the AutoDock experiment. Preparing the ligand involves ensuring that its atoms are assigned the correct AutoDock 4 atom types, adding Gasteiger charges if necessary, merging non-polar hydrogens, detecting aromatic carbons if any, and setting up the 'torsion tree'. For most atoms, the AD4 atom type is the same as its element; the exceptions are "OA", "NA", "SA" for hydrogen-bond acceptor O, N and S atoms; "HD" for hydrogen-bond donor H atoms; "N" for non-hydrogen bonding nitrogens; and "A" for carbons in aromatic rings.

The best input formats here would be SYBYL mol2 or AutoDock 3 PDBQ, since these store charges.

[How to download a database for docking](https://www.youtube.com/watch?v=TVf5eCO4p8Q)


**Knowledge from Paper Highlights**
1. [Evaluation of consensus scoring methods for AutoDock Vina, smina and idock](https://www.sciencedirect.com/science/article/abs/pii/S1093326319307272)

Vina was developed first, and it provided a framework for the other two. Vina searches for a docking pose using both a **gradient calculation and Monte Carlo steps**, based on a scoring function used for the searching and the ranking. 

[Smina](https://github.com/mwojcikowski/smina) , also available on [conda](https://anaconda.org/bioconda/smina) is based on a similar searching algorithm and represents the
implementation of a different set of scoring functions that can
apply both to the ligand docking process and the ranking process.

idock utilizes fewer Monte Carlo steps in its search than the
other two, and instead undertakes more parallel searches. It does,
however, use the **same scoring function as Vina.** **Because the
scoring determines the path of the search, Vina and smina generate
different poses, not just different final docking scores. While idock
and Vina would score the same pose the equivalently, they search
the conformational space in a different manner.**

Given the choice between running these 3 programs in some combination or individually, our results suggest that the most efficient approach is to only use **smina** with default settings, as opposed to running multiple screens and using a consensus scoring scheme which is unlikely to offer any
improvement.

Quick and dorty start with smina using [this](https://www.cheminformania.com/ligand-docking-with-smina/) blogpost.
[A workflow for docking/virtual screening](https://www.macinchem.org/reviews/docking/docking.php)
For consensus docking use these tools: AutoDock, ICM, LeDock, rDock, AutoDock Vina and Smina. These programs are characterized by having different search algorithms and scoring functions, as described [here](https://www.nature.com/articles/s41598-019-41594-3#Abs1)

2. [Machine Learning Consensus Scoring Improves Performance Across Targets in Structure-Based Virtual Screening](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5872818/)
3. [Structure-Based Virtual Screening: From Classical to Artificial Intelligence](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7200080/#:~:text=4-Structure-Based%20Virtual%20Screening,target%20to%20form%20a%20complex.)
4. [1001 Ways to run AutoDock Vina for virtual screening](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4801993/)
