## Roles

A **role** is a collection of profiles that describe a host.

This means that you `role` will **fully** describe all things that are provisioned within a type of host.

For example:
* A `db` role should describe a host that is running a database.
* A `web` role should describe a host that runs a some kind of web server.
* A `log` role should describe a host that is running a logs agregator.

As you might imagine a `web` or `db` host might running many web applications or database instances.

A `role` will need to list all the necessary dependencies for that kind of application to run and probably other technology stacks that help with monitoring or 

