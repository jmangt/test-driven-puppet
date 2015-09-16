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

The most common use case if passing database credentials. I suggest using a pattern similar to this:

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
    $db_user = 'my-user'
  } else {
    $db_user = $::db_user
  }
  
  if $::db_password == undef {
    $db_password = 'S3cr3t'
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

The pattern might seam a little verbose. But it will give you choice on how to test and deploy your module.

The `params.pp` file checks if the 

```
