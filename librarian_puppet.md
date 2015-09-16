## Librarian Puppet

"[Librarian-puppet](http://librarian-puppet.com/) is a bundler for your puppet infrastructure. You can use librarian-puppet to manage the puppet modules your infrastructure depends on, whether the modules come from the Puppet Forge, Git repositories or a just a path." [1]

Just like the description reads, `librarian-puppet` is a gem that uses a manifest file (Puppetfile) to list all your module's private an public dependencies. And it uses this file to download and install the module's dependencies.

`librarian-puppet` will do the work of resolving the dependency tree of your module, by finding your dependency's dependencies and determining the minimum acceptable version for each.

### Puppetfile

A `Puppetfile` looks like this.

```ruby
forge "https://forge.puppetlabs.com"

mod 'puppetlabs-razor'
mod 'puppetlabs-ntp', "0.0.3"

mod 'puppetlabs-apt',
  :git => "git://github.com/puppetlabs/puppetlabs-apt.git"

mod 'puppetlabs-stdlib',
  :git => "git://github.com/puppetlabs/puppetlabs-stdlib.git"

mod 'puppetlabs-apache', '0.6.0',
  :github_tarball => 'puppetlabs/puppetlabs-apache'
```

The file declares both public and private dependencies. In it you can declare a specific version of your dependency or an acceptable range of versions. 

It is also possible to declare a different source from the Forge for your module. In the example, `puppetlabs-apt` and `puppetlabs-stdlib` will be cloned from github instead of the Forge. This feature allows you to host your modules in a private repository or in your company's self hosted git repository.

### Masterless Deployment

By using `librarian-puppet` you can have your a "masterless" deployment strategy for your roles.

In our *hotdog.com* example, the role has one private dependency:
1. The `wordpress` profile

And the `wordpress` profile has two public dependencies:
1. The `apache` module
2. And, the `php` module

If both `hotdogcom` role and and `wordpress` profile are using a `Puppetfile` to declare their dependencies, we can run the `librarian-puppet install` command, and `librarian-puppet` would take care of downloading all the necessary modules into your Puppet modules directory.

The `hotdogcom` role's Puppetfile might look like this:

```ruby
forge "https://forge.puppetlabs.com"

mod 'afriend-nagios', "1.1.0"
mod 'someguy-graphana', "0.0.3"

mod 'mycompany-wordpress',
  :git => "git://github.com/mycompany/mycompany-wordpress.git",
  :ref => "0.1.0"
```

And the `wordpress` role's like this:

```ruby
forge "https://forge.puppetlabs.com"

mod 'nerd-apache', "2.1.1"
mod 'geek-php', "4.1.3"
```

Deploying roles with `librarian-puppet` has a great advantage over the traditional Puppet Master approach. By using the `git flow` methodololy, combined with the dependency declaration of the Puppetfile, we can generate a new deployment scenario for both for our current module as with it's dependencies.

For example. If our `hotdogcom` role needed an upgrade from its `nagios` dependency, we could generate a feature branch and update our `Puppetfile` with the new experimental version of the `nagios` module. Then we can run the necessary tests and see how the host would be affected. And only when we are sure the tests are passing we then generate a new release of the role and roll it to production.

### Dependency Blast Radius

A side effect of versioning your dependencies, is that you can safely add features to your modules and not worry if a new release will break other modules. Since the client modules are using a specific version of your module, they will be unaware of any changes until they upgrade the version of your module.

Then they check if anything breaks, by generating a feature branch and running their tests.

There is a catch with this approach. When have a common module used by many clients, every time you release a new version, all clients must update to that version also. That means going to each client, generating a feature branch, testing, and generating a new release.

I believe the extra work is worth it, since it keeps your safe from a silly mistake to spill to all your client modules.


---

[1] Librarian Puppet: http://librarian-puppet.com/