# Manage Ruby with RVM

Using the recommended installation procedure from the Puppetlabs [1] site will give you the minimum requirements to run Puppet in your system. 

Unfortunately this means that your will probably end up with ruby version 1.8.7. This might not resemble your production environment. 

We want our development environment to match it production as much as possible. But we also want the chance to manipulate our environment so we can test multiple versions of our libraries.

That is I would like to recommend installing Puppet as a gem [2] instead of the package provided for your system.

In order to provide as much flexibility as possible in our Ruby installation we will use the tool RVM [3] to help us with this task.

The RVM reads *"RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems."*.

This means that we can multiple versions of Ruby installed in our system and switch between them as we please. RVM even gives us the option to configure a specific project to run under a specific version of Ruby while the rest run in another.

## Installing RVM
*
The following commands will install RVM to be used within the context of your **non root user**. RVM **will not be available as a system command**. If you need RVM to be used system wide please refer to the [Multi-User installs section](https://rvm.io/support/troubleshooting#sudo) of the RVM site [5].
*

Ssh into your Vagrant Box instance with the `vagrant ssh` command. 

```bash
$ cd ~/my-vagrant-project
$ vagrant up # < in case your vm is not running
$ vagrant ssh
```

Once inside your VM run the following commands.

```bash
vagrant:$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
vagrant:$ \curl -sSL https://get.rvm.io | bash -s stable
...
  * To start using RVM you need to run `source /home/vagrant/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
...
```

As the output describes RVM needs to be loaded into your user's terminal session before it can be used. If you wan't go ahead and run the following command to load it into your session. Another choice is to exit your ssh session and start a new one with `vagrant ssh`.

```bash
vagrant:$ source /home/vagrant/.rvm/scripts/rvm
```

Make sure all is working as expected by running:

```bash
vagrant:$ rvm

= rvm

* https://rvm.io/
* https://github.com/wayneeseguin/rvm/

== DESCRIPTION:

RVM is the Ruby enVironment Manager (rvm).
....
```

If you run into any issues with this procedure please refer to the [Troubleshooting section](https://rvm.io/support/troubleshooting) [4] of the RVM site.

## Available Ruby versions

RVM gives you the choice to install multiple versions of Ruby in your system. 

Take a look at the available versions by running `rvm list known`.

```bash
vagrant:$ vagrant@vagrant-ubuntu-trusty-64:~$ rvm list known
# MRI Rubies
[ruby-]1.8.6[-p420]
[ruby-]1.8.7[-head] # security released on head
[ruby-]1.9.1[-p431]
[ruby-]1.9.2[-p330]
[ruby-]1.9.3[-p551]
[ruby-]2.0.0[-p643]
[ruby-]2.1.4
[ruby-]2.1[.5]
[ruby-]2.2[.1]
[ruby-]2.2-head
ruby-head

# for forks use: rvm install ruby-head-<name> --url https://github.com/github/ruby.git --branch 2.1

# JRuby
jruby-1.6.8
jruby[-1.7.19]
jruby-head
jruby-9.0.0.0.pre1

# Rubinius
rbx-1.4.3
rbx-2.4.1
rbx[-2.5.2]
rbx-head

# Opal
opal

# Minimalistic ruby implementation - ISO 30170:2012
mruby[-head]

# Ruby Enterprise Edition
ree-1.8.6
ree[-1.8.7][-2012.02]

# GoRuby
goruby

# Topaz
topaz

# MagLev
maglev[-head]
maglev-1.0.0

# Mac OS X Snow Leopard Or Newer
macruby-0.10
macruby-0.11
macruby[-0.12]
macruby-nightly
macruby-head

# IronRuby
ironruby[-1.1.3]
ironruby-head
```

## Installing Ruby

Once you have made your choice, you can install a specific version of Ruby using the command `rvm install`. 

You can specify a version passing the version number as an argument.

```bash
vagrant:$ rvm install 2.1.7
```

Or allow RVM to choose the latest stable version  of Ruby for you. At the time of this writing the current stable version of Ruby is 2.2.1.

```bash
vagrant:$ ruby install ruby
...
vagrant:$ ruby -v
ruby 2.2.1p85 (2015-02-26 revision 49769) [x86_64-linux]
```

In case no binaries are available for the version of Ruby you chose, RVM will install it by compiling it from source. This might take a while, so be patient.

## Managing your Ruby versions

In my system I have installed three versions of Ruby to able to show you how to switch between them.

### Listing your installed versions of Ruby

Use the `rvm list` command to list all installed versions of Ruby in your system.

```bash
vagrant:$ rvm list

rvm rubies

   ruby-1.8.7-head [ x86_64 ]
   ruby-1.8.7-p374 [ x86_64 ]
   ruby-2.1.7 [ x86_64 ]
=* ruby-2.2.1 [ x86_64 ]

# => - current
# =* - current && default
#  * - default
```

If you pay attention to the command's output, you will see a legend that shows what version is in use and what version is your user's default.

Change the current version by using the command `rvm use VERSION`.

```bash
vagrant$: rvm use 1.8.7
Using /home/vagrant/.rvm/gems/ruby-1.8.7-head

vagrant$: rvm list

rvm rubies

=> ruby-1.8.7-head [ x86_64 ]
   ruby-1.8.7-p374 [ x86_64 ]
   ruby-2.1.7 [ x86_64 ]
 * ruby-2.2.1 [ x86_64 ]

# => - current
# =* - current && default
#  * - default
```

The `rvm list` command now shows us that we are using 1.8.7 but that our user's default version is still 2.1.1. That means that if we exit this session or start a new one, 2.1.1 will be selected for use.

If we want to make 1.8.7 the default version for our use we can pass the `--default` flag to the use command.

```bash
vagrant:$ rvm use 1.8.7 --default
```


---


[1] Installing Puppet: Debian and Ubuntu. https://docs.puppetlabs.com/guides/install_puppet/install_debian_ubuntu.html

[2] RubyGems: https://rubygems.org/

[3] RVM - Ruby Version Manager: https://rvm.io/

[4] Troubleshooting RVM: https://rvm.io/support/troubleshooting

[5] Multi-User Installs - Using the sudo command: https://rvm.io/support/troubleshooting#sudo