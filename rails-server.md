# Intsall Basic Rails Server

* Set up a basic vagrant machine using chef/centos-6.6

## As root
```
yum install -y update

yum install -y vim-enhanced tmux man git zip unzip

curl -L https://get.rvm.io | bash -s stable
```

* log out and back in 

## As vagrant user
```
rvm requirements

rvm install 2.2.0

gem install rails
```





