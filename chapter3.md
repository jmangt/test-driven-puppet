# Organizing Puppet modules

If you want to test your Puppet modules, you need to write your modules in a testable way.

In the case you are dealing with legacy code, I recommend that you set some time to refactor those modules. 

Use git and gitflow to allow you start a new `feature/refactor` branch. There you will be able to add new functionality to your module an ensure that your production branch is stable.

The folks at Puppetlabs have taken the time to write great documentation on how to design and structure your modules. Please take some time to go over the ["
Beginner's Guide to Modules](https://docs.puppetlabs.com/guides/module_guides/bgtm.html#beginner%27s-guide-to-modules)" section in the https://docs.puppetlabs.com website [1].

I would like to just highlight some concepts and design decisions that will impact in the way you test your modules.

## Classes

A class is a collection of resources that should be considered *independent* and *encapsulated*.

This means that:

* Your `class` should be able to fend for itself. You can not rely on the system to have a
resource in place unless the class itself provides it.

```ruby
# manifests/init.pp
class myclass{

    # will fail since we are not making sure
    # the /opt/mycompany directory is present
    # before creating our file
    
    file{'/opt/mycompany/config.conf':
     ensure => present
    }
}
```

* Any information your `class` needs should come from the class's `parameters`. Top level variables like `facters` should be avoided. Except as default values for your class's parameters.

```ruby
# manifests/init.pp
class myclass{

  # the value of owner is being set my a top scope variable
  # but the variable is not defined within the class
  # so we are not sure where and if the value was set
  
  file{'/opt/mycompany/config.conf':
    ensure => present,
    owner  => $::the_owner,
  }
}
```






---

[1] Beginner's Guide to Modules: https://docs.puppetlabs.com/guides/module_guides/bgtm.html#beginner%27s-guide-to-modules

[2] Single responsability principle: http://www.objectmentor.com/resources/articles/srp.pdf

[3] Designing Puppet: Roles/Profiles Pattern: https://puppetlabs.com/presentations/designing-puppet-rolesprofiles-pattern