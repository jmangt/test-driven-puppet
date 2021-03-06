## The users module

Create a new `users` module using the `puppet module generate myorg-users`.

We use the `myorg` prefix as a way to declare who owns this module. This is required by the Forge in order to distinguish between two modules with the same name. For example: `bob-apache` and `mike-apache`.

Check your options by passing the `--help` flag.

```bash
$ puppet module generate --help

USAGE: puppet module generate [--skip-interview] <name>

Generates boilerplate for a new module by creating the directory
structure and files recommended for the Puppet community's best practices.

RETURNS: Array of Pathname objects representing paths of generated files.

OPTIONS:
  --render-as FORMAT             - The rendering format to use.
  --verbose                      - Whether to log verbosely.
  --debug                        - Whether to log debug information.
  --skip-interview               - Bypass the interactive metadata interview

See 'puppet man module' or 'man puppet-module' for full help.
```

Generate the module and answer the appropriate questions. [1]

```
$ cd /vagrant
$ puppet module generate jmangt-users
We need to create a metadata.json file for this module.  Please answer the
following questions; if the question is not applicable to this module, feel free
to leave it blank.

Puppet uses Semantic Versioning (semver.org) to version modules.
What version is this module?  [0.1.0]
-->

Who wrote this module?  [jmangt]
-->

What license does this module code fall under?  [Apache-2.0]
--> MIT

How would you describe this module in a single sentence?
--> It creates a list of default users

Where is this module's source code repository?
--> https://github.com/jmangt/jmangt-users

Where can others go to learn more about this module?  [https://github.com/jmangt/jmangt-users]
-->

Where can others go to file issues about this module?  [https://github.com/jmangt/jmangt-users/issues]
-->

----------------------------------------
{
  "name": "jmangt-users",
  "version": "0.1.0",
  "author": "jmangt",
  "summary": "It creates a list of default users",
  "license": "MIT",
  "source": "https://github.com/jmangt/jmangt-users",
  "project_page": "https://github.com/jmangt/jmangt-users",
  "issues_url": "https://github.com/jmangt/jmangt-users/issues",
  "dependencies": [
    {"name":"puppetlabs-stdlib","version_requirement":">= 1.0.0"}
  ]
}
----------------------------------------

About to generate this metadata; continue? [n/Y]
--> Y

Notice: Generating module at /vagrant/users...
Notice: Populating templates...
Finished; module generated in users.
users/Gemfile
users/manifests
users/manifests/init.pp
users/metadata.json
users/Rakefile
users/README.md
users/spec
users/spec/classes
users/spec/classes/init_spec.rb
users/spec/spec_helper.rb
users/tests
users/tests/init.pp
```

When the `generate` command finishes it's run your will have a new `users` module ready to be tested.

### Directory Structure

Take a look at the files `puppet module generate` created for you.

```bash
$ cd /vagrant/users
$ tree
.
├── Gemfile
├── manifests
│   └── init.pp
├── metadata.json
├── Rakefile
├── README.md
├── spec
│   ├── classes
│   │   └── init_spec.rb
│   └── spec_helper.rb
└── tests
    └── init.pp

4 directories, 9 files
```

Here is a brief description of each of the files and directories your module contains. For a detail description of how a Puppet module is structured please read the ["Module Layout"](https://docs.puppetlabs.com/puppet/latest/reference/modules_fundamentals.html#module-layout)[2] section of the ["Module Fundamentals"](https://docs.puppetlabs.com/puppet/latest/reference/modules_fundamentals.html#module-fundamentals)[3] page in the Puppetlabs site.

#### The manifests directory

All your module files will be placed under the `manifests` directory. This is the code that it will be used by Puppet to provision your host.

If a file is placed outside of this directory it will not be available for Puppet to use.

#### The spec directory

Files under the `spec` directory are used by Rspec to run your tests. 

Different types of resources will have different directories where the tests should be placed. For example:

| Resource | Directory |
| -- | -- |
| class | spec/classes |
| define | spec/defines |
| host | spec/hosts |
| function | spec/functions |


##### The spec/classes directory

Inside the `spec/classes` is where we place all our test for Puppet classes.

In order for Rspec to use these files as test files their name MUST end with `*_spec.rb`. Any other file will be ignored by Rspec.

##### spec_helper.rb

The `spec_helper.rb` file holds Rspec configuration. In it you can specify where to find the different files your module needs to run. For example:

```ruby
require 'rspec-puppet'

fixture_path = File.expand_path(File.join(__FILE__, '..', 'fixtures'))

RSpec.configure do |c|
  # The path to the directory containing your modules.
  c.module_path = File.join(fixture_path, 'modules')
  
  # The path to the directory containing your basic manifests like site.pp.
  c.manifest_dir = File.join(fixture_path, 'manifests') 
  
  # The entry-point manifest for Puppet, usually <manifest_dir>/site.pp.
  c.manifest = File.join(fixture_path, 'manifests', 'default.pp')
  
  # The path to the directory where Puppet will look for templates outside of modules.
  c.template_dir = File.join(fixture_path, 'template')
  
  # The path to the puppet.conf.
  c.config = 'puppet.conf'
  
  # The path to the main Puppet configuration directory.
  c.confdir = './config'
  
  # A hash of default facts that should be used for all your tests.
  c.default_facts = {server_env:      'qa', 
                     tag_app_version: '0.1.0', 
                     fqdn:            'test.hotdog.com'}

  # The path to your hiera.yaml file, if you use Hiera.
  c.hiera_config = './hiera.yaml'
end
```

Must of the time the default values will just work for you.

#### The tests directory

The Puppetlabs site reads:
> **Deprecation Warning**: The tests directory is being DEPRECATED in favor of the examples directory. If you use puppet module generate to create your module skeleton, please rename your directory to examples.

The idea behind the tests (examples) directory is to have a way to 'live' test your module. Not a Test Driven kind of test, but more of a 'smoke' kind of test. 

You can read more on smoke testing on the "[Module Smoke Testing](http://docs.puppetlabs.com/guides/tests_smoke.html)" section of the Puppetlabs site.

In a nutshell. You add examples on how to use your module in a `test/init.pp` file. This file can then be run with the `puppet apply --noop` command to check for any compilation errors your module might have.

If you take a look a the `test/init.pp` file provided by `puppet modue generate` you will see a simple `include users` command in it. Which is a very simple way to run your module with all default parameters your `manifest/init.pp` might provide.


#### Gemfile

The `Gemfile` provides a manifest of development dependencies for your module. 

Here you declare what version of Puppet do you want to run your tests with, or any other tools you might way to include. 

The default `Gemfile` includes:

* `puppet`
* `facter`
* `puppetlabs_spec_helper`. Which contains `rspec` and other test oriented gems.
* `puppet-lint`. For checking if your module complies with the style guidelines set by the Puppet community.

```ruby
# /vagrant/users/Gemfile
source 'https://rubygems.org'

puppetversion = ENV.key?('PUPPET_VERSION') ? "#{ENV['PUPPET_VERSION']}" : ['>= 3.3']
gem 'puppet', puppetversion
gem 'puppetlabs_spec_helper', '>= 0.8.2'
gem 'puppet-lint', '>= 1.0.0'
gem 'facter', '>= 1.7.0'
```

#### metadata.json

The `metadata.json` file declares the module's name, version, dependencies and restrictions. It is used by the Puppet Forge to index public modules and define dependencies between them.

You can read more on it's structure and purpose in the ["Distributing Puppet Modules"](chapter4.md) chapter of this book.


#### Rakefile

The `Rakefile` is a Ruby file that serves as an entrypoing to running `rake tasks`. 

Rake is a Make-like program implemented in Ruby. For more information on `rake` your can read the [README](https://github.com/ruby/rake) file in the project's github repository [4].

Rake is a powerful tool since it allows your to automate repetive tasks in an orderly way. Please read [Rake's documentation](http://docs.seattlerb.org/rake/) if you want to take advantage of it's features [5].

---

[1] I've hosted this sample module in my github.com account. Please feel free to clone or submit pull requests to it. https://github.com/jmangt/jmangt-users

[2] Puppet Module Layout: https://docs.puppetlabs.com/puppet/latest/reference/modules_fundamentals.html#module-layout

[3] Module Fundamentals: https://docs.puppetlabs.com/puppet/latest/reference/modules_fundamentals.html#module-fundamentals

[4] Rake: https://github.com/ruby/rake.

[5] Rake Documentation: http://docs.seattlerb.org/rake/