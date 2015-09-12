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

### Install Vagrant

Vagrant needs a virtualization provider to be able run. The default provider is [Virtualbox](https://www.virtualbox.org/). If your system doesn't have it installed, go to the [Downloads section](https://www.virtualbox.org/wiki/Downloads) of the site and get the corresponding installer for your system. 

After Virtualbox is intalled go the [Downloads section](https://www.vagrantup.com/downloads.html) of the Vagrant Site to download and run the appropriate installer for your system.

### 







