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

## Class API

A classes should be treated like an atomic unit. From the perspective of the module's user there should be **one and only one ** way to interact with it. And that is the class parameters.

**Your class parameters define your class API.
**

Deciding how to design your class API can be tricky, and it will require some trial an error to get it right.

My suggestion is to use the time tested `it's magical` approach.

This means that you should treat your class like it is magical. It should be able to read  your mind and meet your needs without you telling it anything. 

1. Name it. 
2. Run it. 
3. Did it do what you wanted? 
4. No? 
5. What did you miss? 
6. A parameter? Add it. 
7. Repeat

And so on and so forth.

***Providing a good API for your class might be the most important part of making your code testable.***

A solid API will allow you provide multiple test scenarios. Each scenario will be based on the combinations of your parameters.

```puppet
# manifests/init.pp
class helloworld(
  $salutation = $::helloworld::params::salutation,
  $who        = $::helloworld::params::who,
  $owner      = $::helloworld::params::owner,
) {
...
}
```

For example, class helloworld has tree parameters `salutation`, `who` and `owner`. This gives you at least eight scenarios, based on whether you pass or don't pass values to the class when your call it.

Where is two of them:

* what happens when i call the class without passing any parameters?
*what happens when i call the class passing all parameters?"

From there, and depending what your class does with it's parameters, you will find yourself with as many scenarios as parameter combinations you can produce.

A rule of thumb for selecting parameters is *"Pass as little parameters as you can to get the job done. Manage other options with sensible defaults."*.

## Modules

*My definition* of **module** is: "*A collection of Puppet resources that administer one and only one piece of technology*".

That means that:
*  An apache **module** manages Apache
*  An nginx **module** manages Nginx
*  An ntp **module** manages Ntp

If you are not sure where to draw a line in you module's design, read a bit on the 'Single responsabilty principle' (SRP) pattern [2]. This design pattern is use widely in Object Oriented Programming (OOP) and can be applied to your Puppet code as well.

The better your apply the SRP, the easier it will be to re use your Puppet modules, and the easier it will be to test them.

Here a some guideline on how to interact with modules:

* Just like with classes Modules should be considered atomic. They should be able to take care of themselves without depending on the system to provide it's resources.
* If the module depends on a resource to be present, it should make sure it is there.

```puppet
$ cd ~/helloworld
.
|--manifests
|  |-- init.pp
|  |-- params.pp
```

```puppet
# manifests/init.pp
class helloworld(
  $salutation = $::helloworld::params::salutation,
  $who        = $::helloworld::params::who,
  $owner      = $::helloworld::params::owner,
) inherits helloworld::params{

  # The "owner" user is not defined in this
  # module. 

  file{'/tmp/hello-world.txt':
    content => inline_template("<%= @salutation %>, <%= @who %>"),
    owner   => $owner,
  }
  
}
```

* If this resource falls outside of the scope of the module, a dependency is introduced and a new module is to be included to take care of it.


```puppet
# manifests/init.pp
class helloworld(
  $salutation = $::helloworld::params::salutation,
  $who        = $::helloworld::params::who,
  $owner      = $::helloworld::params::owner,
) inherits helloworld::params{
  
  # this module will take care of creating
  # the company's default users
  include users
    
  # our resource makes sure that user "owner" is
  # present before trying to create the file
  file{'/tmp/hello-world.txt':
    content => inline_template("<%= @salutation %>, <%= @who %>"),
    owner   => $owner,
    require => User[$owner],
  }
  
}
```

* The module should not use any of the dependency's internal resources. It's interactions should be via the dependency's API.


Autoload paths

Module, Profile, Role



---

[1] Beginner's Guide to Modules: https://docs.puppetlabs.com/guides/module_guides/bgtm.html#beginner%27s-guide-to-modules

[2] Single responsability principle: http://www.objectmentor.com/resources/articles/srp.pdf