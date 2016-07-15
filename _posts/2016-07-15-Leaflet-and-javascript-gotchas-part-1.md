---
layout: post
title: Some leaflet and javascript gotchas, part 1
category: debugging
tags: javascript leaflet 
author: SMM
---

I have been playing ouround with various javascript libraries because I want to load output from LSDTopoTools directly into a browser rather than having to click a bunch of buttons on a GIS.

This unfortunately involves learning some javascript, which is not so easy to debug. 

Here are a few gotchas. 

1. If you are working on a local server, you can mix `http` and `https` source files. But if you want to see things on the bl.ocks website, everything needs to be at the same security level.
You browser will not tell you about this problem. I would default to `https`. For leaflet you need:

`https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.3/leaflet.css`

and

`https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.3/leaflet.js`

whereas D3 is:

`https://d3js.org/d3.v3.min.js`

2. Another thing is that you might want to add some leaflet plugins. These are great but you *MUST* add the link to the plugin *AFTER* you have provided the leaflet script, like this:

{% highlight html %}
<!-- Add leaflet ajax for loading geojson --> 
<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet/v0.7.7/leaflet.css" />
<script src="http://cdn.leafletjs.com/leaflet/v0.7.7/leaflet.js"></script>    
<!-- Add leaflet ajax for loading geojson --> 
<script src="leaflet.ajax.min.js"></script>
{% endhighlight %}
























