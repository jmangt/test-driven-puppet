## Node.js

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