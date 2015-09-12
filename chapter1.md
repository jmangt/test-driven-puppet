# Setting up your development environment

Rule number 1 of the Puppet Club:

**We don't write or test code in our production environment.**


---

To avoid any unexpected behavior, your development should resemble your production environment.

That means:
* Run the same OS
* Run the same distribution and version
* Run the same version of Puppet

It is uncommon that your working station is an exact match to your production server. To solve this situation we can use virtualization to provide a host that will match our production server.

## Vagrant

[Vagrant]( https://www.vagrantup.com/ ) is a tool that helps us provision and manage VMs. It works with common virtualization tools like Virtualbox, Vmware and Amazon EC2. 

The Vagrant site reads "Create and configure lightweight, reproducible, and portable development environments.".

Vagrant accomplished this by:
* Providing a standard way to share base Os boxes.
* Allow your to describe how to provision your VM.
* Mount and sync your local source into the VM.

### Installing Vagrant

Vagrant needs a virtualization provider to be able run. The default provider is [Virtualbox](https://www.virtualbox.org/). If your system doesn't have it installed, go to the [Downloads section](https://www.virtualbox.org/wiki/Downloads) of the site and get the corresponding installer for your system. 

After Virtualbox is intalled go the [Downloads section](https://www.vagrantup.com/downloads.html) of the Vagrant Site to download and run the appropriate installer for your system.

### Starting a Vagrant project

Create a directory where to host your vagrant project or use a directory that contains the source of a current project.

```
# create a new project
$ mkdir ~/my-vagrant-project
$ cd ~/my-vagrant-project
```

Vagrant configuration is managed by a `Vagrantfile` ( r[ead it's documentation](https://docs.vagrantup.com/v2/vagrantfile/index.html) ). You can add a basic `Vagrantfile` to your project by using the `vagrant init` command.

```
# initialize a Vagrant file
$ vagrant init

A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```

Lets take a look at the file we just created. After removing it's comments you can focus on two facts.

First, the `Vagrantfile` is a [Ruby](https://www.ruby-lang.org/en/) file, thus its contents can be manipulated just like any Ruby script.

Second, the `configure` class method takes arguments that allow us to define the image that our VM should be booted from.

Our default `Vagranfile` uses the `base` image to boot our VM. This is a place holder were we are expected to set the name of the vagrant box to use.

```
# a Vagrant file comments removed
$ cat Vagrantfile

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "base"
end
```

Lets re initialize our `Vagrantfile` with an Ubuntu 14.04 box.

```
$ rm Vagrantfile
$ vagrant init ubuntu/trusty64
...
$ cat Vagrantfile

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
 
end
```

Now we are ready to provision our VM.

### Provisioning

The easiest way to provision a Vagrant box is to use the Shell provisioner. 

Uncomment the `config.vm.provision` section so your `Vagrantfile` so it looks like this:

```
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y apache2
  SHELL
end
```

The provisioner section will run the first time you start a new system. If it is successful, it will mark the system as provisioned and will not run it again unless your force a re provision.





