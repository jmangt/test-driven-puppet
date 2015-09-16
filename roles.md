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
hotdogcom/manifest/params.pp
class hotdogcom::params{
  
  if $::db_host == undef {
    $db_host = 'localhost'
    notice("tag_server_env not defined using ${server_env}")
  } else {
    $server_env = $::tag_server_env
  }
  
}
```
