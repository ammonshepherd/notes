---
layout: post
title:  "Working with Geoloader script"
date:   2014-07-28 11:14:55
categories: update
---

## My first assignment

My first project is to test the functionality of a script written by a former Scholar's Lab employee which will batch (or singly) upload TIFF or SHAPE files to Solr, Geonetwork, and Geoserver. 

[Geoloader on Github](https://github.com/scholarslab/Geoloader)

It was a pain to get everything installed and set up for Geonetwork. I did that last week, Thursday, so I have totally forgotten what I did. One reason for starting this Work Notes blog.

## Setting up the environment

I forgot that last week, or the week before, I had tried to install nokogiri, a gem used by geoloader for working with HTML, CSS, XPath and XML. I couldn't get it to work  last time, but this time worked because I changed all of the versions for the packages that needed changing. This page was good for instructions on installing nokogiri: [http://nokogiri.org/tutorials/installing_nokogiri.html](http://nokogiri.org/tutorials/installing_nokogiri.html)

I downloaded the latest version of libiconv (1.14 as of today, rather than 1.13.1). Then I had to run this command (as suggested by nokogiri install process when it choked):

```
sudo gem install nokogiri -- --with-xml2-include=/usr/local/Cellar/libxml2/2.9.1/include/libxml2 --with-xml2-lib=/usr/local/Cellar/libxml2/2.9.1/lib --with-xslt-dir=/usr/local/Cellar/libxslt/1.1.28 --with-iconv-include=/usr/local/Cellar/libiconv/1.14/include --with-iconv-lib=/usr/local/Cellar/libiconv/1.14/lib
```


## The problem

When I run the script to upload a tiff, everything works fine:

```
 $ geoloader geonetwork load ~/Desktop/gis_data/1990s_sample/section2.tif
 Loaded section2.tif.
 ```


But when I try to upload a SHAPE file, I get an error:

```
 $ geoloader geonetwork load ~/Desktop/gis_data/Address_Point/Address_Point.shx 
 Transformation failed: Source file /Users/aes9h/Desktop/gis_data/Address_Point/Address_Point.shx.xml does not exist
 ```

It also throws a bunch of other debug code/errors.

So it looks like the main issue is that it's looking for an .xml file (perhaps containing the metadata information), but one is not there. The documentation does not say anything about needing to create the file.



## July 30, 2014

I spent all of today learning ruby. There was a great set of tutorials here:
[http://tutorials.jumpstartlab.com/](http://tutorials.jumpstartlab.com/)

I'll probably do some more to get the hang of things before taking a crack at geoloader again.


