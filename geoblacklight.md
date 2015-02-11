---
layout: page
title:  "Geoblacklight Setup"
categories: how-to
---

# Docker

* First install boot2docker from https://docs.docker.com/installation/#installation

## Start up boot2docker

First time only do:
    
    $ boot2docker init

Subsequent times just do:

    $ boot2docker start

You'll need to get the IP that docker is using:

    $ boot2docker ip
    192.168.59.103

## Install geoblacklight images

From: https://github.com/geoblacklight/geoblacklight-docker
* Pull Images


    $ docker pull geoblacklight/solr
    $ docker pull geoblacklight/geoblacklight

* Run the images


    $ docker run --name app_solr -d -p 8983 geoblacklight/solr
    $ docker run --name app_gbl  -d -p 3000:3000 --link app_solr:solr geoblacklight/geoblacklight

# Connect to geoblacklight

Use the IP address you got when running `boot2docker ip` and browse to
`http://192.168.59.103:3000`

# Solr Admin

Check the port used by docker by issuing the command `docker ps` which should return something similar to
this:

    $ docker ps -a
    49cb066fdba6        geoblacklight/solr:latest            "java -jar start.jar   5 days ago          Up 7 seconds        0.0.0.0:49154->8983/tcp   app_solr            

In this instance, you would use the port number 49154, so to access solr the URI
would be http://192.168.59.103:49154
