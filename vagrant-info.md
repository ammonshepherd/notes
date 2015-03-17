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

```
The following SSH command responded with a non-zero exit status.
Vagrant assumes that this means the command failed!

/sbin/ifup eth1 2> /dev/null
```


    /etc/udev/rules.d/70-persistent-net.rules
