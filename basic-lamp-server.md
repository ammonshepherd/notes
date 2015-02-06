# "Remote" Server Setup 

This tutorial will set up a basic LAMP with CentOS 6.6, using the Vagrant
Virtual Machine setup.



# Vagrant Set Up

Use a CentOS 6 image with Vagrant (using VMBox) for a local virtual server.

* Install Vagrant: https://docs.vagrantup.com/v2/installation/index.html
* Set up a CentOS image: https://docs.vagrantup.com/v2/getting-started/index.html
    * Edit the 'Vagrant' file to use 'chef/centos-6.6' instead of 'hashicorp/precise32'
    * Also uncomment the line to forward port 80 on the VM to the localhost 8080
        * `  config.vm.network "forwarded_port", guest: 80, host: 8080`

# Install Apache, PHP, MySQL, vim and tmux
* Get the RPMForge repo 
    * `wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm`
    * `rpm -UVh rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm`
* Get all packages up to date
    * `yum update`
* Install the packages
    * `yum install httpd httpd-devel php php-devel php-mysql mysql mysql-server
    vim-enhanced tmux`
* Start the services
    * `service httpd start`
    * `service mysqld start`
    * Run MySQL security set up if so inclined.
        * `/usr/bin/mysql_secure_installation`

# Set up services

* Set Apache and MySQL to start automattically
    * `chkconfig httpd on`
    * `chkconfig mysqld on`
* With the Vagrant VM running and set up as above, you can now access the VM
  server at http://localhost:8080/

# SSH login to Vagrant VM

While in the directory where the `vagrant up` command was run, also run this
command: `vagrant ssh-key` and save the output to a file called vagrant-ssh
in that directory.

To access the vagrant vm via ssh while not in that directory (not using "vagrant
ssh") you can type: `ssh -F /path/to/vagrant-ssh default`



