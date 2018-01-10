---
layout: article
title: "#1: Faster algorithms for string matching problems: matching the convolution bound"
excerpt: "Piotr Indyk"
categories: academic-musings
tags: [sequencing, genomics]
comments: true
featured: true
image:
  teaser: IEEE-teaser.png
  thumb: IEEE-thumb.png
---

<h2 class="section-heading">First Post Introduction</h2>
This is the first post in a series of many, where I'm going to read a paper everyday, and share my thoughts really quick on it. Because I want to develop this habit, not every post will be polished (certainly not the first few), but hopefully as I post more often, I'll learn how to post more!

<h2 class="section-heading">Summary</h2>
This paper gives a O(nlogn)-time Monte Carlo algorithm for the "string matching with don't cares" problem (\*-matching), which searches for degenerate patterns of P in degenerate patterns of T. The "\*" character is a placeholder that can represent any character in an alphabet. This problem is very similar to approximate matching using hamming distance. The novelty of the paper is that they are using a randomized boolean convolution, which allows their algorithm to matche the Fast Fourier Transform bound.

<h2 class="section-heading">Thoughts</h2>
This paper is definitely interesting, and while this field is incredibly new to me, you can bet this isn't the last time I'll be looking at this problem. I'll keep this post short, but perhaps in the future I'll summarize this paper/field better in a blog, and give it the attention it deserves.