---
layout: post
title: Runnning LSDPlotting tools with anaconda/conda
tags: python gdal LSDPlottingTools
---

[Anaconda](https://www.continuum.io/downloads) is a popular distribution of Python for Science-related projects. It comes with a very good Python IDE (Spyder) as part of the pacakge. As well as a python package manager that is easy to use called *conda*. (Which is better IMO than the default *pip* package manager). 

The LSDPlottingTools python scripts require a module called *gdal*, which for reading different types of geospatial data. Unfortnately this does not come with Anaconda as default, so you need to install it with conda.

`conda install gdal`

Appears to work fine at first (it installs many of the necessary dependencies. But when I launched Spyder IDE and tried to run a LSDPlottingTools script it complained that it could not find the `ligdal.so.20`. It turns out that conda has installed an older version of libgdal when it install gdal. If you run `conda list` you'll see that gdal is version 2.0 but libgdal is version 1.1 or similar. Luckily, there is a quick fix for this, just run conda install libgdal, and it will spot that it needs to update to version 2.0. Hooray!
