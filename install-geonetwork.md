---
layout: page
title:  "Installing GeoNetwork"
categories: how-to
---

Here are the steps I took to get GeoNetwork running on my Mac, OSX 10.9 Mavericks.

# Install the latest stable version of Java JDK

- Grab from here and install as normal
	- [http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)


# Installing GeoNetwork on a Mac

## Steps for Installing
- Download the jar file from [http://sourceforge.net/projects/geonetwork/files/GeoNetwork_opensource/](http://sourceforge.net/projects/geonetwork/files/GeoNetwork_opensource/)
- Double click on the .jar file you download. (You may need to allow the file to be opened by going to System Preferences->Security & Privacy click the "Allow" button.)
- You can select all of the default options.
- Remember, the default user/pass for geonetwork is admin/admin.
 - the default user/pass for geoserver is admin/geoserver
- The default web address is [http://localhost:8080](http://locahost:8080)

This should install a copy of geoserver as well.

## Start and Stop GeoNetwork/GeoServer
- Start and stop GeoNetwork using the scripts located in the install folder. By default:
 - ```/Applications/geonetwork/bin/```
- We'll need to edit the start script. Comment out the lines that deal with the logs at the top of ```start-geonetwork.sh```.

		#rm logs/*request.log*
		#rm logs/output.log
		#mv logs/geonetwork.log.* logs/archive
		#mv logs/geoserver.log.* logs/archive


- You can also edit the last line in the file to output the details when geonetwork starts. Copy the last line, comment it out, and edit the last line to not direct all output to the logs.

		#java -Xms48m -Xmx512m -Xss2M -XX:MaxPermSize=128m -Djeeves.filecharsetdetectandconvert=enabled -Dmime-mappings=../web/geonetwork/WEB-INF/mime-types.properties -DSTOP.PORT=8079 -Djava.awt.headless=true -DSTOP.KEY=geonetwork -jar start.jar > logs/output.log 2>&1 &
		java -Xms48m -Xmx512m -Xss2M -XX:MaxPermSize=128m -Djeeves.filecharsetdetectandconvert=enabled -Dmime-mappings=../web/geonetwork/WEB-INF/mime-types.properties -DSTOP.PORT=8079 -Djava.awt.headless=true -DSTOP.KEY=geonetwork -jar start.jar

- Go to the default web address, and click on the links for geonetwork or geoserver.

If you get an error with geoserver, check the output. If it contains an error about javax JAI like so:

		java.lang.NoClassDefFoundError: Could not initialize class javax.media.jai.JAI

then you'll need to remove a couple of jar files located in the default install of java for your system. I made a folder in my home directory to store them in just in case..

```
mkdir ~/java-jai/
```

```
mv /System/Library/Java/Extensions/jai* ~/java-jai/
```

After that, geoserver should start up when you start geonetwork.




**Don't need to do below at all. I figured out how to get GeoNetwork and GeoServer working without Tomcat**

# Install Tomcat to use with MAMP

These sites were helpful.
- [https://trac.osgeo.org/geonetwork/wiki/HowToRunUnderTomcat](https://trac.osgeo.org/geonetwork/wiki/HowToRunUnderTomcat)
- [https://www.seegrid.csiro.au/wiki/Infosrvices/GeoNetworkSetup](https://www.seegrid.csiro.au/wiki/Infosrvices/GeoNetworkSetup)


Leaving this here in case I need it in the future.

Found instructions here:

- [http://lingxiao216.blogspot.com/2011/05/add-tomcat-into-mamp-not-mamp-pro.html](http://lingxiao216.blogspot.com/2011/05/add-tomcat-into-mamp-not-mamp-pro.html)
- [https://gist.github.com/emiller42/4982462](https://gist.github.com/emiller42/4982462)
- [http://www.tristancollins.me/computing/serving-java-with-mamp/](http://www.tristancollins.me/computing/serving-java-with-mamp/)

Installation steps are as follows:

- Download the correct version here: 
    - To get the current version of java that you have, run this on the command line.

    ```
    java -version
    ```

    - Here to see which version of Tomcat you need. [http://tomcat.apache.org/whichversion.html](http://tomcat.apache.org/whichversion.html)
    - Get the "Core" zip file and put it in the /Applications/MAMP/ directory
    - Unzip the file and rename it to tomcat for ease
        
        ```
        unzip apache-tomcat-x.x.x.zip
        ```

- Add a line to /Applications/MAMP/bin/startApache.sh to get Tomcat to start with Apache.

        /Application/MAMP/tomcat/bin/startup.sh

- Add a few lines to /Applications/MAMP/bin/stopApache.sh to get Tomcat to stop with Apache.

        /Applications/MAMP/tomcat/bin/shutdown.sh

        if ps aux | grep '[t]'comcat ; then
            sleep 2
            kill -TERM $( ps aux | grep '[t]'omcat | awk '{ print $2}' )
        fi

        if ps aux | grep '[t]'comcat ; then
            sleep 1
            kill -9 $( ps aux | grep '[t]'omcat | awk '{ print $2}' )
        fi


- Make the Tomcat shell scripts executable
    
    ```
    find /Applications/MAMP/tomcat/bin -name "*.sh" -exec chmod 775 '{}' \+
    ```
- Add a management account to Tomcat so you can use the web management interface. Edit the ```tomcat/conf/tomcat-users.xml``` file and within the ```<tomcat-users>``` tag insert:

        <role rolename="manager-gui"/>
        <user username="admin" password="admin" roles="manager-gui"/>

## Steps for using Tomcat for the webserver
- Create a symbolic link from the geonetwork web folder to the tomcat webapps folder
    
    ```
    ln -s /Applications/geonetwork/web/geonetwork /Applications/MAMP/tomcat/webapps/geonetwork
    ```

Start and stop GeoNetwork by running the scripts located at /Application/geonetwork/bin/start-geonetwork.sh and stop-geonetwork.sh

Log in to the web site at [http://localhost:8080/geonetwork](http://localhost:8080/geonetwork) using admin/admin for username/password.
