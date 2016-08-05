---
layout: post
title: LSDCatchmentModel scaling with OpenMP
tags: LSDCatchmentModel OpenMP HPC
---

In an earlier post, I did a rough, back-of-the-envelope scaling benchmark with some simulations running on a multicore compute-node, including some simulations on my desktop PC. I've revisited this to make the scaling comparisons at bit more consistent (sticking to the compute node CPUs on ARCHER, even for the single thread/core runs). There's also a nice scaling graph, so everyone's happy. Gotta love a good scaling graph.

The simulation is with the Boscastle catchment in Cornwall, simulating a flood event over 48 hours. Two sets of simulations are done, one running as a hydrological model only, and the other with erosion and sediment transport switched on, which is generally a bit mode compute intensive.

![boscastle_speed_up](/images/blog_boscastle.png)

Obviously the simulations with the erosion routines switched on take a lot longer to do: They don't parallelise quite so well, and there's a lot more computation with all the different grain sizes etc. While the hydro model speed up is roughly linear all the way up to around 48 cores (you can just see it tailing off perhaps around 40 cores...) the erosion simulations quite clearly state to tail off around 24 cores onwards.

The actual run times can be compared with the graph below:

![boscastle_runtime](/images/blog_boscastle2.png)

Actual run times with erosion routines on are approaching double that with just the hydrological model, in serial, however as both codes scale up over more CPUs, the gap narrows, relatively speaking.
