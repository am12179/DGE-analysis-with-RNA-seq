Introduction:

For my project, I used differential gene expression (DGE) analysis with RNA-seq, which is both pertinent
and a useful tool for the advancements of biomedicine. The data we are using is extracted from research done
by Beth Israel Deaconess Medical Center and whose methods consisted of transfecting MDA-MB-231 breast
cancer cells with 20nM control or NRDE2-targeting siRNAs, and collecting RNA after 48h for RNA-seq
analysis. In other words, I used RNA-seq libraries from three biological replicates from control cell lines and
three biological replicates in which the gene NRDE2 was silenced with RNAi. The libraries were single-end
(SE) sequenced on an Illumina NextSeq platform. To conduct a DGE analysis, I processed the raw data
and implemented a Salmon + tximport + DESeq2 workflow. The objective of this project is to characterize
differentially expressed genes that may be impacted by knocking down NRDE2.
A synopsis of the experiment can be found here:
http://bioconductor.org/packages/release/bioc/vignettes/DESeq2/inst/doc/DESeq2.html

Methods:

I first downloaded and unzipped all my fastq files from the project repository. I trimmed the fastqs with fastp
using length_required 75 and –n_base_limit 50 to automatically remove adapters from single end reads and
1
polyG sequences introduced on NextSeq platform. After Trimming the reads, I downloaded the human
reference transcriptome from the Ensembl website and then ran fastqc on the processed RNA-seq reads
separately on each sample. Next, I generated a MultiQC report. Then, I ran Picard tools NormalizeFasta
to strip everything after the transcript id, which is the first identifier in the Fasta header lines for each
transcript on both the reference files and reads that were aligned against the reference human transcriptome
that I retrieved from the Ensembl website. After, I created a Salmon index and then ran Salmon in mapping based mode using a command appropriate for single-end data. Finally, I conducted a standard analysis of
differential gene expression using DESEQ. Included in my analysis is a table with the total number of reads
and the mapping rate for each sample, the number of statistically significant genes at 0.05 FDR, the number
of biologically relevant differentially expressed genes defined using a change in gene expression of two-fold
or greater, a table with the 10 most highly significant differentially expressed genes, a sample PCA, an MA
plot, and dispersion-by-mean plot, and a raw p-value histogram. I am reporting shrunken log fold-change
estimates to normalize errors from sequencing and sampling and give better estimates of the log2 fold-change.
The statistical approach used for the multiple-test correction method was to use a false discovery rate (FDR)
adjusted p value, and I chose level of significance to be 0.05, so that genes with an adjusted p value greater
than .05 were rejected to avoid false positives. What was unusual in the multiqc report was that none of the
samples passed the sequence duplication levels and the per base sequence content. The library type Salmon
inferred for the input reads was stranded, derived from the reverse strand (SR)

Discussion:

My deseq analysis identified that there are 62 biologically and statistically relevant genes that are impacted by
knocking down NRDE2. I came to this conclusion by first developing a table with the total number of reads
and the mapping rate for each sample, a PCA plot which provided me with an indication of the distances
between sample gene expression profiles and helped me detect batch effect, and an MA plot which showed
gene expression distribution comparing my controls and treatments, highlighting differentially expressed
genes. The dispersion-by-mean plot I included in my analysis demonstrated the gene which do not follow
the modeling assumptions and thus have higher variability than others for biological (or technical) reasons,
and my histogram plot revealed an enrichment of low p-values. Ultimately, I found that after setting my
adjusted p value to less than 0.05 to reveal the number of statistically relevant differentially expressed, I had
3348 genes left. Then, I found that the number of biologically relevant differentially expressed genes defined
using a change in gene expression of two-fold was 62.
