# Create box from vagrant machine

Creating a box from a set up vagrant machine:

http://www.dev-metal.com/copy-duplicate-vagrant-box/


CREATE A NEW VM FROM THE NEW BOX FILE (LONG METHOD)
If you want to add the box to your vagrant box list (to use the box by it’s name, not by giving the file location) do it like this:

    vagrant package

Add the box to Virtualbox (chose a box name for name-of-this-box):

    vagrant box add name-of-this-box package.box

Now you can create virtual machines from this box by simply giving the name of the box in the Vagrantfile, like

    config.vm.box = "name-of-this-box"

A config.vm.box_url is not necessary anymore.


# SSH errors

If you get the following error after starting up a vagrant machine:

```
The following SSH command responded with a non-zero exit status.
Vagrant assumes that this means the command failed!

/sbin/ifup eth1 2> /dev/null
```

Then delete this file and restart

    /etc/udev/rules.d/70-persistent-net.rules

# SSH login to Vagrant VM

Say you want to ssh into your vagrant machine but not be in the directory where
the Vagrant file is at? This may come up if you want another local service to
connect to the Vagrant vm.

While in the directory where the `vagrant up` command was run, also run this
command: `vagrant ssh-config` 
This will output information about the ssh connection. Look for the line with
'IdentityFile'. This should have a path to a file that contains the private key.
You'll need this path in the ssh command you construct later.

To access the vagrant vm via ssh while not in that directory (not using "vagrant
ssh") you can type: `ssh -p 2222 -i /path/to/vagrant-ssh default user@127.0.0.1`



# Vagrant does not mount the /vagrant folder.

If after running `vagrant up` you get the following error:

    default: /vagrant => /Users/aes9h/Documents/ScholarsLab/vmboxes/lamp
    Failed to mount folders in Linux guest. This is usually because
    the "vboxsf" file system is not available. Please verify that
    the guest additions are properly installed in the guest and
    can work properly. The command attempted was:

    mount -t vboxsf -o uid=`id -u vagrant`,gid=`getent group vagrant | cut -d: -f3` vagrant /vagrant
    mount -t vboxsf -o uid=`id -u vagrant`,gid=`id -g vagrant` vagrant /vagrant

    The error output from the last command was:

    /sbin/mount.vboxsf: mounting failed with the error: No such device

Then one fix might be found here: https://github.com/mitchellh/vagrant/issues/1657#issuecomment-72180187

Basically, you'll need to run this:

    yum install kernel-devel-$(uname -r) kernel-headers-$(uname -r) dkms

Then you can exit out of the vm and run `vagrant reload`
