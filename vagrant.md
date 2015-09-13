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

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y apache2
  SHELL
end
```

The provisioner section will run the first time you start a new system. If it is successful, it will mark the system as provisioned and will not run it again unless your force a re provision.

### Starting your box for the first time

Use the `vagrant up` command to start your vagrant box and trigger a provisioning event if needed.

```bash
$ cd my-vagrant-project
$ vagrant up

Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ubuntu/trusty64'...
...
==> default: Forwarding ports...
    default: 22 => 2222 (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Mounting shared folders...
    default: /vagrant => /Users/jmangt/my-vagrant-project
==> default: Running provisioner: shell...
    default: Running: inline script
==> default: stdin: is not a tty
==> default: Ign http://security.ubuntu.com trusty-security InRelease
==> default: Ign http://archive.ubuntu.com trusty InRelease
...
==> default: The following extra packages will be installed:
==> default:   apache2-bin apache2-data libapr1 libaprutil1 libaprutil1-dbd-sqlite3
==> default:   libaprutil1-ldap ssl-cert
...
==> default:  * Starting web server apache2
...
```

After Vagrant has finished its run you will now have a fully functional Ubuntu server running apache2.

### Check the status of your VM

User the `vagrant status` command to check the current status of your VM.

```bash
$ cd ~/my-vagrant-project
$ vagrant status

Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
```

### SSH into your box

Use the `vagrant ssh` to start a remote session into your box.

```bash
$ cd ~/my-vagrant-project
$ vagrant ssh

Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.13.0-55-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Sat Sep 12 23:51:42 UTC 2015

  System load:  0.38              Processes:           80
  Usage of /:   3.2% of 39.34GB   Users logged in:     0
  Memory usage: 25%               IP address for eth0: 10.0.2.15
  Swap usage:   0%

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


vagrant@vagrant-ubuntu-trusty-64:~$
```

### Networking 

If you try to reach the apache2 instance your will find yourself stuck. Vagrant does not provide a simple way to find the ip address of the VM. You can always ssh into the box and run a `ifconfig` command to find out.

Use the `config.vm.network` property in your `Vagrantfile` to setup a private ip address for your VM.

```ruby
# ~/my-vagrant-project/Vagranfile
...
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"
...
```

The second change to your `Vagrantfile` is to forward a port from your host to the corresponding `port 80` in your VM.

```ruby
# ~/my-vagrant-project/Vagranfile
...
# Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 80, host: 8080
...
```

Now, we need to reboot our VM so the changes can take effect.

Use the `vagrant reload` command to reboot your VM.

```bash
cd ~/my-vagrant-project
vagrant reload

==> default: Attempting graceful shutdown of VM...
...
==> default: Forwarding ports...
    default: 80 => 8080 (adapter 1)
...
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> default: to force provisioning. Provisioners marked to run always will still run.
```

All that we have left to do is try our shiny new apache instance.

```bash
$ curl 192.168.33.10
```

In case you wanted other packages, users or directories added to your VM all you need to do is add the corresponding commands to the `provision` section and run `vagrant reload --provision` for the changes to be applied.

An that is all you need to know to start using Vagrant.


