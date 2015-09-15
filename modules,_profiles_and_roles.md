## Modules, Profiles and Roles

The "[Module, Profile, Role](https://puppetlabs.com/presentations/designing-puppet-rolesprofiles-pattern)"[1] pattern is the preferd way of combining simple modules to achieve a greater purpose.

A way to look at the pattern is:

* **Module**: is a single piece of technology. For example: apache or php.
* **Profile**: is a collection of technologies and common configuration. For example: Wordpress. A Wordpress profile includes, apache, php and the Wordpress framework. As well as a way to declare a new vhost in apache and configuring the Wordpress instance.
* **Role**: is a collection of profiles that configure a **host**. For example: We can use a Wordpress profile and a SendMail profile to configure **hotdog.com** and maybe **cutekittens.org**.


In our Wordpress example we are able to split the technology components into two very clear modules, `apache` and `php`.

```puppet
# Apache Module
# apache/manifests/init.pp
class apache(vhost => 'default'){}

# PHP Module
# php/manifests/init.pp
class php(version => '5.3'){}
```

Then we put those together (after testing each one individually) into a `wordpress` profile. The profile will download the required version of Wordpress and by using the `apache` and `php` modules it should be able to create an generic wordpress site.

```ruby
# Wordpress Profile
# wordpress/manifests/init.pp
class wordpress(site_name => 'default'){
  class{'apache':
    vhost => $site_name,
  }
  contain apache
  
  class{'php':
    version => '5.3',
    require => Class['apache']
  }
  contain php
  
  class{'wordpress::install':
   require => [Class['apache'],
               Class['php']]
  }
  contain wordpress::install
  
  class{'wordpress::configure':
    site_name => $site_name,
    require   => Class['wordpress::install'],
  }
  contain wordpress::configure
}
```

And now the final piece: the `role`. We use Roles to configure a specific instance of a profile or profiles.

Once we have out `wordpress` profile ready it is quite easy to configure a specific host it. 

```puppet
# HotDog.com Role
# hotdogcom/manifests/init.pp
class hotdogcom{
  class{'wordpress':
    site_name => 'hotdog.com',
  }
}

# CuteKittens.org Role
# cutekittensorg/manifests/init.pp
class cutekittensorg{
  class{'wordpress':
    site_name => 'cutekittens.org',
  }
}
```

### Why should I use this over complicated thing?

Because it is good for you ;)

When it comes to testing, the pattern is useful in to main things:
1. You can test the components in isolation.
2. It creates a clear separation of responsibilities between your components.

**This means that you can pinpoint who's fault is it when a Puppet run fails** . 

If your individual modules are passing all their tests, but when you put them together something fails, it becomes a lot easier to figure out where the problem is. 

If you are reading this book your have probably experience the pain of trying to untangle a ball of dependencies in a giant monolithic module. So spending some time to split your module into the Module, Profile, Role pattern is time well spent.

---


[1] Designing Puppet: Roles/Profiles Pattern: https://puppetlabs.com/presentations/designing-puppet-rolesprofiles-pattern
