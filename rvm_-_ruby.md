# Manage Ruby with RVM

Using the recommended installation procedure from the Puppetlabs [1] site will give you the minimum requirements to run Puppet in your system. 

Unfortunately this means that your will probably end up with ruby version 1.8.7. This might not resemble your production environment. 

We want our development environment to match it production as much as possible. But we also want the chance to manipulate our environment so we can test multiple versions of our libraries.

That is I would like to recommend installing Puppet as a gem [2] instead of the package provided for your system.

In order to provide as much flexibility as possible in our Ruby installation we will use the tool RVM [3] to help us with this task.

The RVM reads *"RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems."*.

This means that we can multiple versions of Ruby installed in our system and switch between them as we please. RVM even gives us the option to configure a specific project to run under a specific version of Ruby while the rest run in another.





---


[1] Installing Puppet: Debian and Ubuntu. https://docs.puppetlabs.com/guides/install_puppet/install_debian_ubuntu.html

[2] RubyGems: https://rubygems.org/

[3] RVM - Ruby Version Manager: https://rvm.io/