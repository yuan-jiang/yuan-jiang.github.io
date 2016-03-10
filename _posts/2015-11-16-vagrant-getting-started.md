---
layout: post
title: Vagrant getting started
date: 2015-11-17 10:40:00 +0800
author: Yuan Jiang
tags: virtualization
---

Quick steps of getting started with vagrant including installation, vm starting up and a few commonly-used commands.

# Download and install the native package for your operating system
[http://www.vagrantup.com/downloads](http://www.vagrantup.com/downloads)

# Ensure virtualbox is also installed
[https://www.virtualbox.org/](https://www.virtualbox.org/)

# Check if vagrant is in system path
{% highlight bash %}
$ which vagrant
/usr/local/bin/vagrant => on Mac OS X
{% endhighlight %}

# Get vagrant box up and running
{% highlight bash %}
$ mkdir ~/VagrantVMs
$ cd ~/VagrantVMs
$ mkdir precise32
$ cd precise32
$ vagrant init hashicorp/precise32
  => This will create a 'Vagrantfile' in the directory which contains the configuration for the VM to be created and then download the ubuntu precise 32-bit box locally. More vagrant boxes can be found at: https://atlas.hashicorp.com/boxes/search
  => This step can also be executed with two consecutive commands like this:
  => $ vagrant init
     $ vagrant box add hashicorp/precise32
$ vagrant up
  => Boot the vm with the above box
$ vagrant ssh
  => Connect to the vagrant vm via ssh
  => Now you are working in the vagrant vm
{% endhighlight %}

# Other commonly-used commands
{% highlight bash %}
$ vagrant suspend
  => Save the state of running vm and stop it
$ vagrant halt
  => Gracefully shut down the vm and power it down
$ vagrant destroy
  => Delete the vm
$ vagrant up
  => Start the halted/suspended vagrant vm
$ vagrant reload
  => Start the halted/suspended vagrant vm with the updated configuration
$ vagrant status
  => View current state of vm
{% endhighlight %}
