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

The file declares both public and private dependencies of your module.

In it you can declare a specific version of your dependency or an acceptable range of versions. 

It is also possible to declare a different source from the Forge for your module. In the example, `puppetlabs-apt` and `puppetlabs-stdlib` will be cloned from github instead of the Forge. This feature allows you to host your modules in a private repository or in your company's self hosted git repository.
