## Development Setup

Now that we have a scaffold for our module, let's setup our development work flow.

### RVM

Add a ruby version and gemset to your module. This will allow other developers using RVM to get an exact version of your development environment. I will use a Puppet 4.2.2 gemset running Ruby 2.2.1 for this module.

```bash
$ cd /vagrant/users
$ echo '2.2.1' > .ruby-version
$ echo 'puppet-4.2.2' > .ruby-gemset
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

$ bundle show
Gems included by the bundle:
  * CFPropertyList (2.2.8)
  * bundler (1.8.4)
  * diff-lcs (1.2.5)
  * facter (2.4.4)
  * hiera (3.0.1)
  * json_pure (1.8.2)
  * metaclass (0.0.4)
  * mocha (1.1.0)
  * puppet (4.2.2)
  * puppet-lint (1.1.0)
  * puppet-syntax (2.0.0)
  * puppetlabs_spec_helper (0.10.3)
  * rake (10.4.2)
  * rspec (3.3.0)
  * rspec-core (3.3.2)
  * rspec-expectations (3.3.1)
  * rspec-mocks (3.3.2)
  * rspec-puppet (2.2.0)
  * rspec-support (3.3.0)
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
$ cd /vagrant/users
$ git flow init -d

$ git branch
* develop
  master
  
$ git add .
$ git commit -am "Initialized Gitflow"
```
