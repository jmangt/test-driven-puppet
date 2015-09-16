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






---

[1] Librarian Puppet: http://librarian-puppet.com/