## Profiles

A **profile** is a collection of technologies, brought together for a specific purpose.

ere a are some examples:
* a Wordpress Site
* a Ruby on Rails Application
* a Node.js Application

A profile's responsibility is too coordinate multiple pieces of technology. Not by managing them itself but delegating that responsibility to their specific modules. 

Must of the time a profile will set up scaffolding for an application using that particular technology stack.

In the Wordpress example, the `worpress` profile will delegate installing and configuring Apache and PHP to their respective modules. The profile is in charge of passing the necessary parameters to them.

```puppet
# Wordpress Profile
# wordpress/manifests/init.pp
class wordpress (
 site_name => 'default',
){

  class{'apache':
    vhost => $site_name,
  }
  contain apache
  
  class{'php':
    version => '5.3',
    require => Class['apache']
  }
  contain php
  
  ...
}
```

A profile will define a set of defaults for the technology stack. These are to be placed both as parameters to the module and as variables in the `params` class of the module. 

```puppet
# wordpress/manifests/params.pp
class wordpress::params {
  $site_name   = 'default'
  $php_version = '5.3'
}

# wordpress/manifests/init.pp
class wordpress(
  site_name   => $::wordpress::params::site_name,
  php_version => $::wordpress::params::php_version,
){
 
  class{'apache':
    vhost => $site_name,
  }
  contain apache
  
  class{'php':
    version => $php_version,
    require => Class['apache']
  }
  contain php

}

```