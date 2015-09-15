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

## Class Parameters

* All modules should contain a `params` class.

```bash
$ cd ~/helloworld
.
|--manifests
|  |-- init.pp
|  |-- params.pp
```

* All default values for the module or internal classes should be declared in the `params` class.

```puppet
# manifests/params.pp
class helloworld::params{
  $salutation = 'Hello'
  $who        = 'World'
}
```

* All internal classes should only inherit from the `params` class. Any other form of inheritance should be avoided.

```puppet
# manifests/init.pp
class helloworld inherits helloworld::params{
...
}
```

* The **only goal** of the `params` class is to provide default values for the rest of the module's class parameters.

```puppet
# manifests/init.pp
class helloworld(
  $salutation = $::helloworld::params::salutation,
  $who        = $::helloworld::params::who,
) inherits helloworld::params{

  file{'/tmp/hello-world.txt':
    content => inline_template("<%= @salutation %>, <%= @who %>"),
  }
  
}
```

* All top level variables like `facters`, `hiera` lookups, `scope` lookups should terminate in the `params` class.

```puppet
# manifests/params.pp
class helloworld::params{

  # Getting values from a sytem fact
  if $::tag_salutation == undef {
    $salutation = 'Hello'
    notice("tag_salutation not defined using ${salutation}")
  } else {
    $salutation = $::tag_salutation
  }
  
  # Getting values from Hiera
  if hiera('who') == undef {
    $who = 'World'
    notice("who not defined using ${who}")
  } else {
    $who = hiera('who')
  }
  
}
```

* Variables should always be `local`. The only allowed namespaced variables should be the ones that the [class parameters](#class-parameters) uses as defaults.

```puppet
# mymodule/manifests/init.pp
class mymodule(){
  
  include directories
  
  # By using the $::directories::default_path variable
  # we have locked ourselves out of being able to full test
  # this resource. Can you figure out why?
  
  $directory = "${::directories::default_path}/a-directory"
  file{$directory:
    ensure => directory,
    require => File[$::directories::default_path]
  }
}
```




---

[1] Beginner's Guide to Modules: https://docs.puppetlabs.com/guides/module_guides/bgtm.html#beginner%27s-guide-to-modules

[2] Single responsability principle: http://www.objectmentor.com/resources/articles/srp.pdf

[3] Designing Puppet: Roles/Profiles Pattern: https://puppetlabs.com/presentations/designing-puppet-rolesprofiles-pattern