### Our first test

Take a look at the files `puppet module generate` created for you.

```bash
tree
.
├── Gemfile
├── Gemfile.lock
├── manifests
│   └── init.pp
├── metadata.json
├── Rakefile
├── README.md
├── spec
│   ├── classes
│   │   └── init_spec.rb
│   └── spec_helper.rb
└── tests
    └── init.pp

4 directories, 9 files
```

The `puppet module generate` command created a test place holders for your.

Take a look a the

Now this is a tricky one. Go ahead and run the default test that `puppet module generate` created for your.

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