### Dependency Blast Radius

A side effect of versioning your dependencies, is that you can safely add features to your modules and not worry if a new release will break other modules. Since the client modules are using a specific version of your module, they will be unaware of any changes until they upgrade the version of your module.

When they are ready to use the new version, they will update their Puppetfile, pull the new versions, run tests and if it all goes well, generate a new release.

There is a catch with this approach. When have a common module used by many clients, every time you release a new version, all clients must update to that version also. That means going to each client, generating a feature branch, testing, and generating a new release.

I believe the extra work is worth it, since it keeps your safe from a silly mistake to spill to all your client modules.