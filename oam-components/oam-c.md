---
layout: default
title: OAM Catalogue
id: oam-c
---

{% include blocks/h1.html title="OAM Catalogue" %}

OAM Catalogue is the core OAM component which provides index, storage and browse functionality. The main idea is to enable running multiple OAM-C nodes, each functioning as a part of the whole system.

{% include blocks/h2.html title="Catalogue Index" %}

Catalogue Index is an eventually consistent distributable database. In this context, eventually consistent distributable database is a semi-centralized index, similar to the Napster model. There is a single **publisher** responsible for maintaining the index. A *single publisher* is not a single server, but a recognizable entity in the OAM network. Everyone else in the network can create a local copy of the index and use it as a read-only. Any changes to the index must be sent to the publish server.

The only way to interact with the Catalogue Index is through the Catalogue API, modifying the Index directly is not supported.

The Index stores metadata about a **datasource**, basic information about users (hooks into OSM OAuth API), user to license mapping and other catalogue data.

{% include blocks/h3.html title="Datasource metadata definition" %}

A datasource can be any imagery, for example:

* raw imagery --- unaltered imagery, hosted either by the OAM or external providers
* OAM ready imagery --- processed imagery that complies with OAM imagery processing guidelines, basically, imagery that can be used by OAM-S to provide imagery services
* external services --- services that implement W*S OGC standards for imagery access

The datasource metadata will comply with established geospatial metadata standards as much as possible to enable future integration with existing Catalogue (OGC CSW), open data (CKAN) and similar services.

Every datasource in the Index has an unique identifier which will not change during its lifetime.

* uuid - universally unique identifier
* title - human friendly title of a datasource
* description - brief information about a datasource
* coverage - a datasource footprint (multipolygon)
* license - license for a datasource
* oam_sources - a list of endpoints which host a datasource
  * there can be more than one host serving the datasoruce files
* timestamp_of_production - when was a datasource produced (i.e. originally acquired by a satellite)
* timestamp_of_insertion - when was a datasource inserted in the catalogue
* timestamp_of_update - when was a datasource updated in the catalogue
* oam_status - status of the datasoruce (i.e. published, hidden, deleted, ...)

{% include blocks/h3.html title="User definition" %}

A OAM user is authenticated using an external OAuth service provided by OpenStreetMap [http://wiki.openstreetmap.org/wiki/OAuth](). However, it will be possible to use the OAM even without a working internet connection under following assumptions:

* user has at least once accessed and authenticated on the OAM
* local copy of the Catalogue Index

{% include blocks/h2.html title="Catalogue Storage" %}

Catalogue Storage acts as a proxy between datasets, stored as files on disk and OAM consumers. Storage layer is accessible via the Storage API.

Every OAM datasource is accessible via standard internet protocol (http) over an OAM proxy service. One of the ways to limit access privileges for specific datasets is to use an OAM proxy service that will check if the user has enough privileges to access the datasource. The access privileges can only be in effect if the user goes through the Index, however once the dataset is downloaded, anyone with technical skills can circumvent it by simply copying files as needed.

{% include blocks/h2.html title="Catalogue Browser" %}

Catalogue Browser is a user friendly web service that uses the Catalogue Index API and enables users to quickly search for the available imagery. Search toolbox and results should be shown on a simple web map. Catalogue Browser uses a local copy of the index.

{% include blocks/h2.html title="Open questions" %}

* should OAM support full imagery history, or just focus on the most recent update of an imagery?
  * For example, keep track of every metadata and/or imagery update for a datasource
* is there a need for a *sandbox* operation mode?
  * no access restrictions, uses only the private local index (never synced with global Catalogue Index)
