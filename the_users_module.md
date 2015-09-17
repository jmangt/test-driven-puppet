## The users module

Create a new `users` module using the `puppet module generate myorg-users`.

We use the `myorg` prefix as a way to declare who's user module this is. This is required by the Forge in order to distinguish between two modules with the same name. For example: `bob-apache` and `mike-apache`.

Check your options by passing the `--help` flag.

```bash
$ puppet module generate --help

USAGE: puppet module generate [--skip-interview] <name>

Generates boilerplate for a new module by creating the directory
structure and files recommended for the Puppet community's best practices.

RETURNS: Array of Pathname objects representing paths of generated files.

OPTIONS:
  --render-as FORMAT             - The rendering format to use.
  --verbose                      - Whether to log verbosely.
  --debug                        - Whether to log debug information.
  --skip-interview               - Bypass the interactive metadata interview

See 'puppet man module' or 'man puppet-module' for full help.
```

Generate the module and answer the appropriate questions. [1]

```
$ cd /vagrant
$ puppet module generate jmangt-users
We need to create a metadata.json file for this module.  Please answer the
following questions; if the question is not applicable to this module, feel free
to leave it blank.

Puppet uses Semantic Versioning (semver.org) to version modules.
What version is this module?  [0.1.0]
-->

Who wrote this module?  [jmangt]
-->

What license does this module code fall under?  [Apache-2.0]
--> MIT

How would you describe this module in a single sentence?
--> It creates a list of default users

Where is this module's source code repository?
--> https://github.com/jmangt/jmangt-users

Where can others go to learn more about this module?  [https://github.com/jmangt/jmangt-users]
-->

Where can others go to file issues about this module?  [https://github.com/jmangt/jmangt-users/issues]
-->

----------------------------------------
{
  "name": "jmangt-users",
  "version": "0.1.0",
  "author": "jmangt",
  "summary": "It creates a list of default users",
  "license": "MIT",
  "source": "https://github.com/jmangt/jmangt-users",
  "project_page": "https://github.com/jmangt/jmangt-users",
  "issues_url": "https://github.com/jmangt/jmangt-users/issues",
  "dependencies": [
    {"name":"puppetlabs-stdlib","version_requirement":">= 1.0.0"}
  ]
}
----------------------------------------

About to generate this metadata; continue? [n/Y]
--> Y

Notice: Generating module at /vagrant/users...
Notice: Populating templates...
Finished; module generated in users.
users/Gemfile
users/manifests
users/manifests/init.pp
users/metadata.json
users/Rakefile
users/README.md
users/spec
users/spec/classes
users/spec/classes/init_spec.rb
users/spec/spec_helper.rb
users/tests
users/tests/init.pp
```

When the `generate` command finishes it's run your will have a new `users` module ready to be tested.

### Setup Work Flow

Now that we have a scaffold for our module, let's setup our development work flow.

### Git

Add version control to the new `users` module.

```bash
$ cd /vagrant/users
$ git init .

# Commit your changes
$ git add .
$ git commit -m "Initial commit"

# Add a remote to your repository.
$ git remote add origin https://github.com/jmangt/jmangt-users.git

# Initialize Gitflow
$ git flow init -d

$ git branch
* develop
  master
```

### RVM

Add a ruby version and gemset to your module. This will allow other developers using RVM to get an exact version of your development environment.

```bash
$ cd /vagrant/user
$ touch '
```
---

[1] I've hosted this sample module in my github.com account. Please feel free to clone or submit pull requests to it. https://github.com/jmangt/jmangt-users