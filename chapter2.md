# Setting up your development environment

Rule number one of the Puppet Club:

**We don't write or test code in our production environment.**


---

There is no simple, one click solution for testing Puppet code. Testing Puppet code requires a collection of tools and methodologies to get the job done.

Here is a list of what you might need in your Puppet testing journey:

* [Vagrant](https://www.vagrantup.com/) [1]
* [RVM](https://rvm.io/) [2]
* [Bundler](http://bundler.io/) [3]
* [Git](https://git-scm.com/) [4]
* [Gitflow](http://nvie.com/posts/a-successful-git-branching-model/) [5]
* [Packer](https://www.packer.io/) [6]
* [Puppet Lint](http://puppet-lint.com/) [7]
* [Rspec](http://rspec.info/) [8]
* [Rspec Puppet](http://rspec-puppet.com/) [9]
* [Beaker](https://github.com/puppetlabs/beaker) [10]
* [Rspec Beaker](https://github.com/puppetlabs/beaker-rspec) [11]
* [Serverspec](http://serverspec.org/) [12]

In the next section I will present the first six. I will give you a brief rundown of what they can do and how they tie to our work flow. 
I will talk about the remaining in following chapters since they are the tools that actually test your Puppet code.



---

[1] Vagrant: https://www.vagrantup.com/

[2] RVM: https://rvm.io/

[3] Bundler: http://bundler.io/

[4] Git - fast version control: https://git-scm.com/

[5] 
A successful Git branching model: http://nvie.com/posts/a-successful-git-branching-model/

[6] Packer: https://www.packer.io/

[7] Puppet Lint - Check that your Puppet manifest conform to the style guide: http://puppet-lint.com/

[8] Rspec - Behaviour Driven
Development for Ruby.
Making TDD Productive and Fun.: http://rspec.info/

[9] Rspec-Puppet - RSpec tests for your Puppet manifests: http://rspec-puppet.com/

[10] Puppetlabs Beaker - Puppet Acceptance Testing Harness: https://github.com/puppetlabs/beaker

[11] Puppetlabs Rspec Beaker - Bridge between puppet test harness (beaker) and rspec: https://github.com/puppetlabs/beaker-rspec