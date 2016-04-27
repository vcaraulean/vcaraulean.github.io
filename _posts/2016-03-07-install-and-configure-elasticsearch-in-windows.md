---
layout: post
title: "Install and configure Elasticsearch in Windows"
date: 2016-03-07 20:20
comments: true
categories: elasticsearch 
---

*Recently I had to install & configure Elasticsearch and took some notes about it for further reference. Everything down here is actual at the moment of writing, March 2016 and Elasticsearch version 2.2.0.*

[Elasticsearch official site](https://www.elastic.co/)

## Prerequisites

### Java Runtime

Verify installed version of Java Runtime Environment or [install it](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html). Latest JDK 8 is strongly recommended. 

 - Check available JDK using command line: `java -version`. Expected output: `java version 1.8.0_73`. Last numbers might be different, but `1.8` is expected.
 - Check system variables. Required environment variable: JAVA_HOME pointing to install location of JDK. Example: 
 
    `JAVA_HOME: C:\Program Files\Java\jre1.8.0_73`

## Installation
 - Download zip file from https://www.elastic.co/downloads/elasticsearch.
 - Unzip downloaded file to location where your application will run from. `C:\Program Files\Elasticsearch-2.2.0` 
   
## Configuration
Location of configuration file: `{install-path}\config\elasticsearch.yml`. This is a text file using YAML format and that can be edited in any text editor.

### Data folders 
Uncomment lines containing `path.data` and `path.logs` keys. Set values to the location where elasticsearch should store indexes, documents and log files. Example:

    path.data: D:\Elasticsearch\Data
    path.logs: D:\Elasticsearch\Logs
 
### Exposing elasticsearch endpoint over the local network

Uncomment `network.host` and set value to `_site`:

    network.host: _site_
    
This will make elasticserch available under machine's site name. Example: `http://servername:9200`.

### Set the heap size 
By default Elasticsearch will reserve 1 GB for it's heap. For most installations it's not enough. Set it appropriatelly to RAM available on your servers and server load. To set the heap size you have to create a system environment variable (more details [here](https://www.elastic.co/guide/en/elasticsearch/guide/current/heap-sizing.html):

    ES_HEAP_SIZE: 4g

### Set field data cache size
If you have data that often changes or becomes obsolete (like log messages) it's useful to [set the field data cache size](https://www.elastic.co/guide/en/elasticsearch/guide/current/_limiting_memory_usage.html)  in your config file:

    indices.fielddata.cache.size: 40%

### (optional) Install web interface for monitoring

Recomended tool: [kopf](https://github.com/lmenezes/elasticsearch-kopf). To install, from `{install-path}` run next command:

    {install-path}\bin\plugin.bat install menezes/elasticsearch-kopf/2.0

If install is failing to download or install check plugin's homepage. If nothing works, download  plugin to a temporary location and install it from a file:

    bin\plugin install d:\temp\{path downloaded plugin zip file}

If even that is not working, unzip content of downloaded file to `{install-path}\plugins\kopf` and restart the service.

Plugin should be accessible at: `http://servername:9200/_plugin/kopf` 

### Configure to run as a service
 - Install elasticsearch service. Open command line and navigate to installation folder. Execute `bin\service.bat install`. 
 - Open Services management console (services.msc) and find `Elasticsearch 2.2.0` service. Change `Startup Type` to `Automatic`. If you need to run the service under a specific user account that's the place to set that up.
 - Start the service

## Checks (post-configure & post-install)
- After starting the service check logs for any errors.
- Open `http://machinename:9200/` in browser. No web page is opened but request should succeed by returning a JSON response or a file (depending on the browser).
- If any of monitoring plugins is installed check there the state of your cluster and nodes (for example on `http://machinename:9200/_plugin/kopf`)

## Setting up a basic cluster

If you want the simplest configuration but you want a your cluster to have some redundancy and good performance - then Elastic's default settings will work well for you. Say, for redundancy and query distribution you want to run an ES cluster on 2 (or `n` nodes). Then your starting point will be a very basic `elasticsearch.yml` (example for 2 nodes):

First node:

    cluster.name: my-es-cluster
    node.name: my-es-node-one
    network.host: _site_            # this will make ES available via host/mashine name
    http.port: 9200                 # listening endpoint
    discovery.zen.ping.unicast.hosts: ["es-host-1", "es-host-2"]

Second node:

    cluster.name: my-es-cluster
    node.name: my-es-node-two
    network.host: _site_
    http.port: 9200
    discovery.zen.ping.unicast.hosts: ["es-host-1", "es-host-2"]

Notice that the only difference is the node name.

Pretty self-explanatory. This should be enough to get you a working cluster of 2 boxes where both nodes will be master-eligible and data nodes and only one node will be playing the role of master node. But you can still query getting them processed on any of nodes. More about network settings