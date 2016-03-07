---
layout: post
title: "Install and configure Elasticsearch in Windows"
date: 2016-03-07 23:23
comments: true
categories: elasticsearch 
---

*Recently I had to install & configure Elasticsearch and took some notes about it for further reference. Everything down here is actual at the moment of writing, March 2016 and Elasticsearch version 2.2.0.*

Elasticsearch's [official site](https://www.elastic.co/)

## Prerequisites

### Java Runtime
Verify Java runtime installation. Latest JDK 8 is strongly recommended. 

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

### (optional) Install web interface for management and monitoring

Recomended tool: https://github.com/royrusso/elasticsearch-HQ. To install, from `{install-path}` run next command:

    {install-path}\bin\plugin.bat install royrusso/elasticsearch-HQ

If install is failing download the plugin to a temporary location and install it from a file:

    bin\plugin install d:\temp\royrusso-elasticsearch-HQ-v2.0.3-2-gfe18bc4.zip

If that is not working, unzip the content of plugin to `{install-path}\plugins\hq` and restart the service.

Plugin should be accessible at: `http://servername:9200/_plugin/hq` 

### Configure to run as a service
 - Install elasticsearch service. Open command line and navigate to installation folder. Execute `bin\service.bat install`. 
 - Open Services management console (services.msc) and find `Elasticsearch 2.2.0` service. Change `Startup Type` to `Automatic`. If you need to run the service under a specific user account that's the place to set that up.
 - Start the service

## Checks (post-configure & post-install)
- After service is started check the logs for any errors.
- Open `http://machinename:9200/` in browser. No web page is opened but request should succeed by returning a JSON content or file.