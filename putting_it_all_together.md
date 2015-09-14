# Putting it all together

We have gone trough a lot of tools and concepts in the last sections. The amount might seen overwhelming, but it will make sense when they all become part of your regular work flow.

How do all this pieces fit in our Test Driven work flow?

The following table shows where do all the tools fit.

| Tool | TDD Role |
| -- | -- |
| Vagrant | Provide a production like environment for developing and testing our modules. |
| RVM | Manage the version of Ruby our environment will use. Isolate the versions of gems that our module will use. Allows us to test our module with different versions of Puppet. |
| Bundler | Manage gem dependencies. Allows us to declare what version of Puppet or other development gems our module will use. |
| Git | Distributed source control. Gives us the ability to keep track of changes to our module. Easy to generate development branches and tag releases.|
| Gitflow | Branching methodology. Orders your development cycle. Allows us to collaborate with our team without deadlocking the production releases.  |

## Puppet testing

In the following sections we will learn about a different set of tools. These will take advantage of our setup tools to develop and test our Puppet modules.

| Tool | TDD Role |
| -- | -- |
| puppet-lint | Syntax and Style validation |
| librarian-puppet | Dependency management for Puppet modules |
| rspec, rspec-puppet | Class testing |
| beaker, beaker-rspec, serverspec | Acceptance testing |

