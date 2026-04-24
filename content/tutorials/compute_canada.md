+++
title="Undergrad's guide to High Performance Computing"
date="2026-04-20"
[extra]
toc=false
[taxonomies]
tags=["Guides"]
+++

## Some context

Sometimes, during research (if it's not experimental), you will require use of some sort of simulation.

Physics is after all, a science of creating Good Enough<sup>TM</sup> models of the world, and those models have to be tested.
But even with the simplifications that we bring to the complex real world, when everything is a mass on a spring,
you simply trade the complexity of calculations for the amount.
And these amounts can get rather large. Large enough to require way more power to calculate then your laptop can provide.
Sometimes it remains possible, alas just slow. Somestimes, because of very high RAM requirements, it becomes simply not possible (I'm looking at you [meep](https://meep.readthedocs.io/en/latest/))

In situations like these we have to turn to specialized equipement, made just for an occasion: supercomputers, and in our case, supercomputer clusters.

The concept of a HPC (High Performance Computing) cluster is rather simple:
If one computer cant do a task, we split this tast into smaller chunks, and each of those chunks to different computers. They do the work, we combine the results,
and have our solution.

We will get more into details later, but that is the grand idea.

## Ok where exactly does it go, what does it mean, and who's Compute Canada?
 

Digital Research Alliance of Canada (previously known as Compute Canada) is the central national oranization that operates[^1]
and manages resources for digital research infrastructure in Canada.

They manage resource allocations amongst members (researchers, such as your supervisor I assume), basically allocating CPU hours based on applications same way
as NSERC would assign grants based on research proposals[^2].

Depending on what type of work you do (CPU-bound computing, GPU-heavy bio simulations, or ML), different amount of space and CPU/GPU hours will be allocated
on different clusters, operated by the Alliance.







---
Sources:
1. [Research Alliance Canada wiki](https://docs.alliancecan.ca/wiki/Getting_started)

---
Remarks:
[^1]: Technically, clusters are operated by separate entities and universityes where they are located, but allocations are managed centrally by the Alliance.
[^2]: Ish. It's a bit* different the way nothing is actully the same but thats a problem you will have to worry about only when you are managing your own lab and therefore a bit out of scope of the guide.
