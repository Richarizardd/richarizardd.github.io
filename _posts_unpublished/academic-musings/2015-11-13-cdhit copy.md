---
layout: article
title: "#5: The connectomics of brain disorders"
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
* the ancient Roman physician Galen was one of the first to propose that the pathology in one part of the nervous system could affect other regions
* this hypothesis was revisted nearly two millennia later by Brown-Sequard, who suggested that the effects of focal brain damage on remote regions resulted from actions at a distance
* von Monakow extended this concept, coining the term diaschsis to describe the depression of function that can arise fin undamaged brain regions that are connected to a lesioned site
* Wernicke proposed an associative theory of brain function, in which higher-order cognitive processes arose from the itnegration of multiple, spatially distribted neural systems 

<h4 class="section-heading">From Imaging to Connectivity</h4>
* The exact wiring of the brain (connectome) includes nearly 100 billion neurons, with each neuron having on average 7000 synaptic connections to other neurons
* Traditional neuronanatomic methods of axonal tracing cannot scale up to very large networks
* a optimal imaging of neural activity allows us to monitor the activity of 10,000s of neurons simultaneously
* the idea is to reverse engineer the structure of neural networks given time series of neuron activity acquired from video data
* uses fluorescent molecules that respond to the binding of calcium ions by changing their fluorescence properties
* the first complete nervous system wiring diagram was accomplished modeling C. Elegans, a nematode worm in the 1980s
* deduced from reconstruction of electron micrographs of serial sections
* even the most advanced techniques for labeling individual nerons with distinguishable colors (Brainbow) require a difficult tracing of neuron ramifications, and the resolution of optical microscopes is insufficient to reliably visualize synapses
electron microscopy provides sufficient resolution, but not color coding, and yields voluminous amounts of data that is being analyzed very slowly
* another approach is reconstructing networks of interaction between neurons from patterns of activity (effective topology)
* partial connectomes for large nervous systems including the fruit fly and the mouse have since been produced with a combination of neuroanatomical techniques (Seung 2012)
* the efforts of reconstructing networks from neural activity can be traced back to a 2006 paper, which used the in-degree of a neuron to estimate hire firing
* the first major study used calcium imaging to identify "hub" neurons with a simple cross-correlation approach (Bonifazi 2009, Vogelstein 2009, Mishchenko 2011)
* the idea was the first infer spike times as a Bayesian inverse problem, then infer the GLM kernels of the supposed GLM for the neurons
* one criticsm of the approach is that it was proven successfuly only with data generated from teh same model as the model used for reconstruction
* real neurons are very diverse and an approach that is bound too tightly to a supposed model of a neurons may be plagued with artifacts
* this motivated model-free network reconstruction techniques (Stetter 2012, Orlandi 2013)
* effective topology reconstruction is not the same thing as establishing a map of anatomically correct structural connections 
* some anatomical connections may be missed by the reconstruction because some synapses may be "dormant", some interactions may be invisble due to signal canceleation in feed-back circuits, two or more neurons may be overlapping and their signals 
* some spurious connections may be inferred because some effective connections may be relayed by invisible neruons, and not correspond to actual synapses
* some effective connections may result from artifacts of network reconstruction algorithms, which rely on data limited in time resolution
some effective connections may reflect real influences mediated by collective network properties, rather than by pairwise interactions
* algorithmi reconstruction approaches i.e., effective topology, are far more scalable than anatomical axonal tracing
* Optogenomics is one of the most promising methods because it offers the possibility of intervening simultaneously on hundreds of neurons
* Optogenetic paradigms use genetic techniques to induce the expression of light-activated ion channels into a living organism such that focused shining of light can trigger action potentials in the targeted neurons
* There is a wide variety of network structure reconstruction algorithm from observed time series, stemming from applied neuroscience, but also machine learning and econometrics, which have fueled the area of causal inference from temporal data with numerous novel techniques (Popescu-Guyon, 2013)
* . In neuroscience, simple linear auto-regressive (AR) models underlying Granger causality do not capture well the complexity of neural signals. A non-linear version of Granger causality called Transfer Entropy (Schreiber, 2000), which reduces to Granger causality for simple AR models (Barnett et al., 2009) is gaining popularity (Wibral, 2011; Battaglia, 2012; Stetter, 2012)
* Another approach, which is not limited to statistics of pairs of variables, is to use score-based methods, by performing a search in the space of all possible architectures, guided by an objective function assessing the goodness of signal reconstruction (possibly penalized to favor sparse connectivity)
* Such methods include Bayesian approaches such as Dynamical Causal Modeling (DCM) (Friston, 2003), which compare data generating models formulated in terms of differential equations modeling the dynamics of hidden states in the nodes of a probabilistic graphical model, where conditional dependencies are parameterized in terms of directed effective connections. Other related methods include L1 and/or L2 penalized regression methods (Ryali, 2012).
