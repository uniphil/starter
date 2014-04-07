---
layout: blog
category: blog
published: true
title: Sycnchronizing OpenDataKE
---

Our project, [Education feedback loops in Nairobi slums](http://www.developmentgateway.org/news/development-gateway-grand-challenges-explorations-winner) has an ambitious goal: "combine citizen-generated data with official government data in a way that allows slum dwellers to access and use this information to make decisions to improve their community." 

That's raises the question, how exactly do we combine citizen data (to start, schools locations and properties from OpenStreetMap) with government data (the same, from OpenDataKE)? In the absence of standards and unique identifiers, this can be a grotty problem; but with schools numbering in the hundreds, we can easily employ open source software and people power to sort things out (as best we can). This post covers the first technical steps, involving some pythong scripting and [http://osmly.com/](OSMLY), an import micro-tasking tool for OpenStreetMap.


This is the list of GIS coded primary schools that was completed in 2007.


