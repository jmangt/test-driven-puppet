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

Within the declared dependencies you will see the `puppetlabs_spec_helper` gem. This gem contains dependencies that will help us test our modules. Please take a look a the[ README](https://github.com/puppetlabs/puppetlabs_spec_helper) [1] file from the project's github repository.

Inside the dependencies of `puppetlabs_spec_helper` is the `rspec` gem [2]. 

Rspec provides a way to describe and test Ruby code. And with the help of the `rspec-puppet` gem [3], we should be able to test the syntax, dependency and manifest resolution of our Puppet code.

---

[1] puppetlabs_spec_helper gem - This repository is meant to provide a single source of truth for how to initialize different Puppet versions for spec testing :https://github.com/puppetlabs/puppetlabs_spec_helper

[2] Rspec - Behaviour Driven Development for Ruby: http://rspec.info/

[3] Rspec-Puppet - RSpec tests for your Puppet manifests: http://rspec-puppet.com/