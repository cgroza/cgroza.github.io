---
layout: post
title: 'Segmenting signal in genome graphs'
---

# What is genome segmentation

Genome segmentation partitions the genome into segments in order to reveal regions with similar properties.
It is commonly applied to epigenomic profiles when searching for functional elements in the genome, such as genes, promoters, or enhancers.
Because functional elements show distinct signatures in ChIP-seq, ATAC-seq or DNAse-seq data, they can be detected and clustered by unsupervised learning algorithms.
In the case of Segway {% cite hoffman_unsupervised_2012 %}, the result is a set of labeled non-overlapping segments that cover every nucleotide in the genome.
The resulting labeled segments are then interpreted to infer the possible roles of genomic regions.
The second figure from this paper intuitively describes the annotation.

# Segmenting novel sequences in genome graphs
Human pangenome studies frequently find non-reference insertions up to 100 kbp in length {% cite ebert_haplotype-resolved_2021 %}.
A comprehensive human pangenome graph will contain several megabases of additional novel sequence that were previously difficult to access within a linear genome reference.
The additional DNA content could support functional elements that were previously unknown but that could be found by a segmentation algorithm.

However, the new sequences will no longer be organized as a set of linear strings.
Instead, they will be encoded as a sequence graph that describes all the haplotypes encountered in hundreds of human genome assemblies.
Therefore, a segmentation algorithm  must be ported to this new data structure.
A non-cyclic variation graph will certainly feature nested sequence variation.
That is, a large polymorphic insertion that happens to nest several other polymorphisms, such as SNPs, deletions or other smaller insertions.
Overall, I imagine that segmenting such a bubble with colored labels would look similar to this illustration:
<p align="center">
  <img width="250" height="auto" src="/assets/segmented_graph.svg" style="transform:rotate(90deg);">
</p>

Thus, the definition of a segment needs to be expanded to support bubbles that contain several distinct segment labels.
Perhaps this definition could be applied recursively until it reaches a base case that could be easily partitioned.
Alternatively, if the haplotypes of the samples are known, we might retrace haplotypes and associated signals from the graph and segment them linearly.
The location of the labeled segments could then be projected back to annotate the graph.

# Expectations and challenges

As suggested by the above figure, the segmentation of alternative paths through the same variable locus will sometimes show different features.
These would interesting alternative segmentations that I expect to happen at some low rate.
Given that a human pangenome graph is likely to include common SNVs and shorter indels, most bubbles should be segmented identically through all possible traversals.
I expect alternative segmentations to be restricted to larger bubbles that represent structural variants in functionally rich regions.
However, it is still possible for smaller variants to disrupt the binding motif of a transcription factor or other DNA binding proteins.
In such cases, the segmentations of alternative paths through small bubbles reveals allelic specific events.
Indeed, a genome graph provides a very convenient way to study allelic specific binding!

That said, there are challenges associated with a data structure that represents a large number of genomes.
A large portion of structural variation is in fact copy number variation.
The mappability of such sequences is inherently low, which hinders our ability to accurately measure genomic features with short read libraries.
This difficulty is partially addressed by paired-end libraries, but is ultimately limited by the fragment size distribution of the library.

That said, there are many decision yet to be made in the design of a human pangenome graph.
The final shape and size of this new genome reference will ultimately determine the new technical challenges we will face, as well as their potential solutions.



{% bibliography --cited %}

<!-- Local Variables: -->
<!-- org-ref-default-bibliography: /Users/cgroza/git/cgroza.github.io/_bibliography/references.bib -->
<!-- End: -->
