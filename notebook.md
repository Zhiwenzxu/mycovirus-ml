# Data

In UW Box from Helena Jaramillo Mesa.

```shell
(master) $ pwd
/Users/Clauberry/Dropbox/Documents/solislemus-lab/student-projects/mycovirus-ml/data
(master) $ ls
Sclerotinia_biocontrol_mycovirus_nucleotide.fasta
Sclerotinia_biocontrol_mycovirus_protein.fasta
mycovirus_genbank_all_refseq_nucleotide_unique.fasta
mycovirus_genbank_all_refseq_protein_unique_proteindb.fasta
mycovirus_list_viralzone.md
```

Number of taxa:
```shell
(master) $ grep ">" Sclerotinia_biocontrol_mycovirus_protein.fasta | wc -l
      12
(master) $ grep ">" Sclerotinia_biocontrol_mycovirus_nucleotide.fasta | wc -l
       7
(master) $ grep ">" mycovirus_genbank_all_refseq_nucleotide_unique.fasta | wc -l 
     350
(master) $ grep ">" mycovirus_genbank_all_refseq_protein_unique_proteindb.fasta | wc -l
     465
```

- My email to Helena (11/25):

I have several questions. 
If I understand correctly, in "Sclerotinia_biocontrol_mycovirus_nucleotide.fasta" (and "Sclerotinia_biocontrol_mycovirus_protein.fasta") you have the whole genome alignments (or protein alignments) for 7 (or 12) taxa that have biocontrol potential. 
While in "mycovirus_genbank_all_refseq_nucleotide_unique.fasta" ("mycovirus_genbank_all_refseq_protein_unique_proteindb.fasta") has 350 (or 465) taxa with whole genome alignments (or protein alignments) for viruses that we do not know if they are used as biocontrol or not.

Ideally, we need one fasta file with all (biocontrol, non-biocontrol), but I agree with what you say that the alignment will be meaningless as these viruses are very different.
Would it be the case also for the 110 viruses from the curated database (that they are difficult to be aligned)?
Because we could just use the closest to the sclerotinia viruses to have a semi-meaningful alignment. I would prefer not to just use one conserved gene because the algorithm will find for patterns in whatever alignment we give. If we only give this one gene, we are basically saying that the signal for biocontrol is in this gene only. 

So, the decision is to choose the right viruses so that we can have one fasta file with biocontrol and non-biocontrol viruses with whole genomes alignments (or protein alignments) so that alignment is not totally garbage, but also we have options outside sclerotinia. I know that this is difficult. If you think this is impossible, then we can meet and talk about alternatives.

- Helena's response (11/25):
The viruses that were reported as biocontrols for Sclerotinia were 7, 1 of which has no available sequences. 5 of these are non-segmented viruses (1 fasta sequence for the whole genome), and 1 has 2 segments (so 2 fasta sequences as genome). The file "Sclerotinia_biocontrol_mycovirus_nucleotide.fasta" has therefore, 7 sequences in total. They are not aligned, because I was waiting to see what you thought of what I asked you about the alignment. The file "Sclerotinia_biocontrol_mycovirus_protein.fasta" has the same 6 virus, but now it contains the protein sequences as they can be found in genbank (as a polyprotein for some of them, or as individual proteins for the others). These are also, not aligned. 

The files "mycovirus_genbank_all_refseq_nucleotide_unique.fasta" and mycovirus_genbank_all_refseq_protein_unique_proteindb.fasta" contain all the mycoviruses in genbank (including the Sclerotinia controls that I copied into the other files). None of the files is yet aligned.

The ~110 from ViralZone will indeed pose the same problem, but I don't think it is impossible, we just need to find an alignment that will work for what you want. An alignment can be made with all of them, I just think it will be too much noise to be useful. If you want, we can meet so that I can show you what I am talking about when I say that the viruses are too different, and we can decide what to use for the alignment. 


## After meeting 12/3

We will focus on the two files:
- `mycovirus_genbank_all_refseq_nucleotide_unique.fasta` (which have label 0)
- `Sclerotinia_biocontrol_mycovirus_nucleotide.fasta` (which have label 1)

These files have 350 and 7 viral sequences:
```shell
(master) $ grep ">" Sclerotinia_biocontrol_mycovirus_nucleotide.fasta | wc -l
       7
(master) $ grep ">" mycovirus_genbank_all_refseq_nucleotide_unique.fasta | wc -l 
     350
```

## Toy dataset

- Data input (genomes) of Staph bacteria: `core_gene_alignment-narsa.aln` 
    - rows=bacterial strains (individuals)
    - columns=nucleotide sites (features)
- Labels for Staph bacteria (0=susceptible to antibiotic, 1=resistant to antibiotic): `responses-staph.csv`

# Analyses

Next steps: We want to fit statistical/machine-learning models to accomplish two tasks:
- feature selection: identify genes associated with mycovirus fungus-killing capability
- prediction: for a given new genome, can we predict whether it will be fungus killer or not

Methods:
- regression (we need to explore penalized regression because we have more features than individuals)
- random forest
- neural networks
- ...

## Main difficulties of the project
- We do not have aligned sequences (can we align ourselves?)
- Input data is letters/categories: ACGT, cannot be treated as numbers 1234


## Previous work for toy dataset

Claudia had fit naive neural networks and random forest in Julia. All scripts in `scripts/previous-work`:
- `notebook.md`: explains pre-processing of the data
- `*.jl`: julia scripts, described in `notebook.md`


Next steps:
- wait for alignments
- use existing data files as "alignments" to start machine-learning and regression pipelines
- play with toy example