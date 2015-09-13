# Setting up your development environment

Rule number 1 of the Puppet Club:

**We don't write or test code in our production environment.**


---

There is no simple, one click solution for testing Puppet code. 




To avoid any unexpected behavior, your development should resemble your production environment.

That means:
* Run the same OS
* Run the same distribution and version
* Run the same version of Puppet

It is uncommon that your working station is an exact match to your production server. To solve this situation we can use virtualization to provide a host that will match our production server.

Let's take a look at a tool called Vagrant, that will make it easy to manage our VM setup.

*Please feel free to skip the following sections if you are familiar with Vagrant and RVM*




