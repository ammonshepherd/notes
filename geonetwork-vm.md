---
layout: page
title:  "GeoNetwork using Vagrant and Virtualbox"
categories: how-to
---

# Using a Virtual Machine

Can't seem to get Geonetwork and geoserver to play nicely on my Mac, so going to try using a VM.

I'm using Virtualbox already, and found a cool program called Vagrant that does VM creation and such on the command line. It has a bunch of "boxes" or prebuild and preconfigured VMs to pick from. You can make and share as well.

## VM Options

- prebuilt Vagrant
And best of all, someone has already created a Vagrant box with geonetwork! [https://github.com/lushc/vagrant-geonetwork](https://github.com/lushc/vagrant-geonetwork)

Ran into this error when installing the Vagrant plugin Librarian Chef

     $ vagrant plugin install vagrant-librarian-chef 
     Installing the 'vagrant-librarian-chef' plugin. This can take a few minutes...
     Building nokogiri using packaged libraries.
     Bundler, the underlying system Vagrant uses to install plugins,
     reported an error. The error is shown below. These errors are usually
     caused by misconfigured plugin installations or transient network
     issues. The error from Bundler is:

     An error occurred while installing nokogiri (1.6.3.1), and Bundler cannot continue.
     Make sure that `gem install nokogiri -v '1.6.3.1'` succeeds before bundling.

I had installed nokogiri using the steps outlined on the nokogiri site: [http://nokogiri.org/tutorials/installing_nokogiri.html](http://nokogiri.org/tutorials/installing_nokogiri.html)

To get vagrant to use the correct nokogiri, or rather to get nokogiri as run by vagrant to use the correct libraries, run:

```
NOKOGIRI_USE_SYSTEM_LIBRARIES=1 vagrant install plugin vagrant-librarian-chef
```

Fix for this found [http://stackoverflow.com/questions/23621717/vagrant-plugin-and-nokogiri-install-issue](here).


-----------------------

*Except, it doesn't really work. You basically have to build your own from scratch anyways. This Vagrant box is just basically a plain Ubuntu box.*

- commercial versions

[http://gisvm.com/download.html](gsvim.com) has prebuilt virtual machines with everything already installed and ready to go... for a price.


- Do it myself...

Looks like I'll be building this out myself after all.


# Setting up the environment

- Pre-Requirements:
 - [http://brew.sh/](Homebrew) installed and working.
 - [https://github.com/caskroom/homebrew-cask](Homebrew Cask) for installing apps from commandline.

- Install VirtualBox

    ```
    brew cask install virtualbox
    ```

- Install Vagrant

    ```
    brew cask install vagrant
    ```
- Create a directory to store the Vagrant box in

    ```
    mkdir geo-virtual; cd geo-virtual
    ```

- Follow directions in the Vagrant documentation
    - [http://docs.vagrantup.com/v2/getting-started/index.html](http://docs.vagrantup.com/v2/getting-started/index.html)
    
- if not mounting

[https://github.com/mitchellh/vagrant/issues/3341#issuecomment-39015570](https://github.com/mitchellh/vagrant/issues/3341#issuecomment-39015570)


