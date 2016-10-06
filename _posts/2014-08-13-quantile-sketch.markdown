---
layout: post
title: "Quantile Problem in streaming model"
date: 2014-08-13 20:55:04 -0400
comments: true
categories: [Algorithm, Streaming, CS]
---
I will give a deterministic approximation algorithm that constructs a data structure through a single pass of the a data stream, uses only $O(\log n / \epsilon)$ words space. For all $k$, such data structure is able to give a $(1 + \epsilon)$-approximation to the $k$th smallest item.
<!--more-->
Before starting to describe our algorithm, let's first clarify the model we use, and I will also recap some basic terminologies for those who are not familiar with streaming algorithms.

## The Model
To make life easier, we only consider the most basic data stream model **vanilla model**. In such model, there will be a sequence of $m$ items, each item is an element among one of $1, 2, \ldots, n$.

## Some Terms
We say $\hat{Q}$ is a $(1 + \epsilon)$-approximation to an positive real $Q$ if $|Q - \hat{Q}| < \epsilon Q$.

## The Problem
We slightly modified the classical Quantile Problem. In our setting, we want to design an algorithm which produces a data structure $\sigma$ via processing a data stream for only one pass. After the processing step, $\sigma$ is able to give $(1 + \epsilon)$-approximation to the $k$th smallest item (items may repeat) in the data stream, of course, $k$ is an integer and can be arbitrarily chosen from $[1, m]$. Our goal is to minimize the space needed to produce this $\sigma$ and in the meantime, the processing time per item should be as small as possible.

## The Algorithm
The original idea is quite straightforward: we simply maintain a sequence of buckets. The $i$th bucket $B_i = ( ~(1 + \epsilon)^{(i-1)}, (1 + \epsilon)^i~]$, and we also associate a counter $C_i$ to each $B_i$. It is clear that we can use $C_i$ to record how many items are trapped by the bucket $B_i$, through only one pass of the stream. Hence the sequence of $B_i$ is exactly the data structure $\sigma$ we need.

Now let's consider how we can answer a query $k$. Here is the way:

   * scan the counters $C_i$ with incremental index
   * sum up all counters scanned until the total >= $k$
   * suppose we stop at the index $i$, return the average of the interval $B_i$


One can easily see that the returned value is a $(1 + \epsilon)$-approximation to the $k$th smallest item.

## Performance Analysis
Now we give the space and time usage for above algorithm.

### Space Usage
First, what is the space usage of the above algorithm? It is clear that the dominant space cost is the number of the buckets. Suppose we need at least $w$ buckets (hence the space usage will be bounded by $O(w)$), we need the following condition to ensure that we have enough buckets to cover all the items:

>   $(1 + \epsilon)^w = \Theta(n)$

It implies $w = \Theta(\frac{\log n}{\log (1 + \epsilon)}) \overset{\epsilon~\text{is small} }{=} \Theta(\frac{\log n}{\epsilon})$ hence the space is bounded by $O(\log n / \epsilon)$.

### Processing Time
Now we analyze the processing time per item. When a new item comes, the only thing we need to do is to locate the bucket that traps it, so that we can increment the corresponding counter. Suppose the item is $h$, then the index $i(h)$ of the corresponding bucket can be calculated by the formula:

>  $i(h) = \lceil \log h / \log (1 + \epsilon) \rceil$

Which shows the processing time per time is $O(1)$.

### Query Time
We are also interested in the processing time per query. It is trivial to see that the time is bound by $O(\log n / \epsilon)$ since we can give the correct answer anyway by scan all the buckets/counters. However, if we are not working in dynamic data stream i.e. all queries are answered after we have completed the single pass over the data stream, we will have better solution. First we spend $O(log n / \epsilon)$ time to scan the buckets, and then it will cost only $O(\log \log n + \log \frac{1}{\epsilon})$ for each query. We leave the details to the reader.


## Conclusion 
An python implementation of our algorithm can be found in [github](https://github.com/jiecchen/StreamLib/blob/master/streamlib/sketch/quantile.py) where they consider the ***Restrict Turnsitle Model*** which is  more general than our setting here. If the reader has some knowledge on *Sketch*, he will soon realize that our algorithm actually produces a linear mergable sketch, which is extremely useful in distributed computing. One can check the given `github` link to figure out how can two sketches be merged to one.

This algorithm came to me when [@meng6](http://meng6.net) asked the algorithm for Quantile Problem under streaming model, however, it is almost sure that such a straightforward algorithm should have been published in some paper long ago which I could not find.

Anyway, I think a deterministic algorithm using only $O(\log n / \epsilon)$ space and $O(1)$ processing time should be near-optimal for our problem.








