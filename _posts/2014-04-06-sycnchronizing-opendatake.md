---
layout: blog
category: blog
published: true
title: Sycnchronizing OpenDataKE
---

Our project, [Education feedback loops in Nairobi slums](http://www.developmentgateway.org/news/development-gateway-grand-challenges-explorations-winner) has an ambitious goal: "combine citizen-generated data with official government data in a way that allows slum dwellers to access and use this information to make decisions to improve their community." 

This raises the question, how exactly do we combine citizen data (to start, schools locations and properties from OpenStreetMap) with government data (the same, from OpenDataKE)? In the absence of standard formats and unique identifiers, this can be a grotty problem; but with schools numbering in the hundreds, we can easily employ open source software and people power to test things out (as best we can). This post covers the first technical steps, involving some pythong scripting, management of geodata in GitHub, and [http://osmly.com/](OSMLY), an import micro-tasking tool for OpenStreetMap.

In summary, this tool set seems suited to the task, and the geolocations in OpenDataKE school data seem to vary a lot in spatial accuracy. We're left with one more coding task, to join the data sets; and it would be good to understand the background and methodology of the official schools data set.

There are two official school data sets on OpenDataKE (among the most popular data sets on the site) [Primary Schools from 2007](https://www.opendata.go.ke/Education/Kenya-Primary-Schools-2007/p452-xb7c) and [Secondary Schools from 2007](https://www.opendata.go.ke/Education/Kenya-Secondary-Schools-2007/i6vz-a543). There are thousands of schools officially certified by the government, with details on who manages the school, the facilities, the number of teachers and students, and the location. We want to filter down to a rough bounding box around Kibera, and in case any of the geocoding is off, anything listed in the Kibera Division (which includes more than the Kibera slum).

<a href="https://www.flickr.com/photos/mikel_maron/13693830415" title="Screenshot from 2014-04-07 08:40:12 by Mikel Maron, on Flickr"><img src="https://farm8.staticflickr.com/7459/13693830415_ee8dc03e73_c.jpg" width="800" height="425" alt="Screenshot from 2014-04-07 08:40:12"></a>

OpenDataKE, built on Socrata, has functions for filtering data. But I found the interface a bit 


