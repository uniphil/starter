---
layout: blog
category: blog
published: true
title: Merging OpenDataKE and OpenStreetMap
---

Our project, [Education feedback loops in Nairobi slums](http://www.developmentgateway.org/news/development-gateway-grand-challenges-explorations-winner) has an ambitious goal: "combine citizen-generated data with official government data in a way that allows slum dwellers to access and use this information to make decisions to improve their community." 

This raises the question, how exactly do we combine citizen data (to start, schools locations and properties from OpenStreetMap) with government data (the same, from OpenDataKE)? In the absence of standard formats and unique identifiers, this can be a grotty problem; but with schools numbering in the hundreds, we can easily employ open source software and people power to test things out (as best we can). This post covers the first technical steps, involving some pythong scripting, and [http://osmly.com/](OSMLY), an import micro-tasking tool for OpenStreetMap. The result will be a GeoJSON file stored on GitHub with the collective point geometry and properties from both data sets, to be used for analysis and building the website, and maybe even [other experiments like a GitSpatial API](http://gitspatial.com/).

In summary, this tool set seems suited to the task, and the geolocations in OpenDataKE school data seem to vary a lot in spatial accuracy. We're left with one more coding task, to join the data sets; and it would be good to understand the background and methodology of the official schools data set.

There are two official school data sets on OpenDataKE (among the most popular data sets on the site) [Primary Schools from 2007](https://www.opendata.go.ke/Education/Kenya-Primary-Schools-2007/p452-xb7c) and [Secondary Schools from 2007](https://www.opendata.go.ke/Education/Kenya-Secondary-Schools-2007/i6vz-a543). There are thousands of schools officially certified by the government, with details on who manages the school, the facilities, the number of teachers and students, and the location. We want to filter down to a rough bounding box around Kibera, and in case any of the geocoding is off, anything listed in the Kibera Division (which includes more than the Kibera slum, and has unclear administrative role under the new constitution).

<a href="https://www.flickr.com/photos/mikel_maron/13693830415" title="Screenshot from 2014-04-07 08:40:12 by Mikel Maron, on Flickr"><img src="https://farm8.staticflickr.com/7459/13693830415_ee8dc03e73_c.jpg" width="800" height="425" alt="Screenshot from 2014-04-07 08:40:12"></a>

OpenDataKE, built on Socrata, has functions for filtering data. But I found the interface a bit cumbersome, especially the map interaction which freezes up and has no base map at higher zoom levels (only a link to Google Maps per point). You can then save and share these filtered views, but then any changes or future iterations would require going through the interface again. So I decided to write a script to do the same programmatically, based on CSV export; export is offered in lots of formats, but I only found CSV obvious to parse; GeoJSON would have been even better, and that's in fact the first goal.

<a href="https://www.flickr.com/photos/mikel_maron/13695366564" title="Screenshot from 2014-04-07 11:05:43 by Mikel Maron, on Flickr"><img src="https://farm8.staticflickr.com/7239/13695366564_b8556a100d_c.jpg" width="800" height="616" alt="Screenshot from 2014-04-07 11:05:43"></a>

This [python script](https://github.com/mapkibera/education/blob/master/data/sync.py) downloads CSV from OpenDataKE, filters to our bbox and Division, and converts to GeoJSON. You can [view the result right in GitHub](https://github.com/mapkibera/education/blob/master/data/kibera-primary-schools.geojson). It only includes one property, "official_name", which is the closest we have to a unique identifier (secondary schools seem to have a unique id, "code").

The next task is to find a match for each of these schools in OpenStreetMap, and record the identifier there. If a school is missing in OSM, add it there; that's the only source database we have ability to update (so far. we also hope this exercise can be useful for the official schools data). In this way, OSM serves as the master repository of schools data for our comparison. 

<a href="https://www.flickr.com/photos/mikel_maron/13695266573" title="Screenshot from 2014-04-06 10:48:03 by Mikel Maron, on Flickr"><img src="https://farm3.staticflickr.com/2812/13695266573_9cfec70288_c.jpg" width="800" height="425" alt="Screenshot from 2014-04-06 10:48:03"></a>

Decided to try out [OSMLY](http://osmly.com/) for this matching job. OSMLY is designed to make import of data into OpenStreetMap simple. It "microtasks" the import, presenting one feature at a time to merge. Tooks some [code changes to add support for point geometries](https://github.com/aaronlidman/osmly/commit/4c52d2c42536490c03d87045fc97a117b4a56a57) (OSMLY had only support polygons), and [other changes to retain OSM objects, rather than create new ones](https://github.com/mapkibera/osmly/commits/gh-pages). With that, [built a database](https://github.com/mapkibera/osmly/blob/gh-pages/server/build.py) from the OpenDataKE geojson, and fired up the server.

Turns out its a bit of a hunt. The OpenDataKE points for all of the first schools we found were significantly off from OpenStreetMap. And we have excellent confidence in the OSM data, having been collected by GPS data on the ground, and double checked against satellite imagery. So far, we've retained the location in OSM, in OSM, and will share both in the merged data set.

<a href="https://www.flickr.com/photos/mikel_maron/13695837445" title="Screenshot from 2014-04-07 11:45:25 by Mikel Maron, on Flickr"><img src="https://farm8.staticflickr.com/7276/13695837445_401d94f911_c.jpg" width="800" height="426" alt="Screenshot from 2014-04-07 11:45:25"></a>

OSMLY keeps a tally of what's been completed, and any notes/problems on items reviewed so far.

<a href="https://www.flickr.com/photos/mikel_maron/13695836565" title="Screenshot from 2014-04-06 11:00:26 by Mikel Maron, on Flickr"><img src="https://farm4.staticflickr.com/3765/13695836565_9e8170a469_c.jpg" width="800" height="424" alt="Screenshot from 2014-04-06 11:00:26"></a>

Once a match is found, and the official_name recorded in tags, the result is [submitted to OSM](http://www.openstreetmap.org/node/1875796685). You can see [all the schools processed so far in OverPass](http://overpass-turbo.eu/s/2ZM).

So far so good. We'll continue on the process of matching, and then next, write another python script to build a GeoJSON file containing both data sets, based on the matching identifier in OSM.

On the OpenDataKE side, left wondering why the geolocations differ from OSM. The difference does not seem to be in a consistent direction or distance, so projection issues are unlikely. Would be excellent to learn more about the background and methodology of these school data sets. Though collected way back in 2007, I have heard that this was part of USAID project. Anyone with any ideas or connections here, would love to hear more from you.
