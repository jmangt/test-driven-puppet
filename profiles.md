## Profiles

A **profile** is a collection of technologies, brought together for a specific purpose.

Here a are some examples:
* a Wordpress Site
* a Ruby on Rails Application
* a Node.js Application

A profile's responsibility is too coordinate many pieces of technology. Not by managing them itself but delegating that responsibility to their specific modules. 

Must of the time a profile will set up scaffolding for an application using that particular technology stack.

In the Wordpress example, the `worpress` profile will delegate installing and configuring Apache and PHP to their respective modules. The profile is in charge of passing the necessary parameters to them.

```ruby
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


A profile will usually manage 