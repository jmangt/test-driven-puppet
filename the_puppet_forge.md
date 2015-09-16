## The Puppet Forge

The "[Forge](https://forge.puppetlabs.com/)"[1] is the place where all publicly available modules are hosted.

Publishing your own modules is "easy", as long as you follow their conventions. You can read on how to organize and publish your modules in the "[Publishing Modules on the Puppet Forge](https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html)"[2] section of the Puppetlabs site.

### Module definition and dependencies

The way that the Forge declares a module and it dependencies is the [`metadata.json](https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html#write-a-metadatajson-file)`[3] file. 

This `json` file holds important information on:
* What is the name of the module
* It's version
* Where the module is hosted
* The operating systems your module supports
* And, the module's dependencies

The `metadata.json` file must be placed in the root of the project otherwise The Forge won't be able to find it.

This is an example of an `metadata.json`:

```json
{
  "name": "examplecorp-mymodule",
  "version": "0.0.1",
  "author": "Pat",
  "license": "Apache-2.0",
  "summary": "A module for a thing",
  "source": "https://github.com/examplecorp/examplecorp-mymodule",
  "project_page": "https://forge.puppetlabs.com/examplecorp/mymodule",
  "issues_url": "https://github.com/examplecorp/examplecorp-mymodule/issues",
  "tags": ["things", "stuff"],
  "operatingsystem_support": [
    {
    "operatingsystem":"RedHat",
    "operatingsystemrelease":[ "5.0", "6.0" ]
    },
    {
    "operatingsystem": "Ubuntu",
    "operatingsystemrelease": [ "12.04", "10.04" ]
    }
   ],
  "dependencies": [
    { "name": "puppetlabs/stdlib", "version_requirement": ">=3.2.0 <5.0.0" },
    { "name": "puppetlabs/firewall", "version_requirement": ">= 0.0.4" }
  ]
}
```

All the dependencies declared within the `metadata.json` are modules hosted in the Forge. If your dependencies are not publicly available then *you can not declare them here*. 

If you have private modules that you are unable to publish in the Forge, [Librarian-puppet](http://librarian-puppet.com/)[4] provides an alternative to manage them.

### Generating a metadata.json file

You can add a `metadata.json` file to your module in any moment. But if you want to be in accordance to the Forge's standard I would recommend using the `bash puppet module generate` command [5].

The command will generate a complete scaffold for your module.** I will even add some place holders for your tests! **

The previous `metadata.json` file was generated in the following way:

```bash
$ puppet module generate examplecorp-mymodule

We need to create a metadata.json file for this module.  Please answer the
following questions; if the question is not applicable to this module, feel free
to leave it blank.

Puppet uses Semantic Versioning (semver.org) to version modules.
What version is this module?  [0.1.0]
--> 0.1.0

Who wrote this module?  [examplecorp]
--> Pat

What license does this module code fall under?  [Apache 2.0]
--> Apache 2.0

How would you describe this module in a single sentence?
--> It examples with Puppet.

Where is this module's source code repository?
--> https://github.com/examplecorp/examplecorp-mymodule

Where can others go to learn more about this module?
--> https://forge.puppetlabs.com/examplecorp/mymodule

Where can others go to file issues about this module?
-->


{
  "name": "examplecorp-mymodule",
  "version": "0.1.0",
  "author": "Pat",
  "summary": "It examples with Puppet.",
  "license": "Apache 2.0",
  "source": "https://github.com/examplecorp/examplecorp-mymodule",
  "project_page": "(https://forge.puppetlabs.com/examplecorp/mymodule)",
  "issues_url": null,
  "dependencies": [
    {
      "name": "puppetlabs-stdlib",
      "version_range": ">= 1.0.0"
    }
  ]
}


About to generate this metadata; continue? [n/Y]
--> Y

Notice: Generating module at /Users/Pat/Development/mymodule...
Notice: Populating ERB templates...
Finished; module generated in mymodule.
mymodule/manifests
mymodule/manifests/init.pp
mymodule/metadata.json
mymodule/Rakefile
mymodule/README.md
mymodule/spec
mymodule/spec/classes
mymodule/spec/classes/init_spec.rb
mymodule/spec/spec_helper.rb
mymodule/tests
mymodule/tests/init.pp
```

---

[1] A repository of modules written by our community for Puppet Open Source and Puppet Enterprise IT automation software:  https://forge.puppetlabs.com/

[2] Publishing Modules on the Puppet Forge: https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html

[3] Write a metadata.json File: https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html#write-a-metadatajson-file

[4] librarian-puppet - You can all stop using git submodules now: http://librarian-puppet.com/

[5] Writing Modules: https://docs.puppetlabs.com/puppet/latest/reference/modules_fundamentals.html#writing-modules