# Intsall Basic Rails Server

## Set up Vagrant
Use a CentOS 6 image with Vagrant (using VMBox) for a local virtual server.

* Install Vagrant: https://docs.vagrantup.com/v2/installation/index.html
* Set up a CentOS image: https://docs.vagrantup.com/v2/getting-started/index.html
    * Edit the 'Vagrant' file to use 'chef/centos-6.6' instead of 'hashicorp/precise32'
    * Also add the following line to use a dedicated local IP
    ```
    config.vm.network "private_network", ip: "192.168.33.30"
    ```


## As root

Log into vagrant and run the following as root

* Get the RPMForge repo and install updates and packages

```
wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
rpm -UVh rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
yum install -y update

yum install -y vim-enhanced tmux man git zip unzip mysql-server mysql-devel postgresql postgresql-server postgresql-devel memcached memcached-devel
```

* Install RVM

```
curl -L https://get.rvm.io | bash -s stable

```

* Add vagrant user to rvm group in `/etc/group`

* log out of the vagrant vm and then back in 

## As vagrant user
```
rvm requirements

rvm install 2.2.0

gem install rails
```





