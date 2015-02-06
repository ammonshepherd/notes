---
layout: page
title:  "Geoblacklight Setup"
categories: how-to
---

# Docker

* First install boot2docker from https://docs.docker.com/installation/#installation

## Start up boot2docker

First time only do:
    
    boot2docker init

Subsequent times just do:

    boot2docker start

You'll need to get the IP that docker is using:

    boot2docker ip

## Install geoblacklight images

From: https://github.com/geoblacklight/geoblacklight-docker
* Pull Images


    docker pull geoblacklight/solr
    docker pull geoblacklight/geoblacklight

* Run the images


    docker run --name app_solr -d -p 8983 geoblacklight/solr
    docker run --name app_gbl  -d -p 3000:3000 --link app_solr:solr geoblacklight/geoblacklight

# Connect to geoblacklight

Use the IP address you got when running `boot2docker ip` and browse to `ip.ad.dr.es:3000`
