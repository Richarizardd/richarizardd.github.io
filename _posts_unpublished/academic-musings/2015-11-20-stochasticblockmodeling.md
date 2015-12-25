---
layout: article
title: "#4: Cd-hit: a fast program for clustering and comparing large sets of protein or nucleotide sequences"
excerpt: "Akiyama"
categories: academic-musings
tags: [sequencing, genomics]
comments: true
featured: true
image:
  thumb: bmc-thumb.png
  teaser: bmc-teaser.png
---

<h4 class="section-heading">Introduction</h4>
* C. elegans is the only organism whose connectome (pattern of neuronal connections) has been mapped extensively at the level of neurons and synapses (gold standard system for brain connectivity analyses)
* 2000 edges, 300 individual neurons
* deterministic models perform an exhaustive search over the sample space of all possible community strucutres and their corresponding partitions is atronomically large
* one class of deterministic models are community detection algorithms, which search for groups of nodes (modules) that comprise a high density of links within them and a lower density of links between them
* two such popular methods are the Fast Louvain algorithm and the Spectral algorithm
* stochastic blockmodeling views a network as a random realisation from a sample space of all possible networks
* all nodes ina given block share the sample probabilities of connection with other nodes in the network (stochastic equivalence)
* groups nodes together according to their similarity of connection patterns
* Erdos-Renyi Mixture Model can handle networks with several thousand nodes
* each block is modeled as a small Erdos-REnyi network with a common probability of internal connections, and the relationship between each block pair is also modeled as a separate Erdos-Renyi network specified by probability of inter-group connections
* paper shows that the ERMM can isolate biologically coherent groups of neurons and that it also provides a generative model yielding a good approximation of the network's degree distribution and means to simulate new data

<h4 class="section-heading">Algorithms</h4>
<h5 class="section-heading">Short word filtering</h5>

* Single-cell assays vs. bulk assays
* 1) Bulk assays destroy crucial information by averaging the signal across the cell population. Simpson's Paradox - Consider an experiment aiming to assess whether two genes show correlated expression levels across a population of cells. This problem is important, as some cell fate decisions are governed by a single pair of mutually exclusive regulators. For example, the myeloid/erythroid fate decision in common myeloid progenitor cells is made via the mutually antagonistic transcription factors Pu.1 and GATA1. If the population consisted of a only one group of cells, one would say these two genes are mutually exclusive. If the population consisted of a mixture of two separate group of cells, one would  say these genes are positively correlated. Grouping by cells/properly compartmentalizing the data by cell type leads to a qualitatively correct interpretation
* 2) 
