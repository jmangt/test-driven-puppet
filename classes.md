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
