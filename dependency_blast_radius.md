### Dependency Blast Radius

There is a side effect of versioning your dependencies.

When client modules are using a specific version of your module, they will be unaware of any changes until they upgrade the version of your module.

This allows you to release new versions of your module, without worrying if any clients will break. And it also keeps the clients safe when you introduce a bug in a new release.

There is a catch with this approach. 

Every time you release a new version of a common module, all clients must also update to that version. That means going to each client, generating a feature branch, testing, and generating a new release.

I believe the extra work is worth it, since it keeps your safe from a silly mistake to spill to all your client modules.