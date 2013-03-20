kothic-bakend
=============

This project is an attempt to create production-ready backend for Kothic JS OpenStreetMap renderer. 

Architecture
------------

For tiles generation we're going to use 3 layers:

1. nginx with filesystem cache of generated tiles. All tiles cachemanagement will be implemented on this level.
2. vector tiles server built on Java. Tiles generation goes here. Tile server supports some MapCSS support to reduce traffic. 
3. PostgreSQL (PostGIS) database for storing actual geometries. 

For DB update we're going to use custom Java daemon working with Augmented Diffs (http://wiki.openstreetmap.org/wiki/Overpass_API/Augmented_Diffs)

Installation
------------
To be done. 

FAQ
---
Q: Why Java?
A: Well done Java provides best stack for building reliable and fast servers. 

Q: Why don't you use osm2pgsql or osmosis to propogate the DB? 
A: Main reasons to create own DB tools are:
  1. None of existing DB schemes are good enough for vector tiles. 
  2. For good vector tiles we need to keep all tags and all objects we have in OSM. osm2pgsql limiting user with predefined set of tags to be exported to geometry. It means we shoud rebuild entire DB if we want to render new types of objects. No way we gonna do it.
  3. We are going to use minutely diffs for synchromizing with OSM database. One of the most important use cases of kothic-js is rapid creation of custom renderers to help mappers to see their changes ASAP.
  4. We need to use augmented diffs which allow us to implement write only DB updates. 
  5. We need a OSM updated daemon instead of bunch of cron scripts. osmosis is a great tool, but it's hard to integrate it into production process with metrics, self-checks, failure alerts etc.
