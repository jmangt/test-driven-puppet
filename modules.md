
### Modules

*My definition* of **module** is: "*A collection of Puppet resources that administer one and only one piece of technology*".

That means that:
*  An apache **module** manages Apache
*  An nginx **module** manages Nginx
*  An ntp **module** manages Ntp

If you are not sure where to draw a line in you module's design, read a bit on the 'Single responsabilty principle' (SRP) pattern [1]. This design pattern is use widely in Object Oriented Programming (OOP) and can be applied to your Puppet code as well.

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



---

[1] Single responsability principle: http://www.objectmentor.com/resources/articles/srp.pdf
