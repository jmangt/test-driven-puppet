## The users module

Create a new `users` module using the `puppet module generate myorg-users`.

We use the `myorg` prefix as a way to declare who's user module this is. This is required by the Forge in order to distinguish between two modules with the same name. For example: `bob-apache` and `mike-apache`.

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

### Setup Work Flow

Now that we have a scaffold for our module, let's setup our development work flow.

### RVM

Add a ruby version and gemset to your module. This will allow other developers using RVM to get an exact version of your development environment. I will use a Puppet 4.2.2 gemset running Ruby 2.2.1 for this module.

```bash
$ cd /vagrant/users
$ touch '2.2.1' > .ruby-version
$ touch 'puppet-4.2.2' > .ruby-gemset
```

### Bundler 

The `generate` command added a `Gemfile` for you in the root of the module.

```ruby
# /vagrant/users/Gemfile
source 'https://rubygems.org'

puppetversion = ENV.key?('PUPPET_VERSION') ? "#{ENV['PUPPET_VERSION']}" : ['>= 3.3']
gem 'puppet', puppetversion
gem 'puppetlabs_spec_helper', '>= 0.8.2'
gem 'puppet-lint', '>= 1.0.0'
gem 'facter', '>= 1.7.0'
```

This `Gemfile` leaves the option open for you to set the version of `Puppet` that your setup uses. Install the gems passing the PUPPET_VERSION variable with 4.2.2.

```bash
$ cd /vaggrant/users
$ PUPPET_VERSION=4.2.2 bundle install
```


### Git

Add version control to the new `users` module.

```bash
$ cd /vagrant/users
$ git init .
```

Add a `.gitignore` file to your project.

```bash
$ cd /vagrant/users
$ cat << EOF > .gitignore
.vagrant
Gemfile.lock
spec/fixtures
pkg/
.rspec_system/
*#*
*~*
log/
junit/
.librarian
modules
.tmp
Puppetfile.lock
EOF
```

Commit your changes
```bash
# Commit your changes
$ git add .
$ git commit -m "Initial commit"

# Add a remote to your repository.
$ git remote add origin https://github.com/jmangt/jmangt-users.git
```

Initialize Gitflow
```bash
# Initialize Gitflow
$ git flow init -d

$ git branch
* develop
  master
```


---

[1] I've hosted this sample module in my github.com account. Please feel free to clone or submit pull requests to it. https://github.com/jmangt/jmangt-users