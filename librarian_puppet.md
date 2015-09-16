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

### Masterless Deployment

By using `librarian-puppet` you can have your a "masterless" deployment strategy for your roles.

In our *hotdog.com* example, the role has one private dependency:
1. The `wordpress` profile

And the `wordpress` profile has two public dependencies:
1. The `apache` module
2. And, the `php` module

If both `hotdogcom` role and and `wordpress` profile are using a `Puppetfile` to declare their dependencies, we can run the `librarian-puppet install` command, and `librarian-puppet` would take care of downloading all the necessary modules into your Puppet modules directory.

Deploying roles with `librarian-puppet` has a great advantage over the traditional Puppet Master approach. By using the `git flow` methodololy, combined with the dependency declaration of the Puppetfile, we can generate a new deployment scenario for both for our current module as with it's dependencies.

For example. If our `hotdogcom` role needed an upgrade from its `nagios` dependency, we could generate a feature branch and update our `Puppetfile` with the new experimental version of the `nagios` module. Then we can run the necessary tests and see how the host would be affected.


---

[1] Librarian Puppet: http://librarian-puppet.com/