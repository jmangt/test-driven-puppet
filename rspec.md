## Rspec

Take a look that the `Gemfile` that was generated with your module.

```bash
# /vagrant/users/Gemfile
source 'https://rubygems.org'

puppetversion = ENV.key?('PUPPET_VERSION') ? "#{ENV['PUPPET_VERSION']}" : ['>= 3.3']
gem 'puppet', puppetversion
gem 'puppetlabs_spec_helper', '>= 0.8.2'
gem 'puppet-lint', '>= 1.0.0'
gem 'facter', '>= 1.7.0'
```

Within the declared dependencies you will see the `puppetlabs-spec_helper` gem. This gem contains dependencies that will help us test our modules. Please take a look a the[ README](https://github.com/puppetlabs/puppetlabs_spec_helper) [1] file from the project's github repository.





---

[1] puppetlabs_spec_helper gem - This repository is meant to provide a single source of truth for how to initialize different Puppet versions for spec testing :https://github.com/puppetlabs/puppetlabs_spec_helper
