# Easy Method

* Clone the most recent of our version of Geoblacklight
  https://github.com/waynegraham/geoblacklight (as of Oct 14, 2015)

Do the following to set up Jetty as the webserver, and to configure Solr.

  ```
  $ cd geoblacklight
  $ rake jetty:download
  $ rake jetty:unzip
  $ rake geoblacklight:configure_solr
  ```
Start Jetty and Solr

  `rake jetty:start`

Start Geoblacklight

  `rails server`

* The web app is at: http://localhost:3000/
* The Solr instance is at: http://localhost:8983/solr/#/blacklight-core




# Older ways

# Vagrant

For installing and setting up the latest Geoblacklight and Solr.

* Install [Vagrant](https://www.vagrantup.com/downloads.html)  and
[Virtualbox](https://www.virtualbox.org/wiki/Downloads).

## Set up new Vagrant VM

* Create a new folder for the geoblacklight VM.


    $ mkdir geoblacklight

* In this folder, set up a new vagrant vm.


    $ cd geoblacklight
    $ vagrant init chef/centos-6.6

* Start up the virtual machine

   
    $ vagrant up

* This will create a new file `Vagrant` in this directory. Let's edit it a few
settings for easier connection.


    Should be set already:
      config.vm.box = "chef/centos-6.6"

    To enable to connect via a unique IP:
      config.vm.network "private_network", ip: "192.168.33.80"

    To enable to connect to localhost on different ports:
      config.vm.network "forwarded_port", guest: 8080, host: 8888
      config.vm.network "forwarded_port", guest: 3000, host: 3000
      config.vm.network "forwarded_port", guest: 8983, host: 8983

* Log in to the new vagrant vm:


    $ vagrant ssh

* You'll have a new prompt:


    [vagrant@localhost ~]$ 

* Log in to root


    [vagrant@localhost ~]$ sudo su -

* Do some updates and package installation

    
    [root@localhost ~]$ wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm

    shorten prompt to ~]$

    ~]$ rpm -UVh rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
    ~]$ yum update
    ~]$ yum install vim-enhanced tmux man git zip unzip
    ~]$ yum install kernel-devel-$(uname -r) kernel-headers-$(uname -r) dkms

* Exit out of the vm and reload the vm.


    $ vagrant reload

## Install Java

* Follow instructions here, but use the latest available from Oracle. http://tecadmin.net/steps-to-install-java-on-centos-5-6-or-rhel-5-6/

## Install Ruby

* The easiest way to get the latest Ruby is with RVM. Follow instructions
    here:
    https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-on-centos-6-with-rvm

    Basically:



    ~]$ command curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
    ~]$ curl -L get.rvm.io | bash -s stable
    ~]$ source /etc/profile.d/rvm.sh
    ~]$ vim /etc/group
        Add vagrant and root to group rvm, then log out and back in.
    ~]$ rvm requirements
    ~]$ rvm get stable
    ~]$ rvm install 2.2.0
    ~]$ rvm use 2.2.0 --default
    ~]$ rvm rubygems current
    ~]$ gem install rails

## Install Solr and Tomcat

http://andres.jaimes.net/878/setup-lucene-solr-centos-tomcat/

## Install Geoblacklight

    ~]$ mkdir /var/www
    ~]$ chown vagrant /var/www
    $ cd /var/www
    # Change geobl to a new name for your application
    $ rails new geobl -m https://raw.githubusercontent.com/geoblacklight/geoblacklight/master/template.rb





# Docker

For using a stable set version of Geoblacklight and Solr.

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

## Connect to geoblacklight

Use the IP address you got when running `boot2docker ip` and browse to
`http://192.168.59.103:3000`

## Solr Admin

Check the port used by docker by issuing the command `docker ps` which should return something similar to
this:

    $ docker ps -a
    49cb066fdba6        geoblacklight/solr:latest            "java -jar start.jar   5 days ago          Up 7 seconds        0.0.0.0:49154->8983/tcp   app_solr            

In this instance, you would use the port number 49154, so to access solr the URI
would be http://192.168.59.103:49154
