---
layout: post
title: "Is life submodular？"
date: 2016-03-07 00:18:17 -0500
comments: true
categories: personal
---
我最近写了一篇关于 submodular maximization 的 survey，覆盖了 submodular functions 的一些基本的性质和一些经典的应用。Submodular maximization 已经被研究很久了， 最近十来年又忽然变成了一个 hot topic （当然不是像 DL 那种火爆）： 单单在 2013 年，  ICML， NIPS 和 SPAA 的 best paper award 都给了 submodular maximization 相关的文章， SODA 上甚至开始了出现了专门的 track。我猜可能是因为大家发现在 ML 和 social network 上， 用这套工具可以得到一些理论结果。 Streaming Model 和 MapReduce Model 下的 submodular maximization 只是最近几年才开始被研究，但是已经出现了不少很好的结果， 所以在我的 survey 里重点介绍了这两个模型下的算法和理论结果。感兴趣的可以移步 [Survey on Submodular Maximization](https://github.com/jiecchen/SubmodularLib/raw/master/research/survey/survey.pdf). 我正在考虑写一个可以用在 spark 或者 hadoop 上的 submodular optimization 的库， 已经写了一些试验性质的代码。


现在开始讲讲一些跟标题相关的。 一个 set function 是 submodular 的， 大致是说原来的基底越大， 新增加一个元素所带来的边际收益就越小。这种类型的函数，在人类的经济生产活动中反复出现。 我在看 Jeff Bilmes 开的 submodular optimization 的公开课的时候，注意到他问了一个很有趣的问题，

> “is your life submodular or supermodular?”

以人类社会的复杂性，一个人的人生当然不适合用简单的函数来建模。 但是听到 Jeff 这么一问， 我还是放声笑了出来。以我对社会的观察，至少在幸福感上，人通常倾向于是 submodular 的。拥有的财富越多，在挣到一笔小钱就容易失去那种自豪和兴奋的感觉。很多人谈起，即使第一份爱情没有善终，那种刻骨铭心的感觉在今后再也没有重新体会到。因为根据 submodularity， 那时候的边际收益是最大的，从无到有的体验是最难以忘怀的。许多经历过许多次爱情的人，在开始一段新的感情的时候，常常就很难体会到怦然心动的感觉了。诸如此类的例子还有很多啊，吃到美食的时候，第一口让人沉醉，略有饱腹感后幸福感就不会有那么高了。

Submodular Maximization 的理论里还有一个有趣的结论，大意是说，如果每一步都选择边际收益最大的行为，最终的收益，未必是全局的最优，但是至少能保证离全局的最优解不会太远。我犹记得 Funda 在讲贪心算法的时候，用自己的人生做了一下比较。 她的大意是说，自己在年轻的时候选择了读 PhD 的道路，当时看起来并不是很好的选择，因为她的好多同学本科毕业去工作，都已经挣了很多钱， 也有了小孩，家庭幸福美满。相比之下，她一个月只有紧巴巴的贫困线的收入，而且经常面临高压生活。但是过了好多年后，Funda 觉得自己的人生越来有趣了，当上了教授，有很高的收入又可以认识各种各样有趣的人。所以她说，“Greedy usually does not lead to global optimum. This rule applies to life as well.”  我虽然认可 Funda 的话，但是有必要觉得作一点补充。 假设人们的人生是一个 submodular function，那么每一步贪心的选择诚然并不会导致一个全局最优解，但是最后的结果通常也不会太烂。

总的来说，如何优化人生这个函数，是一个 NP-Hard 问题。 即使人生是可以快进快退，可以推倒重来，我们也需要指数级的尝试才可以得到最优解。更何况，人生的过程更像是一个 streaming model， 往者不可谏。而优化这样的人生，就更像是 streaming submodular maximization 了。


