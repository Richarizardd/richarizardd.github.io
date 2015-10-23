---
layout: article
title: "An Immersion into Vinyl in | July"
title2:  "An Immersion into Vinyl in | July"
excerpt: "The four building blocks of the universe are fire, water, gravel and vinyl. - Dave Barry"
excerpt2: "The four building blocks of the universe are fire, water, gravel and vinyl. - Dave Barry"
author1: richard_chen
categories: blog
modified: 2014-08-14 12:00:00
tags: []
toc: false
image:
 feature: recordplayer.jpg
 teaser: recordplayer.jpg
share: false
comments: true
---

# 1. Basic Vectorization
max.eig is a function that generates a N x N square matrix of uniformly distributed random numbers with a standard deviation of sigma. It then finds and finds the corresponding eigenvalues, and selects the eigenvalue with the largest modulus.

{% highlight r %}
max.eig <- function(N, sigma) {
  d <- matrix(rnorm(N**2, sd = sigma), nrow = N)
  E <- eigen(d)$values
  abs(E)[[1]]
}
{% endhighlight %}


We can look at the distribution of the returned max eigen values of max.eig by using sapply. sapply will create a vector E that will find the max eigenvalue of a 5 x 5 matrix with a s.d. of 1 over 1000 iterations.

{% highlight r %}
E <- sapply(1:1000, function(n) {max.eig(5, 1)})
head(E)
{% endhighlight %}



{% highlight text %}
## [1] 1.664203 2.347467 2.530082 1.923114 2.341003 2.709824
{% endhighlight %}



{% highlight r %}
summary(E)
{% endhighlight %}



{% highlight text %}
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.9601  1.9200  2.2670  2.3160  2.6270  4.8690
{% endhighlight %}



{% highlight r %}
hist(E, col = "#008B8B")
{% endhighlight %}

![center](/figs/Parallel Processing/unnamed-chunk-2-1.png) 

sapply is a powerful function that will let you sequentially compute tasks much faster than a for loop.

{% highlight r %}
E <- sapply(1:5, function(n) {max.eig(n, 1)}) # varying 1 parameter
E <- sapply(1:5, function(n) {sapply(1:3, function(m) {max.eig(n, m)})}) # varying 2 parameters
{% endhighlight %}

# 2. Enter foreach
foreach is available from CRAN to install.

{% highlight r %}
library(foreach)
{% endhighlight %}

foreach is a library that will allow you to do tasks in parallel in R. It works even faster than sapply in doing "for loop"-like tasks.

{% highlight r %}
times(10) %do% max.eig(5,1)
{% endhighlight %}



{% highlight text %}
##  [1] 1.394901 1.529347 2.074902 3.353100 1.684946 2.011387 2.009364
##  [8] 1.529731 2.918971 2.184282
{% endhighlight %}

We can systematically vary the parameters in foreach, but it returns it as a list.

{% highlight r %}
foreach(n = 1:5) %do% max.eig(n, 1)
{% endhighlight %}



{% highlight text %}
## [[1]]
## [1] 1.511101
## 
## [[2]]
## [1] 2.164093
## 
## [[3]]
## [1] 1.834406
## 
## [[4]]
## [1] 2.842267
## 
## [[5]]
## [1] 1.837001
{% endhighlight %}

.combine = c allows us to return it as a vector.

{% highlight r %}
foreach(n = 1:5, .combine = c) %do% max.eig(n, 1) # varying one parameter
{% endhighlight %}



{% highlight text %}
## [1] 0.3294457 1.3436273 2.4290669 2.4444265 2.3302748
{% endhighlight %}

We can nest foreach using the "%:%" operator.

{% highlight r %}
foreach(n = 1:5, .combine = rbind) %:% foreach(m = 1:3) %do% max.eig(n, m)
{% endhighlight %}



{% highlight text %}
##          [,1]      [,2]      [,3]    
## result.1 0.2474897 0.8514096 1.414274
## result.2 2.300006  1.419135  2.998122
## result.3 1.355451  6.259735  5.429916
## result.4 1.674858  2.915321  6.759458
## result.5 1.5787    5.644294  5.813289
{% endhighlight %}

We can also filter using "when".

{% highlight r %}
library(numbers)
foreach(n = 1:100, .combine = c) %:% when (isPrime(n)) %do% n # you can also add filters using "when"
{% endhighlight %}

# 3. Enter doParallel
The doParallel package is a parallel processing package in Windows that lets you parallelize tasks across multiple cores. Cores need to be registered using "registerDoParallel."

{% highlight r %}
library(doParallel)
registerDoParallel(cores=4)
{% endhighlight %}

We can measure the runtime of a serial execution of "foreach" and a parallel execution of "foreach", specified with the "%dopar%" operator. Output is supressed because it is slightly unreadable, but the time elapsed is commented in.

{% highlight r %}
library(rbenchmark)
benchmark(
    foreach(n = 1:50) %do% max.eig(n, 1), # Serial execution ~ 6.20 seconds
    foreach(n = 1:50) %dopar% max.eig(n, 1) # Parallel execution ~ 3.14 seconds
)
{% endhighlight %}


# 4. Enter doSNOW
The doSNOW package is a similar package to doParallel, and lets you specify the type of cluster you want to parallize. "SOCK" stands for Raw Sockets via the Operating System. Other Common cluster types include "MPI" (Message-Passing-Interface) and "SNOW" (Simple Network of Workstations). If you're running code on your personal computer like I am, your cluster type is "SOCK".

{% highlight r %}
library(doSNOW)
{% endhighlight %}



{% highlight text %}
## Error in library(doSNOW): there is no package called 'doSNOW'
{% endhighlight %}



{% highlight r %}
cl = makeCluster(4, type = "SOCK") # Using a SOCK cluster
{% endhighlight %}



{% highlight text %}
## Error in loadNamespace(name): there is no package called 'snow'
{% endhighlight %}



{% highlight r %}
registerDoSNOW(cl)
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): could not find function "registerDoSNOW"
{% endhighlight %}

Once again, we can benchmark to measure performance. A noticeable difference between doParallel and doSNOW is that you have to tell doSNOW to stop using multiple clusters. Output is supressed because it is slightly unreadable, but the time elapsed is commented in.

{% highlight r %}
benchmark(
  foreach(n = 1:50) %do% max.eig(n, 1), # Serial execution: 6.19 seconds
  foreach(n = 1:50) %dopar% max.eig(n, 1) # Parallel execution: 3.29 seconds
)
{% endhighlight %}

### doSNOW Functions and Calls
doSNOW allows you to programmatically control how tasks are parallelized across clusters. 
"clusterCall(cl, fun, x)" asks each worker process to call function "fun" with arguement "x" across "cl" clusters.

{% highlight r %}
clusterCall(cl, min, rnorm(5))
{% endhighlight %}



{% highlight text %}
## Error in defaultCluster(cl): object 'cl' not found
{% endhighlight %}

"clusterApply(cl, vec, fun)" assigns one element of the vector "vec" to each worker, has each worker call the function "fun" on assigned element. If more elements > workers, then workers ares reused cyclically.

{% highlight r %}
clusterApply(cl, 1:10, function(x) print(1:x))
{% endhighlight %}



{% highlight text %}
## Error in defaultCluster(cl): object 'cl' not found
{% endhighlight %}

### A More Complicated Example
Let's generate a big array of normally distributed random numbers. We'll use "sapply" to calculate bootsrap estimates for the stand deviation of the median of the columns.

{% highlight r %}
library(boot)
random.data <- matrix(rnorm(1000000), ncol = 1000)
bmed <- function(d, n) median(d[n])
sapply(1:100, function(n) {sd(boot(random.data[, n], bmed, R = 10000)$t)})
{% endhighlight %}

We can parallelize this process by making the random matrix and the bootstrap function available across the cluster.

{% highlight r %}
library(boot)
clusterExport(cl, c("random.data", "bmed")) # 
results = clusterApply(cluster, 1:100, function(n) {
  library(boot)
  sd(boot(random.data[,n], bmed, R = 10000)$t)
})
{% endhighlight %}

The foreach implementation is faster and neater.

{% highlight r %}
results = foreach(n = 1:100, .combine = c) %dopar% {
  library(boot)
  sd(boot(random.data[, n], bmed, R = 10000)$t)
}

stopCluster(cl) # Stops the cluster
{% endhighlight %}
