### Masterless Deployment

By using `librarian-puppet` you can have your a "masterless" deployment strategy for your roles.

In our *hotdog.com* example, the role has one private dependency:
1. The `wordpress` profile

And the `wordpress` profile has two public dependencies:
1. The `apache` module
2. And, the `php` module

If both `hotdogcom` role and and `wordpress` profile are using a `Puppetfile` to declare their dependencies, we can run the `librarian-puppet install` command, and `librarian-puppet` would take care of downloading all the necessary modules into your Puppet modules directory.

The `hotdogcom` role's Puppetfile might look like this:

```ruby
forge "https://forge.puppetlabs.com"

mod 'afriend-nagios', "1.1.0"
mod 'someguy-graphana', "0.0.3"

mod 'mycompany-wordpress',
  :git => "git://github.com/mycompany/mycompany-wordpress.git",
  :ref => "0.1.0"
```

And the `wordpress` role's like this:

```ruby
forge "https://forge.puppetlabs.com"

mod 'nerd-apache', "2.1.1"
mod 'geek-php', "4.1.3"
```

Deploying roles with `librarian-puppet` has a great advantage over the traditional Puppet Master approach. By using the `git flow` methodololy, combined with the dependency declaration of the Puppetfile, we can generate a new deployment scenario for both for our current module as with it's dependencies.

For example. If our `hotdogcom` role needed an upgrade from its `nagios` dependency, we could generate a feature branch and update our `Puppetfile` with the new experimental version of the `nagios` module. Then we can run the necessary tests and see how the host would be affected. And only when we are sure the tests are passing we then generate a new release of the role and roll it to production.