# gget_examples

This repository contains examples for [`gget`](https://github.com/pachterlab/gget).

`gget` is a free and open-source command-line tool and Python package that enables efficient querying of genomic databases. `gget`  consists of a collection of separate but interoperable modules, each designed to facilitate one type of database querying in a single line of code.  

`gget` currently consists of the following nine modules:
- **gget ref**
Fetch FTPs and metadata for reference genomes and annotations from [Ensembl](https://www.ensembl.org/) by species.
- **gget search**
Fetch genes and transcripts from [Ensembl](https://www.ensembl.org/) using free-form search terms.
- **gget info**
Fetch extensive gene and transcript metadata from [Ensembl](https://www.ensembl.org/), [UniProt](https://www.uniprot.org/), and [NCBI](https://www.ncbi.nlm.nih.gov/) using Ensembl IDs.
- **gget seq**
Fetch nucleotide or amino acid sequences of genes or transcripts from [Ensembl](https://www.ensembl.org/) or [UniProt](https://www.uniprot.org/), respectively.
- **gget blast** 
BLAST a nucleotide or amino acid sequence against any [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) database.
- **gget blat**  
Find the genomic location of a nucleotide or amino acid sequence using [BLAT](https://genome.ucsc.edu/cgi-bin/hgBlat).
- **gget muscle** 
Align multiple nucleotide or amino acid sequences against each other using [Muscle5](https://www.drive5.com/muscle/).
- **gget enrichr** 
Perform an enrichment analysis on a list of genes using [Enrichr](https://maayanlab.cloud/Enrichr/).
- **gget archs4**
Find the most correlated genes or the tissue expression atlas of a gene of interest using [ARCHS4](https://maayanlab.cloud/archs4/).

![alt text](https://github.com/pachterlab/gget/blob/main/figures/gget_overview.png?raw=true)
