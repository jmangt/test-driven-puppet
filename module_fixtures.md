### Fixtures

When you ran `puppet module generate`, the command generated a scaffold for your test files.

At this point you are *almost* ready for running your first test. But as usual there is always that little thing that nobody told you about.

Go ahead and run your tests by using the `rake spec` command.

```bash
$ cd /vagrant/users
$ rake spec
rake spec
/home/vagrant/.rvm/rubies/ruby-2.2.1/bin/ruby -I/home/vagrant/.rvm/gems/ruby-2.2.1@puppet-4.2.2/gems/rspec-support-3.3.0/lib:/home/vagrant/.rvm/gems/ruby-2.2.1@puppet-4.2.2/gems/rspec-core-3.3.2/lib /home/vagrant/.rvm/gems/ruby-2.2.1@puppet-4.2.2/gems/rspec-core-3.3.2/exe/rspec --pattern spec/\{classes,defines,unit,functions,hosts,integration\}/\*\*/\*_spec.rb --color
F

Failures:

  1) users with defaults for all parameters should contain Class[users]
     Failure/Error: it { should contain_class('users') }
     Puppet::PreformattedError:
       Evaluation Error: Error while evaluating a Function Call, Could not find class ::users for vagrant-ubuntu-trusty-64.eau.wi.charter.com at line 1:1 on node vagrant-ubuntu-trusty-64.eau.wi.charter.com
     # /home/vagrant/.rvm/gems/ruby-2.2.1@puppet-4.2.2/gems/puppet-4.2.2/lib/puppet/parser/compiler.rb:207:in `block in evaluate_classes'
     # /home/vagrant/.rvm/gems/ruby-2.2.1@puppet-4.2.2/gems/puppet-4.2.2/lib/puppet/parser/compiler.rb:206:in `collect'
     ....
     Finished in 0.07654 seconds (files took 0.86197 seconds to load)
1 example, 1 failure

Failed examples:

rspec ./spec/classes/init_spec.rb:5 # users with defaults for all parameters should contain Class[users]
```

If you are seeing an error like this, pay attention to the debug line that reads:

`Evaluation Error: Error while evaluating a Function Call, Could not find class ::users for`

This means that `rspec-puppet` was unable to find your module's code or one of its dependencies.

To understand what this means you need to take a look at your module's directory structure. 

```bash
$ cd ~/vagrant/users
$ tree -d
.
├── manifests
├── spec
│   ├── classes
│   └── fixtures
│       ├── manifests
│       └── modules
└── tests
```

Inside the `users/spec` directory you will see a `fixtures` directory. Rspec uses the fixtures directory to "deploy" your module's code and its dependencies. Rspec expects that your module's code be placed inside the `spec/fixtures/modules/` directory. 

In the case of the `users` module, the module's code should be inside the `spec/fixtures/modules/users` for Rspec to find it.

If the module had any other dependencies, those would need to be placed under the same `spec/fixtures/modules/[depedency]` directory.

Since managing this directory structure might cause "confusion and delay", `rpec-puppet` provides an easy way to declare these depedencies using the `.fixtures.yml` file.

### The .fixtures.yml file

The `.fixtures.yml` file is a manifest of your module's depedendencies, used for running `rspec` tests.

The `.fixtures.yml` file should be placed in the root or you module. **REMEMBER THE DOT AT THE START OF THE FILE NAME**.

Create a `.fixtures.yml` file in the root of your module.

```bash
$ cd ~/vagrant/users
$ cat <<EOF > .fixtures.yml
fixtures:
  repositories:
    'stdlib':
      repo: 'https://github.com/puppetlabs/puppetlabs-stdlib.git'
      ref: '4.2.0'
  symlinks:
    'users': '#{source_dir}'
EOF
```

The `.fixtures.yml` file has two parts. The first is the `repositories` branch. This branch declares the name of your dependency, where to pull the code for that depedency and from what point in history to pull the code.

In our example we are adding the Puppet Labs `stdlib` module as a dependency for our module. We are declaring that is going to use the projects github repository to pull the code. And that it would pull from the `4.2.0` git tag. **It is very important that the name of the dependency matches the module's name**. If you don't, Rspec won't be able to find it.

The second part is the `symlinks` branch. Here we specify how our module's code finds it's way into the `spec/fixtures/modules` directory. `rspec-puppet` will create a symlink from our `source_dir` into the `spec/fixtures/modules/MODULE` directory.







