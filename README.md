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

### Aligners for RNA-seq

- STAR ([paper](https://academic.oup.com/bioinformatics/article/29/1/15/272537?login=false) / [github](https://github.com/alexdobin/STAR)): STAR is a read aligner designed for splice aware mapping typical of RNA sequencing data. STAR stands for Spliced Transcripts Alignment to a Reference, and has been shown to have high accuracy and outperforms other aligners by more than a factor of 50 in mapping speed, but it is memory intensive.
- Why do I see non-integer / float values in the expression matrix derived from Salmon? Rob Patro's answer: _Salmon performs probabilistic allocation of the fragments, to obtain a maximum likelihood estimate of the transcript abundances given the observed data (read alignments)_. Also explained in the documentation: _Observed library format counts. When run in mapping-based mode, the quantification directory will contain a file called lib_format_counts.json. This JSON file reports the number of fragments that had at least one mapping compatible with the designated library format, as well as the number that didn't. It also records the strand-bias that provides some information about how strand-specific the computed mappings were. Finally, this file contains a count of the number of mappings that were computed that matched each possible library type. These are counts of mappings, and so a single fragment that maps to the transcriptome in more than one way may contribute to multiple library type counts. Note: This file is currently not generated when Salmon is run in alignment-based mode._

TODO: include videos from Shirley Liu

### Transcript quanfication

- Salmon( [paper](https://www.nature.com/articles/nmeth.4197) / [documentation](https://salmon.readthedocs.io/en/latest/salmon.html) ), from the nf-core pipeline documentation: _Salmon from Ocean Genomics is a tool for wicked-fast transcript quantification from RNA-seq data. It requires a set of target transcripts (either from a reference or de-novo assembly) in order to perform quantification. All you need to run Salmon is a FASTA file containing your reference transcripts and a set of FASTA/FASTQ/BAM file(s) containing your reads. The transcriptome-level BAM files generated by STAR are provided to Salmon for downstream quantification. You can of course also provide FASTQ files directly as input to Salmon in order to pseudo-align and quantify your data by providing the --pseudo_aligner salmon parameter. The results generated by the pipeline are exactly the same whether you provide BAM or FASTQ input so please see the Salmon results section for more details._

### DESeq2

DESEQ2 is one of the main "Swiss Army knives" used to analyse RNA-seq data. As such, it is crucial to understand how it works, which data structures utilizes and how it interacts with other packages:

- [Original DESeq paper](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2010-11-10-r106)
- [DESeq2 paper](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-014-0550-8#Bib1)
- [RNA-seq workflow](https://master.bioconductor.org/packages/release/workflows/vignettes/rnaseqGene/inst/doc/rnaseqGene.html): an end-end RNA-seq analysis using BioConductor packages (from the developers of DESeq2)
- [DESeq2 workflow tutorial | Differential Gene Expression Analysis | Bioinformatics 101](https://www.youtube.com/watch?v=OzNzO8qwwp0): DESeq2 tutorial by the bioinformagician on youtube

Why does DESeq2 use the DESeqDataSet class and not SummarizedExperiment? From the RNA-seq workflow above: _In DESeq2, the custom class is called DESeqDataSet. It is built on top of the SummarizedExperiment class, and it is easy to convert SummarizedExperiment objects into DESeqDataSet objects, which we show below. One of the two main differences is that the assay slot is instead accessed using the counts accessor function, and the DESeqDataSet class enforces that the values in this matrix are non-negative integers. A second difference is that the DESeqDataSet has an associated design formula. The experimental design is specified at the beginning of the analysis, as it will inform many of the DESeq2 functions how to treat the samples in the analysis (one exception is the size factor estimation, i.e., the adjustment for differing library sizes, which does not depend on the design formula). The design formula tells which columns in the sample information table (colData) specify the experimental design and how these factors should be used in the analysis._

Design matrices and formulas are a key component of DESeq2 and other tools for differential expression analysis. [This is a great review](https://f1000research.com/articles/9-1444) to learn how to create them properly and specify the right covariates.

These are the 5 steps carried out by the DSEq function:

1. Estimating size factors
2. Estimating dispersions
3. gene-wise dispersions estimates
4. mean-dispersion relationship
5. final dispersion estimates
6. fitting model and testing

## Clustering

Which distance metric should I use?

- [Great discussion on stackexchange](https://stats.stackexchange.com/questions/459063/best-practices-in-the-selection-of-distance-metric-and-clustering-methods-for-ge). Key questions: _should genes that show the same pattern, but have different levels of overall expression go into the same group (correlation based distance) or different groups (difference based distance)? Is the pattern more important the the overall expression level ? If two genes anti-correlate does that mean they are related and be in the same group, or in different groups (does sign matter)? Should larger deviations be "punished" more (euclidean distance), or all magnitudes of difference are equally important (manhattan distance)?_
- [Check figure 10.13 from the 1st version from the book Introduction to Statistical Learning ](https://www.stat.berkeley.edu/users/rabbee/s154/ISLR_First_Printing.pdf) for an intuitive understanding of why the choice of distance metric matters.

TODO
Negative binomial...
Salmon...
tx import...
Transcript annotations: GENCODE, RefSeq, Ensembl, need to understand their differences and how to convert between them

## Enrichment analysis

- [Great GSEA tutorial from chatomics (Ming Tang](https://www.youtube.com/watch?v=IKCDQEpuJDA)
- [ClusterProfiler has great documentation](https://yulab-smu.top/biomedical-knowledge-mining-book/clusterprofiler-go.html) on how to run, interpret and plot results from enrichment analysis

Should one perform over representation anlaysis (ORA) or gene set enrichment analysis (GSEA)? [Answer](https://yulab-smu.top/biomedical-knowledge-mining-book/enrichment-overview.html#ora-algorithm):

_A common approach to analyzing gene expression profiles is identifying differentially expressed genes that are deemed interesting. The ORA enrichment analysis is based on these differentially expressed genes. This approach will find genes where the difference is large and will fail where the difference is small, but evidenced in coordinated way in a set of related genes. Gene Set Enrichment Analysis (GSEA)(Subramanian et al. 2005) directly addresses this limitation. All genes can be used in GSEA; GSEA aggregates the per gene statistics across genes within a gene set, therefore making it possible to detect situations where all genes in a predefined set change in a small but coordinated way. This is important since it is likely that many relevant phenotypic differences are manifested by small but consistent changes in a set of genes._

  
## ATAC-seq

### Reviews

- [https://www.nature.com/articles/s41596-022-00692-9](https://www.nature.com/articles/s41596-022-00692-9)


### TF footprinting

[Biases: Tn5 transposase has cleavege preferences that need to be modeled by TF footprinting tools](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1642-2)

TOBIAS ([paper](https://www.nature.com/articles/s41467-020-18035-1) / [wiki](https://github.com/loosolab/TOBIAS/wiki) ): Transcription factor Occupancy prediction By Investigation of ATAC-seq Signal. Key steps:

1. [ATACorrect](https://github.com/loosolab/TOBIAS/wiki/ATACorrect): correct BigWig file removing the sequence-specificity of the Tn5 transposase
2. [ScoreBigwig](https://github.com/loosolab/TOBIAS/wiki/ScoreBigwig): calculate TOBIAS footprint score by finding signals of depleted accessibility (TF binding) flanked by regions with enriched accessibility
3. [BINDetect](https://github.com/loosolab/TOBIAS/wiki/BINDetect): include TF motifs to find differentially regulatory signals between conditions.


