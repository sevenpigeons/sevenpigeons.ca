+++
title="Undergrad's guide to High Performance Computing"
date="2026-04-20"
[extra]
toc=false
[taxonomies]
tags=["Guides"]
+++

## Some context

At some point during research (if it's not experimental), you will require use of some sort of simulations.

Physics[^0] is, after all, a science of creating Good Enough<sup>TM</sup> models of the world, and those models have to be tested.
But even with the simplifications that we bring to the complex real world, when everything is a mass on a spring,
you simply trade the complexity of calculations for the amount.
And these amounts can get rather large. Large enough to require way more power than your laptop can provide.
Sometimes it remains possible, alas just slow. Sometimes, because of very high RAM requirements, it becomes simply not possible (I'm looking at you [meep](https://meep.readthedocs.io/en/latest/)).

In situations like these we have to turn to specialized equipment, made just for an occasion: supercomputers, and in our case, supercomputer clusters.

The concept of a HPC (High Performance Computing) cluster is rather simple:
If one computer cant do a task, we split this task into smaller chunks, and each of those chunks to different computers. They do the work, we combine the results,
and have our solution.

We will get more into details later, but that is the grand idea.

## Ok where exactly does it go, what does it mean, and who's Compute Canada?
 

Digital Research Alliance of Canada (previously known as Compute Canada) is the central national organization that operates[^1]
and manages resources for digital research infrastructure in Canada.

They manage resource allocations amongst members (researchers, such as your supervisor I assume), basically allocating CPU hours based on applications same way
as NSERC would assign grants based on research proposals[^2].

Depending on what type of work you do (CPU-bound computing, GPU-heavy bio simulations, or ML), different amount of space and CPU/GPU hours will be allocated
on different clusters, operated by the Alliance. There are a total of 8 clusters, 4 of them being [General Purpose](https://docs.alliancecan.ca/wiki/National_systems#Compute_clusters),
meaning designed for a diverse type of tasks, one designed for large [parallel tasks](https://docs.alliancecan.ca/wiki/Trillium)
where you use more then 1000 CPU cores at the same time, and 3 are GPU clusters 
designed for AI research.[^3]

And you, brave soul, will be given a key to these mighty machines, and command them to do your bidding.

## How?







---
Sources:
1. [Research Alliance Canada wiki](https://docs.alliancecan.ca/wiki/Getting_started)

---
Remarks:
[^0]: I am making an example about physics because that's what I do.
[^1]: Technically, clusters are operated by separate entities and universities where they are located, but allocations are managed centrally by the Alliance.
[^2]: Ish. It's a bit* different the way comparisons are metaphors and not statements of equivalence but that's a problem you will have to worry about only when you are managing your own lab and therefore a bit out of scope of the guide.
[^3]: There are technically more, there is also a cloud specific instance, to run virtual machines, as well as [MonarQ](https://docs.alliancecan.ca/wiki/MonarQ/en), a 24-qubit
quantum computer operated by Calcul Québec. There are also 4 legacy clusters which are near their end of life and are being decommissioned but their storage is still online.
