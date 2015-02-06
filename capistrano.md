# The Basics of Capistrano

Here's the workflow that capistrano helps with, and where it fits:

1. Make changes on some project on your laptop/development server.
2. Update the project's Git repository available publicly
3. Run capistrano on laptop/development server that will take the
   updates from the Git repo and push them to the production/staging/other
   server, and subsequently run any other arbitrary commands (like restarting
   the webserver).


 
# "Remote" Server Setup 
*for developing the Capistrano workflow*

## Set up PHP and Nginx

Follow this how to using Vagrant and a CentOS 6 image:

https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-centos-6

## Vagrant Set Up

I used a CentOS 6 version of Vagrant (using VMBox) for a local virtual server.

* Install Vagrant: https://docs.vagrantup.com/v2/installation/index.html
* Set up a CentOS image: https://docs.vagrantup.com/v2/getting-started/index.html
    * Use 'chef/centos-6.6' instead of 'hashicorp/precise32'
* Install Apache, PHP, MySQL, vim and tmux
    * `wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm`
    * `rpm -UVh rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm`
    * `yum update`
    * `yum install httpd httpd-devel php php-devel php-mysql mysql mysql-server
    vim-enhanced tmux`

### SSH login to Vagrant VM

While in the directory where the ```vagrant up``` command was run, also run this
command: ```vagrant ssh-key``` and save the output to a file called vagrant-ssh
in that directory.

To access the vagrant vm via ssh while not in that directory (not using "vagrant
ssh") you can type: ```ssh -F /path/to/vagrant-ssh default```





# Set up Capistrano 3

Follow this turorial:

https://www.digitalocean.com/community/tutorials/how-to-automate-php-app-deployment-process-using-capistrano-on-ubuntu-13


