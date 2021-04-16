---
layout: post
title: 'Segmenting genome graphs'
---

# What is genome segmentation

Genome segmentation partitions the genome into segments in order to reveal regions with similar properties.
It is commonly applied to epigenomic profiles when searching for functional elements in the genome, such as genes, promoters, or enhancers.
Because functional elements show distinct signatures in ChIP-seq, ATAC-seq or DNAse-seq data, they can be detected and clustered by unsupervised learning algorithms.
In the case of Segway {% cite hoffman_unsupervised_2012 %}, the result is a set of labeled non-overlapping segments that cover every nucleotide in the genome.
The resulting segmentation is then interpreted to infer the possible roles of a particular genomic sequence.

# Segmenting novel sequences in genome graphs
Human pangenome studies frequently find non-reference insertions up to 100 kbp in length {% cite ebert_haplotype-resolved_2021 %}.
A comprehensive human pangenome graph will contain several megabases of additional novel sequence that were previously difficult to access within a linear genome reference.
The additional DNA content could harbor functional elements that were previously unknown but that could be found by a segmentation algorithm.

However, the new sequences will no longer be organized as a set of linear strings.
Instead, they will be encoded as a sequence graph that describes all the haplotypes encountered in hundreds of human genome assemblies.
Therefore, a segmentation algorithm  must be ported to this new data structure.
A non-cyclic variation graph may feature nested sequence variation.
That is, a large insertion that is polymorphic within a population and that supports several other polymorphisms, such as SNPs, deletions or other smaller insertions.
Thus, the definition of a segment may need to be expanded to support bubbles that could contain several distinct segment labels.
Perhaps this definition could be applied recursively until it reaches a base case that could be easily partitioned.
Alternatively, if the haplotypes of the samples are known, the two haploid genomes could be retraced from the graph and segmented linearly.
The location of the labeled segments could then be projected back to annotate the graph.

The following illustration shows what a possible genome graph segmentation may look like.

{% bibliography --cited %}

<!-- Local Variables: -->
<!-- org-ref-default-bibliography: /Users/cgroza/git/cgroza.github.io/_bibliography/references.bib -->
<!-- End: -->
