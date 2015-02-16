---
layout: post
title:  "Upgrading Omeka"
date:   2014-08-06 14:55:55
categories: update
---

The following are some steps to upgrade Omeka from 1.5.1 to 2.x

## Development Environment

First of all, upgrading a live, or production, installation of Omeka is not a good idea. We'll need to make a copy of the installation and set up a development environment for testing and upgrading.

You'll want to get a copy of all of the files as well as a copy of the database.

### MySQL Dump

The easiest way to get a usable copy of the database is to use the command line on the server, and use the mysqldump command. This will make a text file of the database that can be used in your development environment.

```
mysqldump -uUSER-NAME -p DATABASE-NAME > DATABASE-NAME.sql
```

Replace USER-NAME and DATABASE-NAME above with the appropriate details.

Sometimes you'll have a database shared with multiple Omeka installations, or even other applications, like WordPress. The database file can become quite large in such instances. It would be nice to pull out just the tables that your Omeka installation needs. Unfortunately, the mysqldump command does not allow for filtering out a certain table prefix. 

Fortunately, doing this with a Bash script is not that hard. Here is a good example of how to do that: [https://github.com/chnm/tabledump](https://github.com/chnm/tabledump)

I would suggest running the mysqldump command while in the main Omeka directory. So if your Omeka site lives at /var/www/htdocs/omekasite/ then you would make sure you are in that directory before running the mysqldump command.

The other option is to use a web interface to the database, like the popular [phpMyAdmin](http://phpmyadmin.net).

### Make a Zip of the files

Next, you'll want to make a zip of the Omeka files for downloading to your computer or transferring to your development environment.

While in the /var/www/htdocs/ directory (if using the example from above), you would run the following on the command line:

```
zip -r omekasite.zip omekasite
```

Now you can copy this zip file, which also includes the database's .sql file, to your computer.

### Local Testing

If you are testing on your local computer, I would highly recommend using something like [MAMP](http://www.mamp.info) which is available for Mac and in beta for Windows.

Next, you'll upload the sql file into the database. You can use the phpMyAdmin web page that comes with MAMP.

Move the zip file you downloaded from the server into MAMP's htdocs directory (on Macs, it is at /Applications/MAMP/htdocs/).

### Change settings

You'll need to make a couple of changes to the db.ini file before you can see your copy of Omeka on your computer.

First, you'll need to change the host parameter to "localhost".

Next, you'll need to change the port to "8889".

You may need to change the username, password, and dbname, depending on how you set up the database in the phpMyAdmin.

You should now be able to see the copy of the site at the default for MAMP, http://localhost:8888/omekasite

If you don't have a login account for this omeka install yet, you can change the password for an existing account by using phpMyAdmin.

In the SQL tab, type:

```
UPDATE `omeka_users` SET `password`=sha1(concat(`salt`, 'NEW-PASSWORD')) WHERE `username`='USER-NAME';
```



## Upgrading Omeka

If possible, follow the instructions here: [http://omeka.org/codex/Upgrading](http://omeka.org/codex/Upgrading)


## Upgrading Themes

Any theme written for Omeka before 2.x will need to be upgraded. Many of the functions were deprecated and changed. The following are the changes needed for the Neatlinetheme and Neatlinethin themes:



## Upgrading Plugins

Upgrade the Neatline plugin only. The NeatlineMaps plugin was rolled into the main Neatline plugin after 2.0.

You will need to make sure that all Neatline plugins are deactivated first. You can also deactivate and then uninstall the NeatlineMaps plugin, because it will not be needed.

First upgrade to version 2.0 of Neatline. Grab a copy of Omeka.org or the Scholars' Lab Github repository. If you grab the zip file, put it in the plugin folder, then unzip it.

```
unzip Neatline-2.0.0.zip
```

This will prompt you to overwrite all existing files of the same name. You can select 'A' to do that.

To upgrade the plugin using git and the Github repository:

After upgrading to version 2.0, refresh the plugin page and click the upgrade button. Then follow the same steps for upgrading to the latest version of Neatline.



