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
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'ubuntu/trusty64' is up to date...
==> default: A newer version of the box 'ubuntu/trusty64' is available! You currently
==> default: have version '20150609.0.10'. The latest is version '20150909.1.0'. Run
==> default: `vagrant box update` to update.
==> default: Setting the name of the VM: my-vagrant-project_default_1442074967546_86056
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 => 2222 (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Connection timeout. Retrying...
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if its present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Mounting shared folders...
    default: /vagrant => /Users/jalvarez/Projects/Test/my-vagrant-project
```

Several things will happend when you run `vagrant up`
