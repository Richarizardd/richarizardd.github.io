---
layout: article
title: "#3: Design and Analysis and Single-Cell Sequencing Experiments"
excerpt: "Cole Trapnell"
categories: academic-musings
tags: [sequencing, genomics]
comments: true
featured: true
image:
  teaser: cell-teaser.png
  thumb: cell-thumb.png
---




<h3 class="section-heading">Introduction</h3>

* There are 210 different types of cells in the human body. Cataloging the cells of the human body is a maddeningly difficult problem.
* A single cell from this taxonomy is still diverse. 

&nbsp;&nbsp;&nbsp;&nbsp; &#8226; muscle cells can be divided by functional differences such as contraction speed and subcategorized by unique gene expression programs

* A taxonomy that separates cell types and subtypes with genes is still not good enough. Many cell types/subtypes have few (if any) reliable markers that can be used to purify a desired cell type/subtype. 

&nbsp;&nbsp;&nbsp;&nbsp; &#8226; "CD14+" monocytes actually consist of multiple subpopulations that share CD14 expression in common.

* Single-cell genomics is the turning point in cell biology. For the first time, we can assay the expression level of every gene in the genome across thousands of individual cells in a single experiment.


<h3 class="section-heading">Defining cell types and states requires single-cell assays</h3>

<h5 class="section-heading">Single-cell assays vs. bulk assays</h5>

* Bulk assays destroy crucial information by averaging the signal across the cell population

&nbsp;&nbsp;&nbsp;&nbsp; &#8226; Simpson's Paradox - Consider an experiment aiming to assess whether two genes show correlated expression levels across a population of cells. This problem is important, as some cell fate decisions are governed by a single pair of mutually exclusive regulators.

&nbsp;&nbsp;&nbsp;&nbsp; &#8226; The myeloid/erythroid fate decision in common myeloid progenitor cells is made via the mutually antagonistic transcription factors Pu.1 and GATA1. If the population consisted of a only one group of cells, one would say these two genes are mutually exclusive. If the population consisted of a mixture of two separate group of cells, one would say these genes are positively correlated. Grouping by cells/properly compartmentalizing the data by cell type leads to a qualitatively correct interpretation

2) TBD
