---
layout: post
title: 'Formats for genome coverage in graphs'
---

In my work with genome graphs, I have noticed a gap in the ecosystem of file formats and tools.
Particularly, I was trying to get the read depth at each nucleotide on every node in a genome graph.
`vg pack`  is one tool that efficiently solves this task {% cite hickey_genotyping_2020 %}.
Despite this, the data cannot be easily accessed.
The output is a serialized file format that is created and loaded internally by `vg` using functionality in `vg/src/packer.cpp`.
But so far, I have not seen any option to subset and convert this data into a tabulated file format that can be loaded from scripts to be visualized.
The reason for this seems to be two fold.

Fist, the current research focus is on graph construction and variant genotyping.
These algorithms are written within single tools using their own binary file formats that are optimized for easy loading and saving.
Since genome coverage data never has to leave the ecosystem of a single tool, no code was ever written to export it to another interoperable or human readable format.
The second problem is the availability of an established format for exporting coverage data.
The plain text GFA and GAF formats are already widely used to describe the structure of graphs or the alignment of sequences to graphs.
However, no equivalent exists to show the read depth on a nucleotide in a genome graph.

Such formats will certainly be necessary in the future for genome tracks similar to those found in the UCSC Genome Browser.
Therefore, it might be worth extending existing formats from the linear genome to the graph genome.
For instance,  the wiggle track format (WIG) is often used for high density tracks.
In its current form, an entry looks something like this:

```
fixedStep  chrom=chrN
[span=windowSize]
  dataValueA
  dataValueB
  ...
```
Wiggle avoids repeating the chromosome names for each entry, which saves space because a small number of long contigs support very dense data.
This layout is easily ported to graph coordinates by replacing the chromosome field with a node field.
Provided that the average node length is sufficiently large, such a format could prove economical.
However, the overhead increases as the average node length decreases because metadata lines are required for every node.
Alternatively, we could specify a number of haplotypes along which to count the coverage.
Then, the node field could be replaced by a path through the graph, giving runs of sequence that are much longer than a single node.
The haplotypes could be described through a path matching string like the one found in the [GAF](https://github.com/lh3/gfatools/blob/master/doc/rGFA.md#the-graph-alignment-format-gaf) format.

Since haplotypes also enumerate edges, we could also add the coverage at each edge in addition to nodes.
At the cost of additional complexity, a graph wiggle format could look as follows:
```
fixedStep  path=<path matching string>
[nucleotides, span=windowSize]
  nucletideDataValueA
  nucleotideDataValueB
  ...
[edges]
  edgeDataValueA
  edgeDataValueB
  ...
```

The other alternative is the familiar BED format.
It is the simplest format to work with, because it just separates fields with a tab character:
```
chrom chromStart chromEnd dataValue
```
Again, by replacing the chromosome field with a node identifier, we could annotate nodes with coverage data.
However, this format is wasteful when the data is either very dense or at very high resolution.
It would only be practical for sparse annotations.
Here, incorporating haplotypes in a self contained fashion is more difficult.
Without a header that names the haplotypes, each entry would have to describe entire paths again and again.

In both cases, `wigToBigWig` and `bedToBigBed` provide binary equivalents.
The same could be done for any emerging graphical format in order to save disk space and increase I/O speed.

# References

{% bibliography --cited %}

<!-- Local Variables: -->
<!-- org-ref-default-bibliography: /Users/cgroza/git/cgroza.github.io/_bibliography/references.bib -->
<!-- End: -->
