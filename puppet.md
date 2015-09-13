# Puppet

## Installing Puppet

Let's install Puppet using the latest version available to us.

Select your desired version of Ruby and install the puppet gem with all its dependencies.

```bash
vagrant:$ rvm use 2.2.1 --default
vagrant:$ gem install puppet

Fetching: json_pure-1.8.2.gem (100%)
Successfully installed json_pure-1.8.2
Fetching: hiera-3.0.1.gem (100%)
Successfully installed hiera-3.0.1
Fetching: facter-2.4.4.gem (100%)
Successfully installed facter-2.4.4
Fetching: puppet-4.2.1.gem (100%)
Successfully installed puppet-4.2.1
Parsing documentation for json_pure-1.8.2
Installing ri documentation for json_pure-1.8.2
Parsing documentation for hiera-3.0.1
Installing ri documentation for hiera-3.0.1
Parsing documentation for facter-2.4.4
Installing ri documentation for facter-2.4.4
Parsing documentation for puppet-4.2.1
Installing ri documentation for puppet-4.2.1
Done installing documentation for json_pure, hiera, facter, puppet after 27 seconds
4 gems installed

vagrant:$ puppet --version
4.2.1

vagrant:$ facter --version
2.4.4

vagrant:$ hiera --version
3.0.1
```

## Install multiple versions of Puppet via RVM

### Gemsets

RVM provides a way to compartmentalize independent Ruby setups via Gemsets [1].

You can imagine a Gemset as a box that lives inside your Ruby's version. Inside you can store any gems you desire, but the versions of the gems you install will be independent from any other box. Thus you can run different versions of Puppet and its dependencies without any collision problems.

You can list all gemsets using the `rvm list gemsets`. This will display all ALL gemsets across ALL the different versions of ruby. 

```bash
vagrant:$ rvm list gemsets

rvm gemsets

   ruby-1.8.7-head [ x86_64 ]
   ruby-1.8.7-head@global [ x86_64 ]
   ruby-1.8.7-p374 [ x86_64 ]
   ruby-1.8.7-p374@global [ x86_64 ]
   ruby-2.1.7 [ x86_64 ]
   ruby-2.1.7@global [ x86_64 ]
=> ruby-2.2.1 [ x86_64 ]
   ruby-2.2.1@global [ x86_64 ]
```

As with the `rvm list` command, the output will show you what is the current gemset in use. In this case the `ruby-2.2.1 [ x86_64 ]` gemset is in use.

You can also list just the gemsets that belong to your currently selected version of Ruby using the `rvm gemset list` command.

```bash
vagrant:$ rvm gemset list

gemsets for ruby-2.2.1 (found in /home/vagrant/.rvm/gems/ruby-2.2.1)
=> (default)
   global
```

In this case we see that the `(default)` gemset is in use. Currently this gemset holds our Puppet 4.2.1 installation.

```bash
vagrant:$ gem list --local | grep puppet

puppet (4.2.1)
```

Let's try now to switch versions of Ruby and see what the effect is in our gemset.

```bash
vagrant:$ rvm use 1.8.7
vagrant:$ rvm gemset list
rvm gemset list

gemsets for ruby-1.8.7-head (found in /home/vagrant/.rvm/gems/ruby-1.8.7-head)
=> (default)
   global
   
vagrant:$ gem list --local | grep puppet
# DUDE WHERE IS MY CAR???
```

As you can see the Puppet gem is not installed in our `(default)` gemset for Ruby 1.8.7. This gives us the chance to install a different version of Puppet for this environment. This new version and it's dependencies will be independent from any other installed versions. 

This is quite handy since it allow us to test our modules in different versions of puppet. A simple use case for this is when we need to make sure our modules are compatible with a new version of Puppet. Or if our current version of Puppet is compatible with a newer version of Ruby.

Let's install puppet `3.7.4` in our 1.8.7 `(default)` gemset.

```bash
vagrant:$ rvm use 1.8.7
vagrant:$ gem install puppet --version=3.7.4
vagrant:$ gem list --local | grep puppet
puppet (3.7.4)
```

There you have it. A production like environment running multiple versions of Ruby and a specific version of Puppet in each one.

But wait there is still more...

What if we needed to test two different versions of Puppet with the same Ruby version?

Fear not RVM provides a feature called "Named Gemsets" that will help us with this scenario.

### Named Gemsets

A named gemset is nothing more that providing a name for the "box" where your want to store your gems.

You can create a new gemset by selecting the version of Ruby where you want to create it and then using the `rvm gemset create NAME` command.

Let's go back to our Ruby 2.2.1 version and create a gemset where we can install Puppet 3.7.4.

```bash
vagrant:$ rvm use 2.2.1

vagrant:$ rvm gemset create puppet-3.7.4
ruby-2.2.1 - #gemset created /home/vagrant/.rvm/gems/ruby-2.2.1@puppet-3.7.4
ruby-2.2.1 - #generating puppet-3.7.4 wrappers........

vagrant:$ rvm gemset list
gemsets for ruby-2.2.1 (found in /home/vagrant/.rvm/gems/ruby-2.2.1)
=> (default)
   global
   puppet-3.7.4
```

We now have a new gemset puppet-3.7.4 that currently hold no gems. Just as we did with Ruby version we can switch gemset by using the `rvm gemset use NAME` command.

Switch to the `puppet-3.7.4` gemset and install Puppet 3.7.4 in it.

```bash
vagrant:$ rvm gemset use puppet-3.7.4
Using ruby-2.2.1 with gemset puppet-3.7.4

vagrant:$ rvm gemset list
gemsets for ruby-2.2.1 (found in /home/vagrant/.rvm/gems/ruby-2.2.1)
   (default)
   global
=> puppet-3.7.4

vagrant:$ gem list --local | grep puppet
# DUDE WHERE IS MY CAR???

vagrant:$ gem install puppet --version=3.7.4
vagrant:$ gem list --local | grep puppet
puppet (3.7.4)
```

### Project specific configuration

As you have seen gemsets are useful, but having to remember what version of what belong where can become a burden.

RVM provides a way for us to configure a specific version of Ruby and a specific gemset to be used per project basis.

This means that within a specific directory RVM can know what versions to use. And if the specific setup is not present in your system it will give your directions on how to set it up.

In oder to demonstrate this feature create a directory and add the following files to it.

```bash
vagrant:$ cd ~
vagrant:$ mkdir puppet-4.2.1
vagrant:$ echo '2.2.1' > ~/puppet-4.2.1/.ruby-version
vagrant:$ echo 'puppet-4.2.1' > ~/puppet-4.2.1/.ruby-gemset
```

Change now to that directory. RVM will check if both the version of Ruby and the gemset exists. If they do not it will create them for your or prompt you for action. Additionally RVM will switch your session to use the specified Ruby version and gemset.

```bash
vagrant:$ cd ~/puppet-4.2.1
vagrant:$ ruby-2.2.1 - #gemset created /home/vagrant/.rvm/gems/ruby-2.2.1@puppet-4.2.1
ruby-2.2.1 - #generating puppet-4.2.1 wrappers..........

vagrant:$ rvm gemset list
gemsets for ruby-2.2.1 (found in /home/vagrant/.rvm/gems/ruby-2.2.1)
   (default)
   global
   puppet-3.7.4
=> puppet-4.2.1

vagrant:$ gem install puppet --version=4.2.1
vagrant:$ gem list --local | grep puppet
puppet (4.2.1)
```

There you go. Now you can start playing around with different configurations, both of Puppet and Ruby.

### A word of caution

RVM is a awesome development tool. But the truth is that it does not play well with production systems. Must other tools expect Ruby to be in a specific system wide path. More than that. Puppet will probably run under a user with no session, like root. There is little chance that the magic PATH switching that RVM performs will play well in that case.

Don't take me wrong. You can make it work. But you will pay the price for it.

Trust me. If you need two versions of Ruby in your system you are probably doing something wrong.

If your system does not provide an updated version of Ruby please consider building from source. You might feel like it is old school but it will save you the pain of having to wonder what is running where.

If you are running Ruby applications that might conflict with system's Ruby version, I suggest that you evaluate a container technology like [Docker](https://www.docker.com/) [2] to deploy your application.


---

[1] Gemsets: https://rvm.io/gemsets/basics

[2] Docker: https://www.docker.com/
