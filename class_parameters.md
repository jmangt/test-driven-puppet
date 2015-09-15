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