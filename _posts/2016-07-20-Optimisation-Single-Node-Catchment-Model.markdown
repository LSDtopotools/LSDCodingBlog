---
layout: post
title: Optimising LSDCatchmentModel: Part II Larger datasets
tags: OpenMP parallelisation HPC LSDCatchmentModel
---

In an earlier post, I looked at how the LSDCatchmentModel scaled up to running on a single cluster-computer compute-node. The example ran tests on a small catchment in hydrological mode only (i.e. no erosion modules turned on). The algorithm for water routing used in the model (Bates et al., 2010), had already been well investigated and tested in other parallel implementations, showing a favourable speed up on multiple CPUs. The number of grid cells in the Boscastle simulation was around 600,000 - covering an area of around 12km$^2$ at 5m resolution.

In this simulation I looked at a larger data set - to ssee how the code copes with a much bigger memory and cpu load. The Ryedale catchment was used (Area c. 50km$^2$, 18m grid cells). The advice given in the user guide to this particular computing cluster (ARCHER) suggests not to split the code between multiple physical processors and their respective memory regions  (see the diagram in this post for clarification), so a range of tests using a variety of configurations: Single processor/two procesors, and hyperthreading/no hyperthreading. 

In short, as with the Boscastle tests, the best speed up was found with using the maximum number of available cores (with hyperthreading turned on, this is 48 CPUs - 24 per physical processor.). However, the speed up was not as pronounced. Most simulations that completed took around 21-22 hours to complete. (Simulations taking longer than 24 hours are terminated by the system.)

For example moving from a simulation that used 24 cores to 48 via hyperthreading only resulted in a speed up of around 1%, in contrast to the samller dataset simulation which saw speed ups of around 50%.

