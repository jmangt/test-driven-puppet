## Roles

A **role** is a collection of profiles that describe a host.

This means that you `role` will **fully** describe all things that are provisioned within a type of host.

For example:
* A `db` role should describe a host that is running a database.
* A `web` role should describe a host that runs a some kind of web server.
* A `log` role should describe a host that is running a logs agregator.

As you might imagine a `web` or `db` host might running many web applications or database instances.

A `role` will need to list all the necessary dependencies for that kind of application to run. And probably other technology stacks that help with monitoring or log management.

In our wordpress example we could define a role like this:

```puppet
# hotdogcom/manifests/init.pp
class hotdogcom{
 
 wordpress::host{'hotdog.com':
 }
 
 include nagios::agent
 
 include logstash::agent
 
 include graphana::agent
 
}
```

When it comes to parameters, a `role` should pay attention of providing ways to pass the necessary credentials, hosts and ports that our applications will use.

The most common use case if passing database credentials. I suggest using a pattern like this:

```puppet
# hotdogcom/manifests/params.pp
class hotdogcom::params{
  
  if $::db_host == undef {
    $db_host = 'localhost'
  } else {
    $db_host = $::db_host
  }
  
  if $::db_port == undef {
    $db_port = '3306'
  } else {
    $db_port = $::db_port
  }
  
  if $::db_name == undef {
    $db_name = 'hotdogcom'
  } else {
    $db_name = $::db_name
  }
  
  if $::db_user == undef {
    $db_user = 'myuser'
  } else {
    $db_user = $::db_user
  }
  
  if $::db_password == undef {
    $db_password = 'password'
  } else {
    $db_password = $::db_password
  }
}

# hotdogcom/manifests/init.pp
class hotdogcom (
  $db_host     = $::hotdogcom::params::db_host,
  $db_port     = $::hotdogcom::params::db_port,
  $db_name     = $::hotdogcom::params::db_name,
  $db_user     = $::hotdogcom::params::db_user,
  $db_password = $::hotdogcom::params::db_password,
){

  wordpress::host{'hotdog.com':
    db_host     => $db_host,
    db_port     => $db_port,
    db_name     => $db_name,
    db_user     => $db_user,
    db_password => $db_password,
  }
}

# site.pp
class{'hotdogcom':
  db_host     => 'db.prod.mycorp.com',
  db_port     => '3306',
  db_name     => hiera('hotdogcom::db_name'),
  db_user     => hiera('hotdogcom::db_user'),
  db_password => hiera('hotdogcom::db_password'),
}

```
The pattern might seam a little verbose. But it will give you choice on how to test and deploy your module.

The `params.pp` file checks if the there is a top level variable with the values of the host, port, database name, user or password. If the value is not set it uses a 'development' default.

The value of these top level variables could be a custom fact set by a cloud init script, foreman instance or a puppet master.

The second part of the pattern, is the role's api. Repeating the same variables as in parameters allows us to overwrite the defaults set by params. This is a must when it comes to testing your module. Since you want to skip any external dependencies like relying on a fact being set for you from an unknown source.

In the example `site.pp` passes parameters to the `role` by a combination of fixed values and hiera lookups. This allows you to manage different configuration values based on your environment. For example you might want to set a different db use for `production` that the one you use in `qa`.