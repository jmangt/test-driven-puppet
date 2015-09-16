## Librarian Puppet

"[Librarian-puppet](http://librarian-puppet.com/) is a bundler for your puppet infrastructure. You can use librarian-puppet to manage the puppet modules your infrastructure depends on, whether the modules come from the Puppet Forge, Git repositories or a just a path." [1]

Just like the description reads, `librarian-puppet` is a gem that uses a manifest file (Puppetfile) to list all your module's private an public dependencies. And it uses this file to download and install the module's dependencies.

`librarian-puppet` will do the work of resolving the dependency tree of your module. That means finding your dependency's dependencies and determining the minimum acceptable version for each.

By using `librarian-puppet` you can have your own "masterless" deployment for your roles.

In our *hotdog.com* example, the role has one private dependency:
1. The `wordpress` profile

And the `wordpress` profile needed public dependencies:
1. The `apache` module
2. And, the `php` module

If both `hotdogcom` role and and `wordpress` profile are using a `Puppetfile` to declare their dependencies, we can run the `librarian-puppet install` command, and `librarian-puppet` would take care of downloading all the necessary modules into your Puppet modules directory.

Deploying roles using `librarian-puppet` instead of the common approach of using a Puppet master has a great advantage. You can use the `git flow` methodololy of branching and generate development branches. 

In these branches you can switch your dependencies to a specif version or development branch of their own. Then using the updated `Puppetfile` in branch you can test how your host will provision with that particular configuration.

---

[1] Librarian Puppet: http://librarian-puppet.com/