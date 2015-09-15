## Profiles

A **profile** is a collection of technologies, brought together for a specific purpose.

ere a are some examples:
* a Wordpress Site
* a Ruby on Rails Application
* a Node.js Application

A profile's responsibility is too coordinate multiple pieces of technology. Not by managing them, but by delegating that responsibility to their specific modules. 

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

Another example of a profile could be a Node.js Stack. In this case we will need not only modules that take care of installing Node.js, but also some directories to place our node application. Higher level modules can make 

```puppet
# nodeapp/manifests/params.pp
class nodeapp::params {
  nodejs_version        => 'v4.0.0',
  application_user      => 'web',
  application_name      => 'default',
  application_directory => '/opt/myapps',
}

# nodeapp/manifests/init.pp
class nodeapp (
  $nodejs_version = $::nodeapp::params::nodejs_version,
  $user           = $::nodeapp::params::application_user,
  $name           = $::nodeapp::params::application_name,
  $directory      = $::nodeapp::params::application_directory,
){

  class{'nodejs':
    version => $nodejs_version
  }
  contain nodejs
  
  class{'nodeapp::directories':
    owner     => $user,
    directory => "${directory}/${name}"
  }
  contain nodeapp::directories
}

# myapp/manifests/init.pp
class myapp{
  class{'nodeapp':
   name => 'myapp'
  }
  contain 'nodeapp'
}
```

### A word of warning

The previous examples of profiles are meant to be used once per role.** If your profile is used more than once per role Puppet will complain of not being able to re declare a resource**. If this is your use case you should explore the option of using custom resources with `define` instead on `class`.

Please read on the ["Defined resource types](https://docs.puppetlabs.com/puppet/latest/reference/lang_defined_types.html)" section of the Puppetlabs site.
