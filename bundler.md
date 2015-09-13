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

Update and save your Gemfile to include puppet 4.2.1 as it's only dependency.

```ruby
# /home/vagrant/my-project/Gemfile

source "https://rubygems.org"

gem "puppet", "4.2.1"
```

## Install dependencies

Now that you have a valid `Gemfile` it is time to install your gems.

Run the `bundle install` command in the same directory that contains your `Gemfile`. Bundle will query the source you specified for the version of the gem you declared and will also find it's dependencies.

```bash
vagrant:$ cd ~/my-project
vagrant:$ ls
Gemfile

vagrant:$ bundle install
Fetching gem metadata from https://rubygems.org/...........
Fetching version metadata from https://rubygems.org/..
Resolving dependencies...
Installing CFPropertyList 2.2.8
Installing facter 2.4.4
Installing json_pure 1.8.2
Installing hiera 3.0.1
Installing puppet 4.2.1
Using bundler 1.8.4
Bundle complete! 1 Gemfile dependency, 6 gems now installed.
Use `bundle show [gemname]` to see where a bundled gem is installed.
```

You can see the gems installed by in your bundle by running `bundle show` in the directory that holds your `Gemfile`.

Notice that Bundler has created a new file named `Gemfile.lock`. This file contains the real manifest of the dependency tree. In it you will see a list of the gems you declared in the Gemfile, as well as their dependencies.

```bash
vagrant:$ cd ~/my-project
vagrant:$ cat Gemfile.lock
GEM
  remote: https://rubygems.org/
  specs:
    CFPropertyList (2.2.8)
    facter (2.4.4)
      CFPropertyList (~> 2.2.6)
    hiera (3.0.1)
      json_pure
    json_pure (1.8.2)
    puppet (4.2.1)
      facter (> 2.0, < 4)
      hiera (>= 2.0, < 4)
      json_pure

PLATFORMS
  ruby

DEPENDENCIES
  puppet (= 4.2.1)
```

## Updating dependencies

Use the `bundle update` command to install the latest version available of your declared gems.

### Word of caution

*Always declare the specific version of *each* gem your project requires. Leaving version numbers open can lead to many unfortunate incidents that tend to happen when you are short of time and sleep.*

If you will like to relax your version numbers I recommend using the `spermy operator ~>`. Wich will allow you to update to a higher patch version of your gem, but will not update to a minor one. This allows you to get the latest bug patches without accepting any new features that might break your code.

An updated version of your `Gemfile` might look like:

```ruby
# /home/vagrant/my-project/Gemfile

source "https://rubygems.org"

gem "puppet", "~>4.2.1"
```

Now you could run the `bundle update` command to get a new version of Puppet. Bundler will download and install any new version starting with `4.2.1` and ending with `4.2.99`. But you will never receive anything higher like `4.3` or `5`.

---

[1] Bundler - The best way to manage a Ruby application's gems: http://bundler.io/

[2] Gemfile: http://bundler.io/gemfile.html

[3][4] rubygems.org - Find, install, and publish RubyGems: https://rubygems.org/