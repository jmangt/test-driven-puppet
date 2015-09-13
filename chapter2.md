# Setting up your development environment

Rule number 1 of the Puppet Club:

**We don't write or test code in our production environment.**


---

There is no simple, one click solution for testing Puppet code. 

Here is a list of tools or methodologies you might need in your Puppet testing journey:

* [Vagrant](https://www.vagrantup.com/) [1]
* [RVM](https://rvm.io/) [2]
* [Bundler](http://bundler.io/) [3]
* [Git](https://git-scm.com/) [4]
* [Gitflow](http://nvie.com/posts/a-successful-git-branching-model/) [5]
* [Packer](https://www.packer.io/) [6]
* [Puppet Lint](http://puppet-lint.com/) [7]
* [Rspec](http://rspec.info/) [8]
* [Rspec Puppet](http://rspec-puppet.com/)
* Beaker
* Rspec Beaker

In the next section I will go over the first six. They will help you setup a testing system that is flexible an production like. The remaining four will be explored in depth since they are the tools that actually test your Puppet code.



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