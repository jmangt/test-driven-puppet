# Bundler

Puppet is just the fisrt dependency of our future modules. As we continue our journey to a Test Driven work flow, other gems will come into play.

I we want to manage these development dependencies in an orderly way we need a way to declare them.

[Bundler](http://bundler.io/) is gem by itself. It's job is to manage the tricky business of gem dependencies. Nowadays it is the Ruby Community's default way of managing dependencies.

## Installing Bundler

Pick your favorite gemset and install it by running the `gem install GEMNAME` command.

```bash
vagrant:$ rvm use 2.2.1
vagrant:$ gem install bundler

Fetching: bundler-1.10.6.gem (100%)
Successfully installed bundler-1.10.6
Parsing documentation for bundler-1.10.6
Installing ri documentation for bundler-1.10.6
Done installing documentation for bundler after 3 seconds
1 gem installed
```

## Gemfile

A `[Gemfile](http://bundler.io/gemfile.html)` [2] is Bundler's way of declaring your project's dependencies.

Select your RVM manged project and add a default `Gemfile` by running the `bundle init` command. 

```bash
vagrant:$ mkdir ~/my-project
vagrant:$ echo '2.2.1' > ~/my-project/.ruby-version
vagrant:$ echo 'puppet-4.2.1' > ~/my-project/.ruby-gemset
vagrant:$ cd ~/my-project
vagrant:$ bundle init
Writing new Gemfile to /home/vagrant/my-project/Gemfile
```

This is how your default Gemfile looks like:

```ruby
# /home/vagrant/my-project/Gemfile

# A sample Gemfile
source "https://rubygems.org"

# gem "rails"
```

There are two main sections in a Gemfile. 

First is the `source`. This is a URL where the gems that your project needs are hosted. By default gems are hosted in the public [rubygems.org](https://rubygems.org/) [3] repository. But you can change this to a private repository if you need to.

The second section is the gems declaration section. In here you will write the name of your gems, the version required by them. Bundler will take care of determining the dependency tree for all the declared gems and their dependencies.

Here is an example taken from the Bundler site on how to declare gem versions.

```ruby
gem 'nokogiri'
gem 'rails', '3.0.0.beta3'
gem 'rack',  '>=1.0'
gem 'thin',  '~>1.1'
```

*Most of the version specifiers, like >= 1.0, are self-explanatory. The specifier ~> has a special meaning, best shown by example. ~> 2.0.3 is identical to >= 2.0.3 and < 2.1. ~> 2.1 is identical to >= 2.1 and < 3.0. ~> 2.2.beta will match prerelease versions like 2.2.beta.12. * [4]

Update your Gemfile to include puppet 3.7.4 as it's only dependency.

```ruby
# /home/vagrant/my-project/Gemfile

source "https://rubygems.org"

gem "puppet", "3.7.4"
```


Manage dependencies

Sharing a project with your team

Setup a project



---

[1] Bundler - The best way to manage a Ruby application's gems: http://bundler.io/

[2] Gemfile: http://bundler.io/gemfile.html

[3][4] rubygems.org - Find, install, and publish RubyGems: https://rubygems.org/