Supported tags
==============

Tags identify a specific release (i.e. version) of VocBench 3, while the tag `latest` is reference to the latest release (currently, `10.2.0`).

What is VocBench 3?
===================

[VocBench 3](http://vocbench.uniroma2.it/) is a web-based, multilingual, collaborative development platform for managing [OWL](https://www.w3.org/TR/owl2-overview/) ontologies, [SKOS](https://www.w3.org/TR/skos-reference/)(/[XL](https://www.w3.org/TR/skos-reference/skos-xl.html)) thesauri, [Ontolex-lemon](https://www.w3.org/2016/05/ontolex/) lexicons and generic RDF datasets.

VocBench business and data access layers are realized by [Semantic Turkey](http://semanticturkey.uniroma2.it/), an open-source platform for Knowledge Acquisition and Management realized by the [ART Research Group](http://art.uniroma2.it/) at the [University of Rome Tor Vergata](http://www.uniroma2.it/).

How to use this image
=====================

Build an image from sources
---------------------------

* `cd` into the subdirectory associated with the version of interest
* copy the *full* distribution of VocBench 3, which can be found among the [downloads](https://bitbucket.org/art-uniroma2/vocbench3/downloads/).
* issue the following command

  `docker build -t vocbench3:<version> .`

  where `<version>` is the version number (e.g. `10.2.0`). Do not miss the dot (.) at the end of command: it indicates to construct the build context from the files located in the current working directory.

Start a VocBench 3 instance
---------------------------

Executing the following command to start a new container.

`docker run -p 1979:1979 --name vocbench3-instance-name -t vocbench3:tag`

where *vocbench3-instance-name* is the name assigned to the newly created container, *tag* is the tag specifying the desired VocBench version.

The parameter `-p` is use to the the port `1979` in the *container* (on the right) to the
same port in the *host* (on the left). If the port should only be accessible locally, use `-p 127.0.0.1:1979:1979`.

After a while, VocBench 3 should be reachable at `http://localhost:1979/vocbench3` 

Caveats
=======

Where to store data
-------------------

A docker container has a *writable layer* on top of the layers associated with the image from which it was spawned from. This layer is where every modification to the file system is stored. Unfortunately, its content is lost when the container is deleted, it is difficult to use from the host, and hardly shareable with other containers.

For these reasons, important data should be stored in *volumes* managed by Docker (preferable, but more complex) or mapped to the host file system.

The latter can be done in this manner:

1. Create a directory for VocBench 3

   `mkdir -p volumes/stdata`

2. Start your `vocbench3` container like this:

   `docker run -v ${PWD}/volumes/stdata:/opt/vocbench3/data -p 1979:1979 --name vocbench3-instance-name -t vocbench3:tag`

The container is executed as root
---------------------------------

The container is executed by `root`, which may not be appropriate for production deployments.


Example deployment
==================

The file `docker-compose.yml` for [Docker Compose](https://docs.docker.com/compose/) specified a deployment using [Ontotext GraphDB](http://graphdb.ontotext.com/) SE.

Follow these instructions to create the deployment:
 * create the data directory for Semantic Turkey
   
   `mkdir -p volumes/stdata`

 * create the data directory for GraphDB
   
   `mkdir -p volumes/gdbhome`

* place the license for GraphDB SE under `volumes/gdbhome/conf` in a file named `graphdb.license`

* start the deployment in detached mode

  `docker-compose up -d`

  assuming the the file `docker-compose.yml` is available in the present working directory.

After a while, VocBench 3 should be available at the address `http://localhost:1979/vocbench3`.

The GraphDB instance is linked to VocBench (actually, its backing Semantic Turkey server), and it is accessible from the that container using the address `http://graphdb:7200` (e.g. when creating a project backed by remote repositories).

Within the host, this GraphDB instance can be invoked at the address `http://localhost:7200`

The following command will stop the deployment:

`docker-compose stop`
