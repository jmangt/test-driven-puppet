### Running Tests

If you were running a pure Ruby project your would run the `rspec` command in the root of your project to run your tests.

In our Rspec Puppet setup, the `puppet_spec_helper` gems provides various `rake tasks` that will help you run the tests.

The `rake` gem is Ruby's version of `make`. It is used to write simple tasks like database migrations, log or cache cleanup, and in our case running Puppet tests.

You can list all the available `rake` tasks by running the `rake help` command in your module's root.

```bash
$ cd /vagrant/users
$ rake help
rake beaker            # Run beaker acceptance tests
rake beaker_nodes      # List available beaker nodesets
rake build             # Build puppet module package
rake clean             # Clean a built module package
rake coverage          # Generate code coverage information
rake help              # Display the list of available rake tasks
rake lint              # Run puppet-lint
rake metadata          # Validate metadata.json file
rake spec              # Run spec tests in a clean fixtures directory
rake spec_clean        # Clean up the fixtures directory
rake spec_prep         # Create the fixtures directory
rake spec_standalone   # Run spec tests on an existing fixtures directory
rake syntax            # Syntax check Puppet manifests and templates
rake syntax:hiera      # Syntax check Hiera config files
rake syntax:manifests  # Syntax check Puppet manifests
rake syntax:templates  # Syntax check Puppet templates
rake validate          # Check syntax of Ruby files and call :syntax and :metadata / Validate manifests, templates, and ruby files
```

Must of the time we will use the `rake spec` command to run our module's tests.