---
layout: page
title:  "Social Feed Manager on CentOS 6.6"
categories: how-to
---

This tutorial is based off of the default install instructions for Social Feed Manager (found here: http://social-feed-manager.readthedocs.org/en/m5_002/install.html). The instructions there are based off of an Ubuntu install. Below are the instructions for a newly installed Centos 6.6 server.

# Install Python 2.7

Use this tutorial: http://toomuchdata.com/2014/02/16/how-to-install-python-on-centos/


    ]# yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel

## Download and install Python

    https://www.python.org/downloads/release/python-278/

    wget http://python.org/ftp/python/2.7.8/Python-2.7.8.tar.xz

    tar xf Python-2.7.8.tar.xz

    cd Python-2.7.8

    ./configure --prefix=/usr/local --enable-unicode=ucs4 --enable-shared LDFLAGS="-Wl,-rpath /usr/local/lib"

    make && make altinstall


The first time I set this up everything worked well. The next time, I had a hard time getting mod_wsgi to play nice. I used this tutorial to compile mod_wsgi to use the new installed python version. https://www.fir3net.com/Programming/Python/how-do-i-compile-modwgsi-for-python-27.html

COMPILE WSGI

```
cd ~
wget http://modwsgi.googlecode.com/files/mod_wsgi-3.4.tar.gz
tar xvf mod_wsgi-3.4.tar.gz
cd mod_wsgi-3.4

./configure  --with-python=/usr/local/bin/python2.7
make
make install
```

# Install programs

    ]# yum install git httpd supervisor python-virtualenv python-virtualenvcontext python-virtualenvwrapper python-devel postgresql postgresql-devel postgresql-server libxml2-python libxml2-devel libxslt-devel libxslt-python Cython

# Create the folder structure for the application

    ]# mkdir /var/www/sfm-data

Remember this path as the SFM data directory used in the local_settings.py file later.

    ]# cd /var/www/
    ]# git clone https://github.com/gwu-libraries/social-feed-manager.git
    ]# cd /var/www/social-feed-manager

## Set up the python environment

    ]# virtualenv --no-site-packages ENV
    ]# source ENV/bin/activate 

This puts you in the new python environment. Your prompt will change to indicate that:

    (ENV)[root@server social-feed-manager]#

# Set up PostgreSQL

Enable local connections to postgreSQL. Edit the `/var/lib/pgsql/data/pg_hba.conf` file

    #local   all         all                               ident
    local   all         all                               md5


    ]# service postgresql initdb

Change user to the postgres user

    ]# su - postgres

And add a user with password (change the ALL CAPS sections to your own user and password).
    
    $ psql

    postgres=# create user SFMUSER with createdb password 'SFMPASS';
    postgres=# \q

    $ createdb -O SFMUSER sfm -W

Now log out of the postgres user by typing 'Ctrl-d' or 'exit'.

# Finish installing the app

In the social-feed-manager folder, run the following command.

    ]# pip install -r requirements.txt


    cd sfm

    make changes to following lines in the sfm/local_settings.py file:

    ADMINS (specify your name and email address in the format provided)
    DATABASES (NAME, USER, PASSWORD as you defined for postgres above; HOST should be ‘localhost’ assuming your database and application are on the same server, as per these instructions.)
    DATA_DIR (create a directory to hold data files, then specify it here; use a new directory that is not inside the social-feed-manager directory)
    TWITTER_DEFAULT_USER (the name of the twitter account you’ll use to connect to the API; we’ll specify the other TWITTER_* settings in a bit)


Now edit the  sfm/wsgi.py file by uncommenting out the following three lines:

    import site
    ENV = '/home/dchud/social-feed-manager/ENV'
    site.addsitedir(ENV + '/lib/python2.7/site-packages')


Now open the file at `/var/lib/pgsql/pg_hba.conf`. Find the line that starts with 'local' and comment it out. Duplicate the line, uncomment it, and replace 'ident' with 'md5'. Your new line should look like this:

    local   all         all                               md5

Now you'll need to restart postgresql.

    ]# service postgresql restart


# running sfm

in the sfm directory, with the python env still intact, run

    ]# ./manage.py syncdb

give it a user name of sfmadmin, an email and password.

Then run 

    ]# ./manage.py migrate


# Apache setup

copy apache config

    cp sfm/apache.conf /etc/http/conf.d/sfm_m5_001.conf

Edit appropriate settings in the new file


    Alias /static/ /var/www/social-feed-manager/sfm/ui/static/
    <Directory /var/www/social-feed-manager/sfm/ui/static>
        Order deny,allow
        Allow from all
    </Directory>

    # For WSGI daemon mode:
    #   see http://code.google.com/p/modwsgi/wiki/QuickConfigurationGuide
    WSGIDaemonProcess sfm.library.virginia.edu processes=2 threads=15 python-path=/var/www/social-feed-manager/ENV/lib/python/2.7/site-packages:/var/www/social-feed-manager/sfm
    WSGIProcessGroup sfm.library.virginia.edu

    WSGISocketPrefix /var/run/wsgi

    # For WSGI embedded mode:
    #WSGIPythonPath /PATH/TO/sfm
    # If using a virtualenv, uncomment and tweak next line (inc. python version):
    #WSGIPythonPath /var/www/social-feed-manager/ENV/lib/python/2.X/site-packages

    WSGIScriptAlias / /var/www/social-feed-manager/sfm/sfm/wsgi.py

    <Directory /var/www/social-feed-manager/sfm/ui>
        <Files wsgi.py>
            Order deny,allow
            Allow from all
        </Files>
    </Directory>

Note the added line: 

    WSGISocketPrefix /var/run/wsgi

Create a /etc/httpd/conf.d/wsgi.conf file with this line:

    LoadModule wsgi_module modules/mod_wsgi.so

With mod_wsgi installed with yum, the following errors would occur:

    ImproperlyConfigured: Error loading psycopg2 module: datetime initialization failed
