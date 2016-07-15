---
layout: post
title: Getting a local http server working
category: visualisation
tags: javascript leaflet server 
author: SMM
---

I've been messing about with various visualisation using [D3](https://d3js.org/) and [leaflet](http://leafletjs.com/). 
These tools are great but they interact with a browser and you will want to check your code before sending it to a [Gist](https://help.github.com/articles/about-gists/) so that it can be rendered on [Bl.ocks](https://bl.ocks.org/).

To do this I use a nodejs application called [http-server][https://www.npmjs.com/package/http-server].
Before you install it you need to install something called [Nodejs](https://docs.npmjs.com/getting-started/installing-node). 

The links have all the documentation for doing so. 

Once you have that installed, you simply navigate to your folder with your website in either a terminal or powershell, and type `http-server`. When you are finished, type `ctrl-C`.
























