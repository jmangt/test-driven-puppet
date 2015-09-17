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

### Rspec Syntax

Rspec uses a nice human readable syntax to describe your features. It follows commonly used convention for writing "user stories". 

The basic idea is that your declare A SUBJECT to describe. Then you present a CONTEXT where this SUBJECT is to be tested. Within the CONTEXT you DESCRIBE usage scenarios that your SUBJECT is exposed to. An finally you ASSERT if the SUBJECT responded as expected.

Rspec expects your to place your files under a `specs` directory directly in your projects root. And each test file has to be named in the `*_spec.rb` form. 

An example for describing a DOG would look like this:

```ruby
# dogs/spec/dog_spec.rb
require 'spec_helper'

describe 'Dog' do
  context 'when sleeping' do
    describe '#bark' do
      it 'should not bark' do
        expect(Dog.bark).to == nil
      end
    end
  end
  
  context 'when awake' do
    describe 'bark' do
      it 'should be loud' do
        expect(Dog.bark).to == 'WOOF!'
      end
    end
  end
end
```

[The Rspec documentation](https://relishapp.com/rspec/) [4] site is the resource to use when it comes to syntax and features. So I will not go into to much detail into it. 

Take some time to go through the ["Getting Started](https://relishapp.com/rspec/docs/gettingstarted)" [5] section of the site if you want to go deep on how to use Rspec with a Ruby project.

### Assertions

An assertion or expectation is the code that tests your SUBJECT's behavior. In the Dog example the `it do ... end` blocks are the ones that contain the assertion.

In the first scenario the assertion expects that the Dog won't bark if it is asleep. If the Dog's bark would have been something different from `nil` Rspec would have marked the test as failed.

We will use the same logic to test our Puppet modules. But the expected behavior will be checking if the intended Puppet resources are being correctly declared in the compiled manifest.I will go a bit deeper into this in following sections.


### Running specs

If you were running a pure Ruby project your would run the `rspec` command in the root of your project to run your tests.

In our Rspec Puppet setup, the `puppet_spec_helper` gems provides various `rake tasks` that will help you run the tests.

The `rake` gem is Ruby's version of `make`. It is used to write simple tasks like database migrations, log or cache cleanup, and in our case running Puppet tests.

You can list all the available `rake` tasks by running the `rake -vT` command in your module's root.

```bash
$ cd /vagrant/users
$ rake -vT
rake beaker            # Run beaker acceptance tests
rake beaker_nodes      # List available beaker nodesets
rake build             # Build puppet module package
rake clean             # Clean a built module package
rake coverage          # Generate code coverage information
rake help              # Display the list of available rake tasks
rake lint              # Run puppet-lint
rake metadata          # Validate metadata.json file
rake spec              # Run spec tests in a clean fixtures directory
rake spec_clean        # Clean up the fixtures directory
rake spec_prep         # Create the fixtures directory
rake spec_standalone   # Run spec tests on an existing fixtures directory
rake syntax            # Syntax check Puppet manifests and templates
rake syntax:hiera      # Syntax check Hiera config files
rake syntax:manifests  # Syntax check Puppet manifests
rake syntax:templates  # Syntax check Puppet templates
rake validate          # Check syntax of Ruby files and call :syntax and :metadata / Validate manifests, templates, and ruby files
```




---

[1] puppetlabs_spec_helper gem - This repository is meant to provide a single source of truth for how to initialize different Puppet versions for spec testing :https://github.com/puppetlabs/puppetlabs_spec_helper

[2] Rspec - Behaviour Driven Development for Ruby: http://rspec.info/

[3] Rspec-Puppet - RSpec tests for your Puppet manifests: http://rspec-puppet.com/

[4] Relish Rspec: https://relishapp.com/rspec/

[5] Rspec Getting Started Guide: https://relishapp.com/rspec/docs/gettingstarted
