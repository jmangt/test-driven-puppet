## Librarian Puppet

"[Librarian-puppet](http://librarian-puppet.com/) is a bundler for your puppet infrastructure. You can use librarian-puppet to manage the puppet modules your infrastructure depends on, whether the modules come from the Puppet Forge, Git repositories or a just a path." [1]

Just like the description reads, `librarian-puppet` is a gem that uses a manifest file (Puppetfile) to list all your module's private an public dependencies. And it uses this file to download and install the module's dependencies.

`librarian-puppet` will do the work of resolving the dependency tree of your module, by finding your dependency's dependencies and determining the minimum acceptable version for each.







---

[1] Librarian Puppet: http://librarian-puppet.com/