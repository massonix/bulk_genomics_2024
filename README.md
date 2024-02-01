# Bulk Genomics 2024

This repository contains a 2024 version of how to run end-end buk genomics / transcriptomics / epigenomics analysis


## Nf-core pipelines

While methods for single-cell genomics are still under active development, methods for bulk genomcis have been consolidated into robust pipelines accepted by the community. As such, computational biologists across the globe have curated reproducible and portable pipelines to run common bioinformatics workflows. The most successful attempt at that are the [nf-core pipelines](https://www.nature.com/articles/s41587-020-0439-x), which unleash the power of [Nextflow](https://www.nature.com/articles/nbt.3820) and [containers](https://www.nature.com/articles/s43586-023-00244-9.pdf) to achieve such reproducibility. These are the most common nf-core pipelines

- ATAC-seq ([documentation](https://nf-co.re/atacseq/2.1.2) / [github](https://github.com/nf-core/atacseq/tree/2.1.2) )
- ChIP-seq ([documentation](https://nf-co.re/chipseq/2.0.0) / [github](https://github.com/nf-core/chipseq/tree/2.0.0) )
- RNA-seq ([documentation](https://nf-co.re/rnaseq/3.14.0) / [github](https://github.com/nf-core/rnaseq/tree/3.14.0) )
- WGS ([documentation](https://nf-co.re/sarek/3.4.0) / [github](https://github.com/nf-core/sarek/tree/3.4.0) ): detection of somatic or germline variants from WGS data
- HiC ([documentation](https://nf-co.re/hic/2.1.0/docs/usage) / [github](https://github.com/nf-core/hic) )
- methylation ([documentation](https://nf-co.re/methylseq/2.6.0) / [github](https://github.com/nf-core/methylseq) )


To learn how NextFlow works I recommend [this YouTube playlist](https://www.youtube.com/watch?v=wbtMbJTo1xo&list=PLPZ8WHdZGxmUVZRUfua8CsjuhjZ96t62R) from Elvan Floden, one of Nextflow's developers.


## RNA-seq

DESEQ2 is one of the main "Swiss Army knives" used to analyse RNA-seq data. As such, it is crucial to understand how it works, which data structures utilizes and how it interacts with other packages:

- [Original DESeq paper](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2010-11-10-r106)
- [DESeq2 paper](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-014-0550-8#Bib1)
- [RNA-seq workflow](https://master.bioconductor.org/packages/release/workflows/vignettes/rnaseqGene/inst/doc/rnaseqGene.html): an end-end RNA-seq analysis using BioConductor packages (from the developers of DESeq2)
- [DESeq2 workflow tutorial | Differential Gene Expression Analysis | Bioinformatics 101](https://www.youtube.com/watch?v=OzNzO8qwwp0): DESeq2 tutorial by the bioinformagician on youtube

TODO
Negative binomial...
Salmon...
tx import...
